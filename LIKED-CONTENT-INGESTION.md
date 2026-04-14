---
type: procedure
created: 2026-03-17
status: active
slug: liked-content-ingestion
tags: [procedure, nexus]
---

# LIKED-CONTENT-INGESTION — Procedură Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-01
**Versiune**: 1.0
**Regulă asociată**: MEM-H-001 (Everything → Cortex), INGESTION-DUPLICATE-CHECK v2.0
**Scope**: Ingestie automată nocturnă a conținutului dat like de Pafi pe platformele sociale

---

## 1. Problema

Pafi dă like la conținut valoros pe YouTube, Reddit, Instagram, X, Facebook în timp real. Fără automatizare:
- Conținutul e pierdut (uitat după ce trece din feed)
- Nu intră în Cortex/ECHELON
- Nu generează actionable items legate de proiecte
- Nu apare în Research DB pe paginile Notion relevante

**Soluția**: Nightly script (21:00) → fetch liked items → pipeline standard de ingestie manuală.

---

## 2. Trigger

- **Automat**: LaunchAgent `com.genie.liked-content-ingestion` — zilnic la **21:00**
- **Manual**: `bash ~/.nexus/scripts/liked-content-ingestion.py` sau `/ingest-likes`
- **Scope**: Doar items noi față de ultima rulare (state file tracking)

---

## 3. Platforme — Status & Metodă

| Platformă | Status | Metodă | Credentials |
|-----------|--------|--------|-------------|
| **YouTube** | ✅ ACTIVE | yt-dlp cu Chrome cookies → playlist LL (Liked Videos) | Chrome session |
| **Reddit** | ⚙️ PENDING | Reddit API v1 OAuth (PRAW) → `/user/{me}/liked` | Keychain: `REDDIT_CLIENT_ID` + `REDDIT_CLIENT_SECRET` |
| **X/Twitter** | ⚙️ PENDING | Twitter API v2 → `GET /2/users/:id/liked_tweets` | Keychain: `TWITTER_BEARER_TOKEN` |
| **Instagram** | ✅ ACTIVE (via Apify) | Apify `apify/instagram-post-scraper` liked posts | Keychain: `APIFY_API_KEY` ✅ |
| **Facebook** | ⚙️ PENDING | Apify Facebook scraper sau Manual review | Keychain: `FACEBOOK_COOKIE` |

---

## 4. Pipeline per Item

```
Pentru FIECARE liked item nou:
  │
  ├─ Step 0: Duplicate Check (INGESTION-DUPLICATE-CHECK §Step 0)
  │   → Search Cortex by URL. Score > 0.75 → SKIP
  │
  ├─ Step 0.5: Source Research (INGESTION-DUPLICATE-CHECK §Step 0.5)
  │   → Dacă sursa e nouă → documentează în ~/.nexus/channels/
  │
  ├─ Step 1: Extract Content
  │   YouTube  → yt-dlp transcript (srv1 format)
  │   Article  → WebFetch + readability
  │   Post     → fetch text + media description
  │   PDF      → text extraction
  │
  ├─ Step 2: Classify + Summarize
  │   → Ollama llama3.2:3b pentru clasificare topic
  │   → Gemini Flash pentru summary rapid
  │   → Claude Opus (claude-opus-4-6) pentru insights — TOATE posturile, fără excepție
  │   → Extrage: summary, key insights, bias flags, actionable items
  │
  ├─ Step 3: Save to Cortex
  │   collection: topic-specific (health, investments, personal, etc.)
  │   tags: platformă + topic + autor
  │
  ├─ Step 4: Update Echelon Reports DB (Notion)
  │   Proprietăți: Name, Type, Status=Summarized, Date, Channels, URL
  │
  ├─ Step 5: Add to Research DB pe pagina Notion relevantă
  │   → Determină topic din clasificare → găsește page_id din page-map.json
  │   → Creează entry în Research DB al paginii respective
  │   → Adaugă actionable items legate de proiectele existente
  │
  └─ Step 6: Propose new project dacă insight identifică oportunitate nouă
```

---

## 5. State Management

**State file**: `~/.nexus/sync/liked-content-state.json`

```json
{
  "last_run": "2026-03-01T21:00:00",
  "ingested": {
    "youtube": ["url1", "url2"],
    "reddit": ["post_id1"],
    "instagram": ["post_id1"],
    "twitter": ["tweet_id1"]
  },
  "stats": {
    "total_ingested": 0,
    "last_session_count": 0
  }
}
```

**Logică**: La fiecare rulare, compară liked items cu `ingested` array → procesează doar ce e nou.

---

## 6. Topic Routing

Clasificatorul determină topic-ul conținutului → mapare la pagina Notion corectă:

| Topic clasificat | Research DB pagina |
|-----------------|-------------------|
| health, fitness, longevity, biohacking, peptides | health (`316d31d1-1b7d-8128-a6af-fd9eb7ad5da4`) |
| investing, stocks, crypto, finance | investments (`316d31d1-1b7d-814f-94b1-f1d3641be8ce`) + crypto |
| AI, automation, tech, LLM | echelon / codex / multiagentic |
| business, marketing, sales | ai_b2b / smsads (cel mai relevant) |
| personal development, coaching | personal_coach (`316d31d1-1b7d-814f-a215-f2a9c1473557`) |
| strategy | strategy training page |
| default | personal_hub |

---

## 7. Notificare Telegram

La finalul fiecărei rulări, trimite sumar la `@claudemacm4_bot`:

```
🔄 LIKED CONTENT INGESTION — 2026-03-01 21:00

📊 Procesate: 8 items noi
  ✅ YouTube: 5 (Bachmeyer peptides, AI Twin, Obsidian...)
  ✅ Reddit: 2 (r/LocalLLaMA, r/investing)
  ✅ Instagram: 1 (@drtrevorbachmeyer reel)
  ⏭️  Skipped (duplicate): 3

📁 Salvate în: Cortex + Echelon DB + Research DB
💡 Actionable items propuse: 4
```

---

## 8. Script & LaunchAgent

**Script**: `~/.nexus/scripts/liked-content-ingestion.py`
**Log**: `~/.nexus/logs/liked-ingestion.log`
**LaunchAgent**: `~/Library/LaunchAgents/com.genie.liked-content-ingestion.plist`
**Schedule**: zilnic 21:00 (StartCalendarInterval Hour=21 Minute=0)

---

## 9. Setup Checklist

- [x] YouTube: yt-dlp Chrome cookies → playlist LL funcțional
- [x] Apify: API key disponibil în Keychain
- [ ] Reddit: adaugă `REDDIT_CLIENT_ID` + `REDDIT_CLIENT_SECRET` în Keychain
- [ ] Twitter/X: adaugă `TWITTER_BEARER_TOKEN` în Keychain
- [ ] Facebook: adaugă `FACEBOOK_COOKIE` în Keychain sau decide Apify
- [ ] Script implementat (Codex brief: m4-XXX)
- [ ] LaunchAgent creat și testat

---

## 10. Cortex Logging

```json
{
  "text": "LIKED-CONTENT-INGESTION: {N} items processed | {N} new | {N} duplicates skipped | platforms: {list}",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "LIKED-CONTENT-INGESTION",
    "rule_id": "MEM-H-001",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["ingestion", "liked-content", "nightly", "automation"]
  }
}
```

## 11. Enforcement Loop (META-H-002)

### WHERE
- La fiecare rulare nocturnă automată
- La fiecare `/ingest-likes` manual

### WHEN
- 21:00 zilnic (automat)
- Manual la cerere

### HOW (violation detection)
- Script fără state file → re-ingestează duplicates → violation
- Item procesat fără duplicate check → violation
- Item procesat fără topic routing → salvat în personal_hub ca fallback (warning, nu violation)

### CONNECT
- `MEM-H-001` → Everything → Cortex (regula sursă)
- `INGESTION-DUPLICATE-CHECK.md` → apelat la Step 0/0.5 per item
- `procedure-health.json` → entry `LIKED-CONTENT-INGESTION`
- Notion Research DB → destinația finală items
- Telegram `@claudemacm4_bot` → notificare post-rulare

### VERIFY
- [ ] State file actualizat după fiecare rulare
- [ ] Telegram notification trimis
- [ ] Zero duplicate entries în Cortex (verificabil cu search)
- [ ] VK emis în sesiune

**VK-uri obligatorii**:
1. `✅ [PROC] LIKED-CONTENT-INGESTION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Liked Content Ingestion" | FORGE ✓ | rule: MEM-H-001 | v1.0`

---

## 11. Changelog

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2026-03-01 | Initial FORGE procedure. YouTube LL playlist funcțional. Apify disponibil. Reddit/Twitter pending credentials. |

---

## Checklist Pre-Publicare (FORGE META-H-002)

- [x] Regulă asociată: MEM-H-001 + INGESTION-DUPLICATE-CHECK
- [x] Enforcement loop complet: WHERE + WHEN + HOW + VERIFY
- [x] Pipeline clar per item (§4)
- [x] State management definit (§5)
- [x] Topic routing definit (§6)
- [x] Setup checklist (§9)
- [ ] Script implementat
- [ ] LaunchAgent creat
- [ ] Testat end-to-end
