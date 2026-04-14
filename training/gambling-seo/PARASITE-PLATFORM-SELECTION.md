---
type: procedure
created: 2026-03-22
status: active
slug: parasite-platform-selection
tags: [procedure, nexus]
---

# TRAIN-SEO-004: PARASITE-PLATFORM-SELECTION — Opus v2
<!-- FORGE v2.0 | Created: 2026-03-04 | Updated: 2026-03-20 | Author: Genie | Source: Julian Goldie + Charles Floate Parasite Platform Analysis -->
<!-- Domeniu: Gambling SEO | Subdomeniu: Parasite SEO / Platform Selection / SERP Analysis -->

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 2.0
**Regulă asociată**: TRAIN-SEO-004
**Scope**: Structured decision process for selecting the optimal parasite SEO platform for a given casino/gambling keyword and target market, including SERP opportunity audit, platform scoring, localization validation, and content gap extraction.

---

## 1. Problema

Parasite SEO in the gambling niche relies on publishing on high-authority third-party platforms (DR 79–95+) to rank for competitive casino keywords without building domain authority from scratch. The core challenge is that platform selection is the make-or-break decision: wrong platform = content published but never ranked; right platform = top-3 result within 2–8 weeks with minimal link building.

Situations covered:
- **New market entry** — no existing parasite presence, need to identify best platform to rank fast
- **Existing parasite underperforming** — identify higher-authority or better-fit alternative
- **Multi-market expansion** — Peru, Kenya, Chile require localized platform validation
- **Budget-constrained campaigns** — choose free platforms (Medium, Quora) vs. paid news placements ($1–2K/article)
- **Content type mismatch** — video vs. written vs. Q&A format dictates platform choice

Without this procedure, platform selection is guesswork that wastes content production budget and misses ranking windows.

---

## 2. Procedura

### Pas 1: SERP Opportunity Audit (Ahrefs)

Before selecting any platform, audit the SERP for the target keyword to determine if a parasite play is viable and which platform is already active.

**Pași:**
1. Export top 100 organic results from Ahrefs for target keyword (ex: "best online casino Peru", "online casino Kenya", "casino en línea Chile")
2. Scan top 10 results — identify:
   - Are any known parasite platforms (Medium, Quora, Reddit, LinkedIn, business.site, news outlets) already in top 10?
   - What is the lowest DR site ranking in the top 10?
   - How many total backlinks does the weakest top-10 result have?
3. Apply opportunity scoring:

| Condition | Score | Action |
|-----------|-------|--------|
| No parasites in top 10 + low-authority sites (DR <40) ranking | EXCELLENT | Proceed immediately |
| No parasites in top 10 + medium-authority sites (DR 40–60) | GOOD | Proceed with link inversion plan |
| Parasites in top 10 but from different platform | VIABLE | Use identified platform + freshness inversion |
| Parasites in top 10 from same target platform | VIABLE | Outrank with freshness + more backlinks |
| Established casino brands dominate top 10 (DR 70+) | HARD | Use paid news parasites only or pivot keyword |

**Decision gate:**
- IF score = EXCELLENT or GOOD → continue to Pas 2
- IF score = VIABLE → note existing parasite URLs (you can outrank them), continue to Pas 2
- IF score = HARD → escalate: either pivot to long-tail keyword or budget for $1–2K paid news placement

---

### Pas 2: Platform Selection Decision Tree

Apply this decision tree in order. Stop at the first match.

**STEP A — Content Format**
- IF target keyword has strong video intent (ex: "casino slots demo", "online casino review [brand]") → **YouTube first**
- IF keyword is local/city-level (ex: "casino Lima", "casino online Nairobi") → **business.site first**
- IF keyword is a question format (ex: "is online casino legal in Peru?", "best casino bonus Kenya") → **Quora first**
- IF keyword is broad national casino term (ex: "best online casino Peru", "mejores casinos Chile") → **Medium or LinkedIn Articles**
- IF keyword has community/discussion angle (ex: "casino recommendations Reddit", forum threads) → **Reddit** (note: medium moderation risk)
- IF budget is available ($1–2K/article) and news authority is needed → **Paid news outlets** (10news.com, SFGate, regional equivalents)

**STEP B — Market Localization Filter (Pas 3 feeds into this)**
- Run the selected platform through Ahrefs country filter for target market
- IF platform ranks predominantly in US/UK English searches → check if it also ranks in Spanish/Swahili/local SERPs before committing
- IF platform does NOT rank in target country's Google → disqualify and select next platform from Step A

**STEP C — Budget Gate**
- IF budget = $0 → filter to: Medium, Quora, Reddit, LinkedIn Articles, business.site
- IF budget = $300–800 → add: regional news outlets in target market (regional press in Peru, Kenya, Chile)
- IF budget = $1,000–2,500 → add: SFGate, 10news.com, US-syndicated outlets

**Platform Quick Reference:**

| Platform | DR | Gambling-Friendliness | Best For | Cost | Risk |
|----------|----|-----------------------|----------|------|------|
| Medium | 94 | HIGH — no restrictions | Broad national casino terms, Spanish content | Free | LOW |
| Quora | 95+ | HIGH — Q&A format | Question-intent keywords, evergreen | Free | LOW |
| Vocal Media | 79 | HIGH — supports links | Secondary/supporting articles | Free | LOW |
| Reddit | 95+ | MEDIUM — moderation risk | Community/discussion angle | Free | MEDIUM |
| LinkedIn Articles | 95+ | MEDIUM — B2B angle | Operator/affiliate B2B terms | Free | LOW |
| business.site | High | HIGH — local review-based | City/local casino terms | Free | LOW |
| 10news.com / SFGate | 79–85 | HIGH — news authority | Competitive national terms | $1–2K/article | LOW |
| Regional news (LATAM/Africa) | 60–75 | HIGH | Market-specific national terms | $300–800/article | LOW |
| YouTube | 95+ | MEDIUM — policy risk | Video-intent keywords | Free | MEDIUM |

---

### Pas 3: Localization Validation

**Obligatoriu pentru fiecare piață țintă (Peru, Kenya, Chile).**

1. Open Ahrefs → Site Explorer → enter chosen platform URL (ex: medium.com)
2. Filter: Country = target market (Peru = PE, Kenya = KE, Chile = CL)
3. Filter: Position type = "Reviews" OR filter by niche keywords
4. Inspect ranking keywords for this platform in the target country:
   - IF mostly local brand keywords in positions 3–10 → platform HAS traction in that market → PROCEED
   - IF no rankings in target country → platform may not be indexed/trusted by local Google → DISQUALIFY
5. Check: does the platform publish/rank Spanish content for Peru/Chile? English for Kenya?

**Per-Market Platform Recommendations:**

**Peru:**
- Primary: Medium (Spanish articles — Google.pe ranks Medium heavily for casino/gambling Spanish terms)
- Secondary: Quora (Spanish Q&A content for "casino online Perú" questions)
- Paid: Regional Peruvian news outlets ($300–500/article, DR 50–70)
- Avoid: Reddit (low Spanish moderation support), YouTube (policy enforcement stricter in LatAm)

**Kenya:**
- Primary: Medium (English — Google.co.ke indexes Medium well)
- Secondary: Quora (English, strong Q&A traction for "best casino Kenya" searches)
- Paid: Local Kenyan news portals — Nation.Africa, Standard Media (DR 60–75, $300–600/article)
- Avoid: business.site (low effectiveness for national English terms in Kenya)

**Chile:**
- Primary: Medium (Spanish — strong penetration in Chilean Google)
- Secondary: LinkedIn Articles (Spanish, B2B angle for affiliate operators)
- Paid: Regional Chilean press — La Tercera, regional outlets ($400–700/article)
- Avoid: Reddit (r/chile moderate moderation, gambling content flagged frequently)

---

### Pas 4: Content Gap Extraction (Ahrefs — Mandatory Before Writing)

Once platform is selected, extract exact keywords to target using competitor gap analysis.

**Pași:**
1. Identify 3 top-ranking niche sites from the target SERP (casino affiliates/operators already ranking)
2. In Ahrefs → Content Gap tool:
   - Input: URL of the chosen parasite platform (ex: medium.com)
   - Competitors: 3 URLs from step 1
   - Filter: Intersect = 3 (keywords all 3 competitors rank for, but the platform does NOT yet rank for)
3. Export result → this is your **100% niche-relevant keyword list** for this platform
4. From this list, select:
   - Primary keyword: highest search volume with SERP score GOOD or better (from Pas 1)
   - 3–5 secondary/LSI keywords: to include naturally in the article
5. Log selected keywords in the content brief before writing begins

**Why this matters:** Without content gap analysis, you target keywords where the platform already has competing content or where there is no demand. The gap list gives you the exact search intent vacuum where your parasite article can dominate.

---

### Pas 5: Freshness Strategy

Freshness is a ranking factor that allows outranking older parasite articles on the same platform.

**Rules:**
- IF an existing article on the chosen platform already ranks for the target keyword → you CAN still outrank it by:
  1. Publishing a newer, more comprehensive article (link inversion: build links to your article specifically)
  2. Adding explicit date references in title and intro: "Best Online Casinos in Peru — Updated March 2026"
  3. Including current stats, recent bonus offers, and fresh operator data
- IF no existing article exists → include date in title regardless (freshness signal from day one)
- Schedule monthly updates: update date reference, refresh bonus amounts, add one new data point
- Freshness cadence: review and touch every 30 days minimum for competitive keywords

---

### Pas 6: Platform Account Preparation

Before publishing, verify:
- [ ] Account exists on selected platform (or create new account with clean email — not linked to other gambling properties)
- [ ] Account has at least 2–3 non-gambling warmup posts (for platforms like Medium, Reddit) — optional but reduces early moderation flags
- [ ] Profile bio does not contain explicit gambling language (use "affiliate marketing" or "digital marketing" framing)
- [ ] For paid outlets: verify editorial contact, confirm gambling content is accepted, negotiate dofollow link explicitly

---

### Pas 7: Decision Log and Tracking

Record the following after each platform selection decision:

| Field | Value |
|-------|-------|
| Target keyword | ex: "best online casino Peru" |
| Target market | Peru / Kenya / Chile |
| SERP opportunity score | EXCELLENT / GOOD / VIABLE / HARD |
| Selected platform | ex: Medium |
| Localization validated | YES / NO |
| Budget allocated | $0 / $300–800 / $1–2K |
| Primary keyword (from gap analysis) | ex: "casino online Perú bono" |
| Secondary keywords (3–5) | list |
| Freshness trigger date | monthly update date |
| Article URL (post-publish) | |
| Ranking position (30-day check) | |

Store in project tracking sheet (Google Sheets or Notion gambling SEO tracker).

---

## 3. Cortex Logging

```json
{
  "text": "Parasite platform selection completed for keyword: [keyword] / market: [market]. Platform selected: [platform]. SERP score: [score]. Localization validated: [yes/no]. Budget: [amount]. Primary keyword from gap analysis: [keyword].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "PARASITE-PLATFORM-SELECTION",
    "domain": "Gambling SEO",
    "rule_id": "TRAIN-SEO-004",
    "tags": ["parasite-seo", "casino-seo", "platform-selection", "gambling", "medium", "quora", "serp-audit", "content-gap", "peru", "kenya", "chile"],
    "has_enforcement_loop": true,
    "forge_version": "1.0"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution + Gambling SEO project sessions

### WHEN
La fiecare selecție de platformă pentru un keyword/piață nouă sau la revizuirea unui parasite existent

### HOW (violation detection)
- Platformă selectată fără SERP audit (Pas 1) → violation
- Conținut publicat înainte de content gap extraction (Pas 4) → violation
- Localization validation omisă pentru Peru/Kenya/Chile → violation
- Platformă selectată fără decision tree (Pas 2) → violation
- Freshness strategy neaplicată (fără dată în titlu) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

**Acțiuni interzise:**
- Selectarea platformei fără SERP audit prealabil (Pas 1)
- Publicarea conținutului pe platformă nevalidată pentru localizare în piața țintă
- Ignorarea budget gate (publicare pe platforme plătite fără buget aprobat)
- Saltul peste content gap extraction (Pas 4) — conținut fără keyword list validat = gambling pe keyword-uri greșite

### CONNECT
- TRAIN-SEO-004 → enforcement gambling SEO curriculum
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum Gambling SEO
- Projects tracker → actualizează entry gambling SEO Leo
- Prerequisite: CASINO-MARKET-ANALYSIS.md (market viability must be confirmed before platform selection)
- Downstream: PARASITE-PAGE-BUILD.md (uses selected platform and keywords from this procedure)

### VERIFY
La finalul execuției, verifică:
- [ ] Pas 1: SERP audit executat, oportunitate scorată?
- [ ] Pas 2: Decision tree aplicat, platformă selectată cu justificare?
- [ ] Pas 3: Localization validation completă pentru piața țintă?
- [ ] Pas 4: Content gap extraction din Ahrefs completat, keyword list exportat?
- [ ] Pas 5: Freshness strategy definită (dată în titlu + update schedule)?
- [ ] Pas 6: Platform account pregătit?
- [ ] Pas 7: Decision log completat în tracker?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `✅ [PROC] PARASITE-PLATFORM-SELECTION | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Parasite Platform Selection — [market]/[keyword]" | FORGE ✓ | rule: TRAIN-SEO-004 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură (platform selection) | Sonnet (Genie) |
| SERP analysis + competitive audit | Sonnet (Genie) |
| Multi-market campaign architecture | Opus |
| Validare structură FORGE | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs | SERP export (top 100), localization filter, content gap tool — mandatory for Pas 1, 3, 4 |
| Ahrefs Content Gap | Competitor intersection analysis to extract niche keyword list (Pas 4) |
| Google Search (target country) | Manual SERP spot-check to confirm platform ranking in local Google |
| Platform accounts (Medium, Quora, Reddit, LinkedIn, business.site) | Pre-prepared accounts for publishing |
| Editorial contacts (regional news outlets) | Required for paid placement in Peru/Kenya/Chile |
| Google Sheets / Notion tracker | Decision log (Pas 7) and campaign tracking |
| `memory/research/gambling-markets-global-2026.md` | Market intelligence for Peru, Kenya, Chile |
| `memory/research/seo-2026-full-spectrum.md` | Grey/black hat SEO context, PBN and tiered building reference |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași (Pas 1–7 obligatorii) |
| VK emisie | 2/2 per execuție |
| SERP audit acoperire | Top 100 rezultate exportate |
| Localization validation | Confirmat pentru fiecare piață țintă |
| Content gap keywords | Minimum 20 keywords exportate per execuție |
| Ranking check (30 zile) | Target: top 10 pentru primary keyword |
| Freshness update cadence | Monthly (30 zile) |
| Cost per placement (free platforms) | $0 |
| Cost per placement (paid news) | $300–800 regional / $1–2K US outlets |
