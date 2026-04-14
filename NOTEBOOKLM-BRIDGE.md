---
type: procedure
created: 2026-03-17
status: active
slug: notebooklm-bridge
tags: [procedure, nexus]
---

# NOTEBOOKLM-BRIDGE — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-15
**Versiune**: 1.0
**Regulă asociată**: INGEST-H-001 (orice conținut ingerat TREBUIE să producă o intrare Cortex de calitate)
**Scope**: Îmbogățește automat videoclipurile YouTube pentru care pipeline-ul curent eșuează (HTTP 429 sau fără subtitrări) folosind NotebookLM MCP ca tier secundar de procesare.
**Gemini Audit**: CONDITIONAL 3.17/4.0 (2 HIGH + 2 MEDIUM rezolvate în această versiune)

---

## 1. Problema

Pipeline-ul curent de ingestare YouTube (`nexus-unified.sh` + `ingest-25-targeted.py`) produce intrări Cortex de calitate slabă în 40-50% din cazuri din două motive confirmate:

1. **HTTP 429 rate-limiting** pe descărcarea subtitrărilor YouTube — frequent în batch-uri mari, logat în `~/.nexus/logs/liked-ingestion.log`
2. **Lipsă subtitrări auto** — 40-50% din videoclipuri nu au captions disponibile → fallback la titlu+descriere only (fără insights)

**Consecință**: Cortex conține mii de intrări YouTube cu `insights: ""` sau insights generate doar din titlu (2-3 rânduri). Daily brief și raportul BI nu au conținut real din aceste videoclipuri.

**NotebookLM** rezolvă ambele probleme: procesează audio direct (nu depinde de YouTube subtitle API), nu are rate-limiting extern, și produce output structurat (raport markdown + mind map + Q&A).

**Integrare MCP completă disponibilă**: `notebook_add_url` → `notebook_query` → `report_create` → `studio_poll` → `notebook_delete` — toate automatizabile.

---

## 2. Procedura

### Pas 1: Detectare și flagging în `nexus-unified.sh`

Când download-ul de subtitrări eșuează (HTTP 429, exit code non-zero, sau transcript gol):

- Setează câmpul `notebooklm_enrich: true` în JSON-ul itemului ÎNAINTE de pasarea la `ingest.py`
- Implementare: `jq '. + {"notebooklm_enrich": true}' "$item_json" > "$item_json.tmp" && mv "$item_json.tmp" "$item_json"` (atomic, nu bash string mutation)
- Logează: `[WARN] transcript unavailable for {url} — flagged for NotebookLM enrichment`

### Pas 2: Schema `monitoring-db.json` — câmpuri noi

Adaugă în `ingest.py:item_to_schema()` (după câmpurile `source_channel`/`channel_id` din m4-500):

```python
"notebooklm_enrich":     bool(item.get("notebooklm_enrich", False)),
"notebooklm_enriched_at": str(item.get("notebooklm_enriched_at", "")),
"notebooklm_report":     str(item.get("notebooklm_report", "")),
```

### Pas 3: `notebooklm-bridge.py` — script de îmbogățire

Fișier nou: `~/.nexus/scripts/notebooklm-bridge.py`

**Logica principală** (cu fix-urile Gemini HIGH rezolvate):

```
load monitoring-db.json cu flock (filelock sau fcntl.flock — previne race condition cu nexus-unified.sh și alte procese care scriu în DB)

candidați = [item for item in db if item.notebooklm_enrich=true AND item.notebooklm_enriched_at=""]
procesați = candidați[:MAX_ENRICH_PER_RUN]  # max 10 per run

pentru fiecare item:
  1. notebook_id = mcp.notebook_create(title=f"NexusOS-{item['title'][:50]}-{timestamp}")

  2. mcp.notebook_add_url(notebook_id, item['url'])
     sleep(30)  # wait for source processing (NotebookLM async)

  3. query_result = mcp.notebook_query(notebook_id, ENRICHMENT_PROMPT)
     # ENRICHMENT_PROMPT (structurat — fix Gemini MEDIUM M2):
     # "Extract and format as JSON with keys:
     #   key_insights: [max 5 bullet strings],
     #   main_takeaways: [max 3 strings],
     #   action_items: [max 3 strings],
     #   notable_quotes: [max 2 strings].
     #   Keep each item under 100 words. Return only valid JSON."

  4. studio_id = mcp.report_create(notebook_id)
     # FIX Gemini HIGH H1: captează studio_id returnat de report_create
     # NU pasează notebook_id la studio_poll

     report_content = ""
     for attempt in range(20):  # max 10 min (20 × 30s)
       status = mcp.studio_poll(studio_id)
       if status.complete: report_content = status.content; break
       sleep(30)
     # dacă timeout → folosește doar query_result, nu bloca procesarea

  5. Salvează în Cortex:
     text = f"{query_result}\n\n{report_content}"
     collection = "intelligence"
     metadata: type="youtube_enriched", source_channel=item.source_channel,
               url=item.url, notebooklm=true, enriched_at=now_iso

  6. Actualizează Notion page dacă item.notion_id există:
     append block: "## NotebookLM Enrichment\n{query_result summary}"

  7. Marchează item în monitoring-db: notebooklm_enriched_at=now, notebooklm_enrich=false

  8. mcp.notebook_delete(notebook_id)  # cleanup — 1 notebook per video, fără acumulare

atomic write monitoring-db.json (cu flock menținut pe toată durata operațiunii)
log run summary: "[BRIDGE] {N} videos enriched, {M} failed"
Cortex log: type="notebooklm_bridge_run", enriched=N, failed=M
```

**Constante**:
```python
MAX_ENRICH_PER_RUN = 10      # previne depășirea timeout-ului LaunchAgent
STUDIO_POLL_MAX_ATTEMPTS = 20 # 20 × 30s = 10 minute max per report
NOTEBOOK_ADD_WAIT_S = 30      # wait după add_url pentru procesare NLM
```

**File locking** (fix Gemini HIGH H2 — race condition):
```python
import fcntl
with open(MONITORING_DB_PATH, 'r+') as f:
    fcntl.flock(f, fcntl.LOCK_EX)
    # read + modify + write
    fcntl.flock(f, fcntl.LOCK_UN)
```

### Pas 4: LaunchAgent `com.nexus.notebooklm-bridge.plist`

- Rulează: **07:00 zilnic** (după feed-monitor 06:00 care poate genera noi 429)
- `PATH=/opt/homebrew/bin:/usr/local/bin:/usr/bin:/bin`
- Log: `~/.nexus/logs/notebooklm-bridge.log`
- `RunAtLoad: false`

### Pas 5: `notion-sync.py` — proprietăți noi

Adaugă în `build_page_properties()`:

```python
if item.get("notebooklm_enriched_at"):
    props["NotebookLMEnriched"] = {"checkbox": True}
if item.get("notebooklm_report"):
    props["NotebookLMReport"] = {"url": item["notebooklm_report"]}
```

**Notion Intel Hub DB** — adaugă manual (2 click-uri):
- `NotebookLMEnriched` — tip Checkbox
- `NotebookLMReport` — tip URL

---

### Test Cases

1. **Normal flow (429 fallback)**: `nexus-unified.sh` hits 429 → `notebooklm_enrich: true` setat → la 07:00 bridge procesează → Cortex primește intrare cu `notebooklm: true` → Notion page actualizată → `notebooklm_enriched_at` setat → al doilea run skip (idempotent)

2. **Normal flow (manual flag)**: Pafi setează `notebooklm_enrich: true` pe un item strategic → bridge îl procesează la 07:00 → report complet generat

3. **Edge case — timeout report**: `studio_poll` nu completează în 10 minute → bridge folosește `query_result` (fără report), marchează `enriched_at` → nu retry (NLM a procesat ce a putut)

4. **Edge case — NotebookLM unavailable**: `notebook_create` eșuează → item rămâne `notebooklm_enrich: true` → retry automat la run următor — **zero pierdere de date**

5. **Idempotency**: bridge rulat de 2 ori → al doilea run găsește `notebooklm_enrich=true AND enriched_at=""` = 0 items → exit curat, zero duplicate Cortex entries

6. **Race condition test**: `nexus-unified.sh` scrie în `monitoring-db.json` în timp ce bridge citește → `flock` blochează accesul concurrent → operațiunea cu lock câștigă, cealaltă așteaptă → zero corupție

---

## 3. Cortex Logging

La fiecare run bridge:
```json
{
  "text": "notebooklm-bridge run: {N} enriched, {M} failed, {K} pending remaining",
  "collection": "intelligence",
  "metadata": {
    "type": "notebooklm_bridge_run",
    "procedure": "NOTEBOOKLM-BRIDGE",
    "rule_id": "INGEST-H-001",
    "enriched": N,
    "failed": M,
    "run_at": "ISO timestamp",
    "has_enforcement_loop": true,
    "forge_version": "1.4"
  }
}
```

La fiecare video îmbogățit:
```json
{
  "text": "{query_result}\n\n{report_content}",
  "collection": "intelligence",
  "metadata": {
    "type": "youtube_enriched",
    "source_channel": "{url}",
    "url": "{video_url}",
    "notebooklm": true,
    "enriched_at": "ISO timestamp"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- **`nexus-unified.sh` gate** — orice eșec de transcript → `notebooklm_enrich: true` setat obligatoriu (nu silent fail)
- **Bridge daily run gate** — bridge procesează toți itemii flagați din ziua precedentă
- **Daily smoke test** — verifică că `notebooklm-bridge.log` conține run în ultimele 25h

### WHEN
- La fiecare HTTP 429 sau transcript empty în `nexus-unified.sh`
- Zilnic la 07:00 (LaunchAgent)
- La flagare manuală de Pafi

### HOW (violation detection)
- `monitoring-db.json` — dacă există items cu `notebooklm_enrich: true` și `notebooklm_enriched_at: ""` mai vechi de 48h: **violation** (bridge nu a rulat sau a eșuat repetat)
- `notebooklm-bridge.log` — dacă nu conține run în ultimele 25h: **LaunchAgent eșuat**
- Cortex — dacă `type: youtube_enriched` entries = 0 după 7 zile de la activare: **pipeline nu produce output**
- `nexus-unified.sh` log — dacă conține 429 errors fără `notebooklm_enrich flag set` nearby: **violation**

### CONNECT
- **INGEST-H-001** → enforcement la `nexus-unified.sh` (flagare) + bridge (procesare)
- **`procedure-health.json`** → entry pentru această procedură
- **`notebooklm-bridge.log`** → monitorizat de daily smoke test
- **SOURCE-CHANNEL-FEED-MONITOR** (m4-500) → `monitoring-db.json` schema partajată; câmpurile `notebooklm_*` adăugate în același `item_to_schema()`
- **m4-501** → implementare Codex

### VERIFY
- [ ] Procedura executată complet? Bridge rulat, `notebooklm_enriched_at` populat pe cel puțin 1 item
- [ ] Output satisface §1? Cortex entries cu `notebooklm: true` există și au `key_insights` non-empty
- [ ] VK emis? ✅
- [ ] Gemini HIGH H1 fix? (`studio_id` capturat din `report_create`, pasat la `studio_poll` — NU `notebook_id`)
- [ ] Gemini HIGH H2 fix? (`flock` implementat pe toate read/write `monitoring-db.json` în bridge)
- [ ] Gemini MEDIUM M1 fix? (`jq` atomic pentru JSON mutation în `nexus-unified.sh`)
- [ ] Gemini MEDIUM M2 fix? (prompt `notebook_query` returnează JSON structurat cu chei definite)

### MODEL ROUTING
- `notebooklm-bridge.py` — NotebookLM (Google, $0) pentru procesare video
- `notebook_query` prompt → NotebookLM AI intern (nu Claude, nu Opus)
- Cortex store — no LLM
- `nexus-unified.sh` flagging — no LLM (bash `jq`)

---

## Referințe

- Gemini Audit: `~/.nexus/workspace/output/forge/audit-gemini-20260315-082313.md` (CONDITIONAL 3.17/4.0)
- Proposal original: `/tmp/notebooklm-hybrid-proposal.md`
- Procedură înrudită: `~/.nexus/procedures/SOURCE-CHANNEL-FEED-MONITOR.md` (m4-500 — schema partajată)
- Fișiere afectate: `~/.nexus/echelon/nexus-unified.sh`, `~/.nexus/intel/ingest.py`, `~/.nexus/intel/notion-sync.py`
- Fișiere noi: `~/.nexus/scripts/notebooklm-bridge.py`, `~/Library/LaunchAgents/com.nexus.notebooklm-bridge.plist`
- MCP tools: `mcp__notebooklm-mcp__notebook_create/add_url/query/report_create/studio_poll/notebook_delete`
