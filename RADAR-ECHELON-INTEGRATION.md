---
type: procedure
created: 2026-03-17
status: active
slug: radar-echelon-integration
tags: [procedure, nexus]
---

# RADAR-ECHELON-INTEGRATION — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-07
**Versiune**: 1.0
**Regula asociata**: ECHELON-OPS-001
**Scope**: Integrarea bidirectionala intre sistemul Radar (surse monitorizate) si ECHELON (pipeline intelligence) — adaugare surse, sync watchliste, descoperire automata.

---

## 1. Problema

Fara aceasta procedura, sursele noi descoperite in NEXUS/BI raman izolate in `sources.json` si nu ajung niciodata in ECHELON watchlist-uri. Invers, canalele noi descoperite de ECHELON nu sunt inregistrate in Radar, deci nu beneficiaza de monitoring continuu.

Situatii acoperite:
- Pafi adauga manual o sursa noua via Telegram (`/radar-add <url>`)
- NEXUS sinteza cu EPR >= 14 descopera surse valoroase automat
- ECHELON YouTube descopera un canal nou via `discoverSourceProfile()`
- Pafi vrea sa vada toate sursele monitorizate intr-un singur loc (Notion Radar DB)

---

## 2. Procedura

### Pas 1: ADAUGARE SURSA MANUALA (via Telegram)

Trigger: Pafi trimite `/radar-add <url>` in Telegram.

**Flow**:
1. `relay.ts` intercepteaza comanda `/radar-add`
2. Apeleaza `source-discover.py --url <url>` → genereaza profil in `~/.nexus/channels/<slug>.md`
3. Apeleaza `radar-db.js add --url <url> --discovered-via manual --project-slugs echelon` → adauga in `sources.json`
4. Ruleaza `radar-sync.py --push` → sincronizeaza entry nou in Notion Radar DB
5. Daca `type=youtube` si `project_slugs` contine `"echelon"` → executa Pasul 3 (sync in watchlist)

**Verificare**:
```bash
python3 ~/.nexus/radar/radar-sync.py --status
# Output asteptat: sources local=N, notion=N (egal)
```

---

### Pas 2: ADAUGARE SURSA AUTOMATA (via NEXUS EPR hook)

Trigger: `nexus-run.sh` finalizeaza o sinteza cu `epr_score >= 14`.

**Flow**:
1. `nexus-run.sh` apeleaza `nexus-radar-hook.py --synth-file <path> --epr-threshold 14`
2. Hook-ul extrage URL-uri din `source_ids` + `summary` (max 3 per sinteza)
3. Filtreaza domenii excluse: wikipedia, twitter, x.com, youtube, google, amazon, linkedin
4. Pentru fiecare URL nou: adauga entry in `sources.json` cu `discovered_via: "nexus"`, `tier: 2`, `frequency: "daily"`
5. Ruleaza `radar-sync.py --push` automat pentru a sincroniza Notion

**Verificare**:
```bash
# Ultimele surse adaugate automat
python3 -c "
import json, pathlib
db = json.loads(pathlib.Path.home().joinpath('.nexus/radar/sources.json').read_text())
nexus = [s for s in db['sources'] if s.get('discovered_via') == 'nexus']
print(f'Surse auto-adaugate: {len(nexus)}')
for s in nexus[-3:]: print(f'  {s[\"type\"]} | {s.get(\"github\",{}).get(\"repo\") or s.get(\"website\",{}).get(\"url\",\"\")}')
"
```

---

### Pas 3: SYNC RADAR → ECHELON WATCHLIST

Trigger: Sursa noua de tip `youtube` adaugata in Radar cu `project_slugs` continand `"echelon"`.

**Flow**:
1. Identifica entry-urile din `sources.json` unde `type == "youtube"` si `"echelon" in project_slugs`
2. Extrage `youtube.url` sau `youtube.channel_id` din fiecare entry
3. Verifica daca channel_id exista deja in `~/.nexus/echelon/youtube/watchlist.json`
4. Daca NU exista → adauga entry nou in watchlist cu `tier: 2`, `trigger: "radar"`, `status: "active"`
5. Logheaza in `~/.nexus/echelon/youtube/logs/radar-sync.log`: `[RADAR-SYNC] added channel=<id> source_id=<uuid>`

**Comanda manuala**:
```bash
node ~/.nexus/radar/radar-db.js sync-echelon
# Sincronizeaza toate sursele youtube din Radar in ECHELON watchlist
```

**Verificare**:
```bash
python3 -c "
import json, pathlib
wl = json.loads(pathlib.Path.home().joinpath('.nexus/echelon/youtube/watchlist.json').read_text())
radar_added = [c for c in wl.get('channels', []) if c.get('trigger') == 'radar']
print(f'Canale adaugate via Radar: {len(radar_added)}')
"
```

---

### Pas 4: ECHELON → RADAR DISCOVERY FEEDBACK

Trigger: `youtube-enrich.js` apeleaza `discoverSourceProfile()` pentru un canal nou (scor relevanta > 0.4 sau engagement ridicat).

**Flow**:
1. `youtube-enrich.js` detecteaza canal nou relevant (nu e in watchlist, dar e de interes)
2. Apeleaza `discoverSourceProfile()` → creeaza `~/.nexus/channels/<slug>.md`
3. Dupa creare profil, apeleaza `radar-db.js add --channel-id <id> --type youtube --discovered-via echelon --project-slugs echelon`
4. Canal devine vizibil in Radar DB (status: `active`, tier: 2)
5. La urmatorul run `radar-sync.py --push` → apare si in Notion

**Nota**: Aceasta directie (ECHELON → Radar) este automata. Nu necesita interventie manuala.

---

### Pas 5: SYNC BIDIRECTIONAL COMPLET (rulat la cerere sau daily)

**Comanda**:
```bash
python3 ~/.nexus/radar/radar-sync.py --push   # local → Notion
python3 ~/.nexus/radar/radar-sync.py --pull   # Notion → local (modificari manuale Pafi)
python3 ~/.nexus/radar/radar-sync.py --status # verificare paritate
```

**Cand se ruleaza**:
- Automat: dupa orice `radar-db.js add` (Pasii 1-4)
- Manual: dupa ce Pafi editeaza direct in Notion Radar DB (frecventa, status, project_slugs)
- Daily: LaunchAgent `com.echelon.orchestrator` (06:00) include `--push` in orchestrare

**Integrare in orchestrator**:
Adauga in `~/.nexus/echelon/echelon-orchestrate.sh` sectiunea de pre-run:
```bash
# Radar sync — pull modificari Notion inainte de run
python3 ~/.nexus/radar/radar-sync.py --pull 2>/dev/null || true
```

---

### Pas 6: VERIFICARE STARE COMPLETA RADAR↔ECHELON

Ruleaza dupa orice modificare majora sau la review saptamanal:

```bash
# 1. Stare Radar DB
python3 -c "
import json, pathlib
db = json.loads(pathlib.Path.home().joinpath('.nexus/radar/sources.json').read_text())
sources = db.get('sources', [])
print(f'Total surse: {len(sources)}')
by_type = {}
for s in sources:
    by_type[s['type']] = by_type.get(s['type'], 0) + 1
for t, n in sorted(by_type.items()): print(f'  {t}: {n}')
active = sum(1 for s in sources if s.get('status') == 'active')
print(f'Active: {active} | Paused: {len(sources)-active}')
"

# 2. Canale ECHELON din Radar
python3 -c "
import json, pathlib
db = json.loads(pathlib.Path.home().joinpath('.nexus/radar/sources.json').read_text())
echelon = [s for s in db['sources'] if 'echelon' in s.get('project_slugs', [])]
print(f'Surse cu project_slug=echelon: {len(echelon)}')
"

# 3. Paritate Notion
python3 ~/.nexus/radar/radar-sync.py --status
```

---

## 3. Cortex Logging

Dupa fiecare adaugare de sursa (manuala sau auto), se salveaza in Cortex:

```json
{
  "text": "RADAR-ADD: <url> | type=<type> | discovered_via=<via> | project_slugs=<slugs> | epr=<score>",
  "collection": "echelon",
  "metadata": {
    "type": "radar-source-add",
    "procedure": "RADAR-ECHELON-INTEGRATION",
    "rule_id": "ECHELON-OPS-001",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["radar", "echelon", "source-discovery"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Post-H dupa orice `/radar-add` via Telegram
- Post-H dupa orice sinteza NEXUS cu EPR >= 14
- `echelon-orchestrate.sh` sectiunea pre-run (daily 06:00)

### WHEN
- La fiecare adaugare sursa noua → verify Pas 5 (`radar-sync.py --status`)
- Saptamanal: review Pas 6 (verificare stare completa)
- Dupa modificari manuale in Notion → `radar-sync.py --pull`

### HOW (violation detection)
- `sources.json` modificat dar `radar-sync.py --status` arata discrepanta → violation
- Canal YouTube in Radar cu `project_slugs: ["echelon"]` dar NU in watchlist.json → violation
- `nexus-run.sh` a terminat cu EPR >= 14 dar `sources.json` nu e actualizat → violation
- Runner: `radar-sync.py --status` (manual), daily in orchestrator log

### CONNECT
- `~/.nexus/radar/nexus-radar-hook.py` → Pasul 2 (auto-add din NEXUS)
- `~/.nexus/echelon/youtube/youtube-enrich.js` → Pasul 4 (discovery feedback)
- `~/.nexus/radar/radar-sync.py` → Pasul 5 (sync bidirectional)
- `~/.nexus/echelon/echelon-orchestrate.sh` → Pasul 5 (pre-run pull)
- `procedure-health.json` → adauga entry `RADAR-ECHELON-INTEGRATION`

### VERIFY

- [ ] `sources.json` si Notion Radar DB sunt in paritate (`radar-sync.py --status` = OK)?
- [ ] Sursele YouTube cu `project_slugs: ["echelon"]` sunt in `watchlist.json`?
- [ ] `nexus-radar-hook.py` apelat din `nexus-run.sh` pentru EPR >= 14?
- [ ] `echelon-orchestrate.sh` include `radar-sync.py --pull` in pre-run?

**VK-uri obligatorii** (per VK-H-001):
1. `[PROC] RADAR-ECHELON-INTEGRATION | §1 §2 §3 §4 §5 §6 VER | complete`
2. `[CORTEX] "RADAR-ECHELON-INTEGRATION v1.0" | FORGE | rule: ECHELON-OPS-001 | v1.0`

### MODEL ROUTING
| Activitate | Model | Motivul |
|-----------|-------|---------|
| Executie procedura (adaugare surse, sync) | Sonnet 4.6 (Genie) | Operational, comenzi directe |
| Modificare script-uri (radar-db.js, nexus-run.sh) | gpt-5.6-sol (Codex) | DEV-H-013 |
| Audit periodic | Opus 4.6 subagent | Review structural |
