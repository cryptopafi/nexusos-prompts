---
type: procedure
created: 2026-03-17
status: active
slug: x-post-ingestion
tags: [procedure, nexus]
---

# X-POST-INGESTION — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-16
**Versiune**: 1.0
**Regulă asociată**: MEM-H-001 (Everything → Cortex), INGESTION-DUPLICATE-CHECK v2.0
**Scope**: Ingestie completă a unui post X/Twitter trimis manual de Pafi în Telegram, cu deep-follow pe links și fișiere, plus înregistrare automată a contului în ECHELON Intel Hub.

---

## 1. Problema

Pafi trimite linkuri x.com în Telegram (articole, threads, tool-uri, insights). Fără procedură:
- Conținutul se pierde după ce dispare din feed
- Linkurile și fișierele din tweet nu sunt citite
- Contul autorului nu intră în monitorizare ECHELON
- Nu există urmă în Cortex → nu apare în research viitor

**Soluția**: La orice URL x.com/twitter.com primit în Telegram → pipeline complet: fetch tweet → deep-follow links → store Cortex → register ECHELON.

---

## 2. Procedura

### Pre-Execution Plan
Înainte de a executa, evaluează:
1. URL-ul e tweet individual, thread, sau profil?
2. Ce tools sunt disponibile? (agent-browser → fetch, WebFetch → links, youtube-transcript → video)
3. Contul există deja în `~/.nexus/channels/`? (nu adăuga duplicate)
4. Care este depth-ul curent? (max 2 nivele de recursie)

---

### Pas 1: Fetch Tweet via agent-browser

- Navighează la URL-ul x.com primit
- Extrage:
  - Text complet al tweet-ului / thread-ului (toate tweet-urile în ordine)
  - Autor: `@handle` + display name + bio scurtă
  - Dată publicare
  - Media: descrie imagini și video (nu descărca)
  - Toate URL-urile găsite în text sau în carduri atașate
- **Checkpoint**: text extras ≥ 10 caractere → continuă. Altfel → raportează Pafi că x.com a blocat accesul și cere paste manual.

---

### Pas 2: Deep Ingestion — Links din Tweet (max depth 2)

Pentru **fiecare link** găsit în tweet:

```
THOUGHT: Ce tip de conținut e link-ul?
ACT:
  - Articol / pagină web  → WebFetch → extrage text complet
  - PDF                   → WebFetch sau Read dacă local
  - YouTube               → youtube-transcript MCP
  - Alt tweet x.com       → repetă Pas 1 (DOAR dacă depth < 2)
  - GitHub repo           → WebFetch README + descriere structură
  - Fișier (doc/zip/img)  → notează tipul, nu descărca
OBSERVE: Conținut extras cu succes?
STORE: Cortex save pentru fiecare item (vezi Pas 4), cu depth=2 în metadata
```

**Depth Guard**: La depth 2, dacă găsești alt link → salvează URL-ul în Cortex cu `type: pending_ingestion` dar **NU** fetch. Listează-le în raportul final pentru Pafi.

**Domenii excluse** (skip fără a raporta): wikipedia.org, google.com/search, amazon.com, t.co redirect loops.

---

### Pas 3: Înregistrare Cont X în ECHELON Intel Hub

1. Extrage din profilul autorului: `@handle`, display name, bio, URL profil
2. Generează slug: `x-{handle}` (ex: `x-steipete`)
3. Verifică dacă există deja:
   ```bash
   ls ~/.nexus/channels/ | grep "{slug}"
   ```
4. **Dacă există** → skip, notează "already monitored" în raport
5. **Dacă nu există** → creează `~/.nexus/channels/x-{handle}.md`:

```markdown
# {DisplayName} — Source Profile
**Status**: DISCOVERED
**Type**: x-account
**Discovered**: {data azi}
**URL**: https://x.com/{handle}
**Identity**: handle=@{handle}; display={DisplayName}; bio={bio scurtă}
**Reach**: followers=unknown (fetch la prima rulare ECHELON)
**Content Map**: {topicuri detectate din tweet}
**Bias Flags**: none identified
**Decision**: TRACK
**Added by**: lis-manual-ingest
```

---

### Pas 4: Cortex Save

**Tweet principal (depth 1)**:
```json
{
  "text": "[X-POST] @{handle}: {text_complet_tweet}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "x-post",
    "url": "{url_original}",
    "author": "@{handle}",
    "date": "{data_tweet}",
    "depth": 1,
    "links_found": "{N}",
    "links_ingested": "{M}",
    "links_pending": "{P}",
    "echelon_registered": true,
    "tags": ["x-post", "manual-ingest"]
  }
}
```

**Fiecare link ingested (depth 2)**:
```json
{
  "text": "[X-DEEP] {titlu sau primele 500 caractere conținut}",
  "collection": "business_clickwin",
  "metadata": {
    "type": "x-post-link",
    "parent_url": "{url_tweet_original}",
    "source_url": "{url_link}",
    "depth": 2,
    "tags": ["x-post", "deep-ingest"]
  }
}
```

---

### Pas 5: Raport Final către Pafi

```
✅ Tweet ingested: @{handle} — "{primele 80 caractere text}"
📎 Links: {N} găsite | {M} ingested | {P} pending
🎯 ECHELON: @{handle} — {adăugat nou ✅ / deja monitorizat ♻️}
🧠 Cortex: {ID_principal} + {M} sub-items saved
```

Dacă `P > 0` (links pending depth 3+), listează-le:
```
⏳ Links neingested (depth limit):
  - {url1}
  - {url2}
Trimiți-le manual dacă vrei ingestie completă.
```

---

## 3. Cortex Logging

La fiecare execuție, pe lângă itemele de conținut, salvează un log de execuție:
```json
{
  "text": "X-POST-INGESTION run: @{handle} | links={N} | ingested={M} | echelon={new/existing}",
  "collection": "procedures",
  "metadata": {
    "type": "ingestion-log",
    "procedure": "X-POST-INGESTION",
    "rule_id": "MEM-H-001",
    "has_enforcement_loop": true,
    "forge_version": "1.4",
    "tags": ["x-post", "ingestion", "log"]
  }
}
```

---

## 4. Enforcement Loop

### WHERE
- Lis system prompt (reflex activ): orice mesaj Telegram care conține `x.com/` sau `twitter.com/` → declanșează această procedură
- WISH Step H: dacă Genie detectează URL x.com în context → dispatch la Lis

### WHEN
- Trigger: URL x.com/twitter.com primit în Telegram de la Pafi (manual sau forward)
- Nu se aplică pentru: linkuri x.com în interiorul altor pagini web (acolo se aplică INGESTION-DUPLICATE-CHECK)

### HOW (violation detection)
- Lis răspunde la URL x.com fără a ingesta → violation MEM-H-001
- Lis ingestează dar nu verifică ECHELON → procedură incompletă
- Lis depășește depth 2 fără depth guard → loop risk

### CONNECT
- Regulă: MEM-H-001 — orice informație valoroasă → Cortex
- Proceduri conexe: INGESTION-DUPLICATE-CHECK (Step 0 înainte de fetch), LIKED-CONTENT-INGESTION (pentru liked tweets automat — procedură separată)
- ECHELON: channel file creat → pickup automat la următoarea rulare ECHELON scraper

### VERIFY
- [ ] Tweet principal stocat în Cortex cu metadata completă?
- [ ] Toate linkurile din tweet procesate (ingested sau pending)?
- [ ] Channel file creat în `~/.nexus/channels/x-{handle}.md`?
- [ ] Raport trimis Pafi cu counts corecte?
- [ ] Depth guard respectat (niciun fetch la depth 3+)?

### MODEL ROUTING
- agent-browser: fetch tweet content (x.com blochează WebFetch simplu)
- WebFetch: articole și pagini linked din tweet
- youtube-transcript MCP: video-uri linkate
- Cortex API: store + duplicate check
- File write: `~/.nexus/channels/x-{handle}.md`
