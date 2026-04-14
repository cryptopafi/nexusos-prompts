---
type: procedure
created: 2026-03-17
status: active
slug: superinsight
tags: [procedure, nexus]
---

# SuperInsight — Intelligence Pipeline

**Status**: ACTIVE
**Version**: 2.0
**Date**: 2026-03-01
**Regulă asociată**: ECHELON-S-002 (Intelligence Enrichment)
**Scope**: Pipeline complet de enrichment pentru orice conținut ingerat — clasificare automată Layer 1 (Ollama) + sinteză Layer 2 (Gemini/Opus) + livrare Telegram + save Cortex.

## Architecture
- Layer 1 (classifier): Ollama qwen2.5:7b → content_type, topic, processing_profile **[AUTOMATIC]** *(⚠️ llama3.2:3b dezinstalat — qwen2.5:7b activ ca primary. Run `ollama pull llama3.2:3b` pentru a reveni la modelul original.)*
- Layer 2 (insight): Gemini Flash (default) **[AUTOMATIC]** | Opus **[AUTOMATIC dacă engagement>5000]** | Opus **[MANUAL trigger explicit]**
- Fallback standard: Gemini Flash → Ollama qwen2.5:7b → Grok **[AUTOMATIC]**
- Fallback deep: Opus → Gemini Flash → Ollama qwen2.5:7b **[AUTOMATIC]**

### Automation Gates
| Pas | Mod | Cine decide |
|-----|-----|-------------|
| Layer 1 clasificare | AUTOMATIC | Ollama qwen2.5:7b (primary actual; llama3.2:3b dezinstalat) |
| Layer 2 model selection | AUTOMATIC | engagement threshold (>5000 → Opus, default → Gemini) |
| Layer 2 Opus manual | MANUAL DELIBERAT | Pafi (pentru conținut excepțional) |
| Source TRACK/OCCASIONAL/SKIP | MANUAL DELIBERAT | Pafi (decizie strategică de canal) |
| Output folder routing | AUTOMATIC | topic din Layer 1 → business/ sau pafi/ |

## Trigger
- Telegram @claudemacm4_bot: `/ingest <url>` sau file attachment cu `/ingest`
- Automated: din ECHELON pipeline (engagement>2000)

## Output
- Folder: ~/.nexus/business/<topic>/ sau ~/.nexus/pafi/<topic>/
- Telegram: @claudemacm4_bot (full insight text)
- Cortex: save cu tag superinsight + topic

## 10 Sections
1. Summary (3-5 bullet points)
2. Key Claims
3. Claims Verification
4. Entity Discovery
5. Bias Detection
6. Action Items
7. Referenced Materials
8. Cortex Connections
9. Pattern Recognition
10. Intelligence Grade (A/B/C/D)

---

## Sub-Procedure: Auto Source Discovery (SUPERINSIGHT-SOURCE-001)

**Version**: 1.0 | **Date**: 2026-03-01
**Scope**: Când ECHELON/SuperInsight auto-pipeline întâlnește o sursă nouă → documentează, NU ingestează tot.

**PRINCIPIU**: Source discovery ≠ full ingestion. Goal = catalog rapid al surselor noi pentru decizia manuală ulterioară.

### Trigger
- **Orice link găsit automat** de ECHELON/radar-research — indiferent dacă sursa e nouă sau nu
- La fiecare link procesat: verifică dacă ecosistemul sursă e documentat → dacă nu, documentează-l
- Un autor/cont nou apare în pipeline (YouTube channel ID, Instagram @handle, Twitter @handle, website domain, Reddit user, GitHub owner)

### Ce face
```
Orice link procesat de auto-radar
  │
  ├─ Extrage: autor/canal/domeniu din link
  │
  ├─ Check: există ~/.nexus/channels/SOURCE-SLUG.md?
  │   └─ DA → skip source research (deja documentat), continuă cu conținutul
  │   └─ NU → documentează ecosistemul sursă
  │
  ├─ Extract lightweight profile (NU transcribe, NU ingesta):
  │   YouTube: channel metadata + ultimele 20 titluri + view counts
  │   Instagram: profil public (bio, followers, post count, topicuri frecvente din captions)
  │   Twitter/X: bio, followers, ultimele 20 tweet-uri (titluri/subiecte)
  │   Blog/website: titluri ultimelor 10 articole + about page
  │   Podcast: show metadata + ultimele 20 episoade (titluri)
  │
  ├─ Salvează ~/.nexus/channels/SOURCE-SLUG.md cu:
  │   - Identity (nume, tip, platforme găsite)
  │   - Reach (subscribers/followers per platformă)
  │   - Content map (topicuri frecvente din titluri — NU din transcrieri)
  │   - Bias flags (vinde produse? afilieri vizibile?)
  │   - Status: DISCOVERED (nu TRACK/OCCASIONAL/SKIP — decizia rămâne manuală)
  │   - Queue: lista titlurilor recente cu URL-uri (pentru ingestie manuală la decizia Pafi)
  │
  └─ Notifică via Telegram: "[NEW SOURCE] Autor: X | Nișă: Y | Titluri recente: ..."
     (scurt, max 3 linii — nu full ingestion)
```

### Ce NU face
- NU descarcă transcrieri
- NU analizează conținut video/audio
- NU creează Echelon DB entries (asta rămâne pentru ingestia manuală)
- NU decide TRACK/OCCASIONAL/SKIP — lasă decizia lui Pafi

### Output
```
[SOURCE DISCOVERY] Nou: @handle sau channel | Tipuri conținut: topics | Reach: Nnn | Profil: ~/.nexus/channels/slug.md
```

**Implementare**: Codex brief pending (m4-XXX) — adăugat în queue

---

## Sub-Procedure: Website Ingestion (SUPERINSIGHT-WEB-001)

**Version**: 1.0
**Date**: 2026-03-01
**Scope**: Ingestion completă a unui website (toate paginile relevante, nu doar homepage)

### Trigger
- `/ingest https://example.com` — când URL-ul e un root domain sau homepage (fără path lung)
- `/ingest-deep https://example.com` — cu Opus
- Detecție automată: dacă URL nu conține path > 2 segmente → tratează ca website, nu articol

### Pipeline

```
INPUT (root URL)
  │
  ├─ Step 1: DISCOVER — găsește toate paginile relevante
  │   ├─ 1a. Verifică /sitemap.xml (parsează URL-uri)
  │   ├─ 1b. Dacă nu există sitemap → crawl homepage: extrage toate linkurile interne
  │   ├─ 1c. Deduplică, filtrează (exclude: /cdn, /assets, /wp-json, /api, anchors #)
  │   └─ 1d. Prioritizează pagini (scor bazat pe path):
  │         HIGH:  /, /about, /services, /products, /pricing, /features, /team, /contact
  │         MED:   /blog (ultimele 3-5), /faq, /case-studies, /testimonials, /partners
  │         LOW:   /terms, /privacy, /careers (incluse dar procesate ultimele)
  │         SKIP:  /login, /register, /dashboard, /cart, /checkout, imagini, PDF-uri
  │
  ├─ Step 2: RENDER — extrage content din fiecare pagină
  │   ├─ 2a. Încearcă curl + readability (rapid, gratis)
  │   ├─ 2b. Dacă readability < 100 chars → Playwright headless rendering
  │   │       - Folosește profil persistent: ~/.claude/browser-profile/
  │   │       - Wait: networkidle sau 10s timeout
  │   │       - Extrage: document.body.innerText (fără nav/footer/cookie banners)
  │   ├─ 2c. Fallback: WebFetch MCP tool (dacă disponibil)
  │   └─ 2d. Output: un fișier .txt per pagină în $WORK_DIR/pages/
  │
  ├─ Step 3: COMBINE — unifică tot conținutul
  │   ├─ 3a. Header per pagină: "=== PAGE: /about ==="
  │   ├─ 3b. Concatenează toate paginile (ordine: HIGH → MED → LOW)
  │   ├─ 3c. Limit: max 15000 chars total (trunchiază LOW pages dacă depășește)
  │   └─ 3d. Output: $WORK_DIR/full_website.txt → devine FULL_TRANSCRIPT_FILE
  │
  ├─ Step 4: CLASSIFY + SUPERINSIGHT — pipeline normal
  │   ├─ 4a. Clasificare topic din combined content
  │   ├─ 4b. SuperInsight 10-section analysis pe conținut combinat
  │   ├─ 4c. Secțiunea extra pentru websites: "Site Structure Analysis"
  │   │       - Pages found vs crawled
  │   │       - Missing pages (no pricing? no team? no blog?)
  │   │       - Tech stack detectat (framework, CMS, analytics)
  │   └─ 4d. Output salvat în folder topic + Telegram
  │
  └─ Step 5: CORTEX — ingestie per-pagină
      ├─ 5a. Fiecare pagină → chunk separat în Cortex cu metadata:
      │       { source_url, page_path, website_domain, topic, crawl_date }
      └─ 5b. Website-ul complet → 1 entry în Cortex cu tag "website_snapshot"
```

### Limits & Guards
- **Max pages**: 25 (configurabil în ingest-smart-config.json `max_website_pages`)
- **Max crawl time**: 3 min (timeout total)
- **Max content per page**: 3000 chars
- **Max combined content**: 15000 chars (SuperInsight input)
- **Playwright timeout**: 10s per pagină
- **Rate limit**: 500ms delay între requests (politeness)
- **Robots.txt**: respectă dacă există (opțional override cu --force)

### Detecție Website vs Articol
```
URL este WEBSITE dacă:
  - path == "/" sau path gol (root domain)
  - path == "/home" sau "/index"
  - hostname fără path (https://smsads.net)
  - path are max 1 segment (/about, /pricing)

URL este ARTICOL dacă:
  - path are ≥2 segmente (/blog/2026/my-article)
  - path conține /post/, /article/, /p/, /watch
  - domain e platformă cunoscută (youtube, medium, substack, twitter)
```

### Config (ingest-smart-config.json)
```json
{
  "website_crawl": {
    "max_pages": 25,
    "max_crawl_time_seconds": 180,
    "max_content_per_page_chars": 3000,
    "max_combined_chars": 15000,
    "request_delay_ms": 500,
    "priority_paths": {
      "high": ["/", "/about", "/services", "/products", "/pricing", "/features", "/team", "/contact"],
      "medium": ["/blog", "/faq", "/case-studies", "/testimonials", "/partners", "/portfolio"],
      "low": ["/terms", "/privacy", "/careers", "/press"],
      "skip": ["/login", "/register", "/dashboard", "/cart", "/checkout", "/api", "/wp-json"]
    },
    "use_playwright": true,
    "playwright_executable": "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
    "respect_robots_txt": true
  }
}
```

### Dependențe
- Playwright (opțional dar recomandat pentru JS-heavy sites)
- Python3 (XML parsing sitemap, HTML stripping)
- curl (fallback non-JS)

### Output Extra (doar pentru websites)
Secțiunea 11 adăugată la SuperInsight standard:
```
11. Site Structure Analysis
    - Pages discovered: N | Crawled: M | Skipped: K
    - Missing critical pages: [pricing/team/blog — absent]
    - Tech indicators: [Next.js / WordPress / Webflow / custom]
    - SEO signals: [meta descriptions, OG tags, structured data]
    - Overall site maturity: [Early/Growing/Mature/Enterprise]
```
