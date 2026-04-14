---
type: procedure
created: 2026-03-17
status: active
slug: ingestion-duplicate-check
tags: [procedure, nexus]
---

# Manual Ingestion: Duplicate Check + Source Research

**Status**: ACTIVE
**Version**: 2.0 | **Date**: 2026-03-01 | **FORGE**: META-H-002

## 1. Problema

La ingestia manuală există două riscuri: (1) duplicarea conținutului deja prezent în Cortex, (2) pierderea contextului despre autorul/sursa linkului. Această procedură elimină ambele riscuri prin deduplication obligatorie și source research sistematic înainte de orice ingestie.

**PRINCIPIU**: Orice link trimis manual = interes pentru subiect SAU autor. Ambele merită studiate în detaliu.

---

## 2. Procedura

### Step 0 — DUPLICATE CHECK (obligatoriu înaintea oricărei ingestii manuale)

### A. Search Cortex by URL/Title
```bash
curl -sf -X POST http://localhost:6400/api/search \
  -H 'Content-Type: application/json' \
  -d '{"query":"URL_SAU_TITLU","limit":3}'
```

### B. Interpretare
| Score | Decizie |
|-------|---------|
| > 0.75 | **SKIP** — deja ingestat. Afișează titlu + dată + Cortex ID |
| 0.5–0.75 | **ASK** — probabil duplicat, confirmă cu userul |
| < 0.5 | **PROCEED** — conținut nou |

### C. Output obligatoriu
```
[DUPLICATE CHECK] URL: https://... | Status: NOT FOUND → proceed ✅
[DUPLICATE CHECK] URL: https://... | Status: FOUND → Cortex ID: xxx (2026-02-15) → SKIP ⛔
```

### D. Excepții
- **Re-ingestie explicită**: Userul spune "re-ingestează" sau "update" → proceed + notează ca update
- **URL diferit, conținut identic**: Verifică și după titlu exact

---

## Step 0.5 — SOURCE / AUTHOR RESEARCH (OBLIGATORIU pentru orice link nou)

**Regula**: Dacă userul trimite un link manual, îl interesează SUBIECTUL și/sau AUTORUL. Ambele se cercetează.

### A. Tipuri de surse și ce cercetezi

| Tip sursă | Ce extragi |
|-----------|-----------|
| **YouTube video** | Channel ID, subscribers, nișă, top videos, frecvență, bias flags. Salvează în `~/.nexus/channels/AUTOR.md` |
| **Instagram post** | Profil: followers, nișă, frecvență postare, alte platforme din bio |
| **Twitter/X post** | Profil: followers, topicuri frecvente, website din bio, alte rețele |
| **Facebook post/page** | Page: likes/followers, nișă, alt social din about |
| **Blog/articol** | Autor: bio, alte publicații, social media, site personal |
| **Podcast episode** | Show: platforme, guests recurenți, RSS feed, host background |
| **Website/landing page** | Companie/persoană: founding, produse, social links, email |
| **PDF/document** | Autor/organizație: afiliere, bias, alte lucrări |

### B. Comenzi utile
```bash
# YouTube channel
yt-dlp --print "%(channel)s | %(channel_id)s | %(channel_follower_count)s" "URL_VIDEO"
yt-dlp --flat-playlist --playlist-end 20 --print "%(title)s | %(duration_string)s | %(view_count)s" \
  "https://www.youtube.com/channel/CHANNEL_ID/videos"

# WebSearch pentru orice autor/brand
# Search: "AUTOR_NUME" site:instagram.com OR site:twitter.com OR site:linkedin.com
```

### C. Evaluare autor/sursă
| Criteriu | Ce urmărești |
|----------|-------------|
| **Credibilitate** | Expert independent sau vendor? Citează surse peer-reviewed? |
| **Bias flags** | Vinde produse proprii? Afilieri? Sponsorizări? |
| **Reach** | Câți urmăritori pe fiecare platformă? |
| **Consistență** | Produce regulat? De când? |
| **Cross-platform** | Pe ce alte platforme e activ? |

### D. Decizie
- **TRACK**: Autor/canal de mare valoare → creează fișier tracking în `~/.nexus/channels/` + ECHELON source
- **OCCASIONAL**: Valoros dar de nișă → ingestează selectiv, urmărește manual
- **SKIP**: Bias major, conținut slab, sau irelevant

### E. Output obligatoriu
```
[SOURCE RESEARCH] Tip: YouTube/Instagram/X/Blog | Autor: Nume | Reach: Nnn | Nișă: topic | Bias: note | Decizie: TRACK/OCCASIONAL/SKIP
[SOURCE RESEARCH] Social: Instagram @handle | Twitter @handle | LinkedIn URL | Website URL
```

### F. Fișier tracking
Salvează în `~/.nexus/channels/AUTOR-SLUG.md`:
- Identity (nume, bio, background)
- Toate social media + URLs
- Content map (ce topicuri acoperă)
- Bias flags
- Decizie + protocol ingestie
- Queue de conținut de ingestat (sortat după relevanță)
- Already ingested (cu Echelon DB ID)

---

## Flux complet ingestion manuală

```
Step 0:   Duplicate Check ← OBLIGATORIU
Step 0.5: Source / Author Research ← OBLIGATORIU pentru sursă nouă (orice tip)
Step 1:   Extract Content (yt-dlp / WebFetch / PDF read)
Step 2:   Summarize + Extract Key Insights + Actionable Items
Step 3:   Save to Cortex (collection: topic-specific)
Step 4:   Update Echelon Reports DB (Notion) — cu tags specifice nișei
Step 5:   Add Actionable Items în entry (legate de proiecte existente) ← OBLIGATORIU
Step 6:   Propose new project dacă insight-ul identifică oportunitate nouă
```

**Note**: Ingestia automată (ECHELON scrapers) are deja deduplication via URL hash în pipeline.

---

## 3. Cortex Logging

```json
{
  "text": "INGESTION-DUPLICATE-CHECK: URL={url} | Status={FOUND/NOT_FOUND} | Source research: {TRACK/OCCASIONAL/SKIP}",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "INGESTION-DUPLICATE-CHECK",
    "rule_id": "MEM-H-001",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["ingestion", "deduplication", "source-research"]
  }
}
```

## 4. Enforcement Loop (META-H-002)

### WHERE
- La fiecare link trimis manual de Pafi/Leo — fără excepție
- Genie execută Step 0 înainte de orice alt pas de ingestie

### WHEN
- La fiecare ingestie manuală de conținut
- Implicit prin LIKED-CONTENT-INGESTION pipeline (automat)

### HOW (violation detection)
- Ingestia fără Step 0 = MEM-H-001 violation (conținut duplicat în Cortex)
- GATE: Dacă Cortex score > 0.75 → STOP ingestie, raportează duplicatul
- Source research skip-uit pe sursă nouă → warning

### CONNECT
- `MEM-H-001` → Everything → Cortex (regula sursă)
- `LIKED-CONTENT-INGESTION.md` → apelează această procedură la Step 0/0.5
- `ECHELON` pipeline → dedup prin URL hash (automat, separat)
- `procedure-health.json` → entry `INGESTION-DUPLICATE-CHECK`

### VERIFY
- [ ] Duplicate check executat înainte de ingestie?
- [ ] Source research completat pentru surse noi?
- [ ] Output VK emis în sesiune?

**VK-uri obligatorii**:
1. `✅ [PROC] INGESTION-DUPLICATE-CHECK | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Ingestion Duplicate Check" | FORGE ✓ | rule: MEM-H-001 | v2.0`

**Exemple din execuție**:
```
[DUPLICATE CHECK] URL: https://youtube.com/watch?v=abc | Status: NOT FOUND → proceed ✅
[SOURCE RESEARCH] Tip: YouTube | Autor: Dr. Trevor Bachmeyer | Reach: 82K | Nișă: peptide/longevity | Bias: vinde protocoale proprii | Decizie: TRACK
```
