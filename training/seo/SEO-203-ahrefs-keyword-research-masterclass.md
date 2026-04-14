---
type: procedure
created: 2026-03-22
status: active
slug: seo-203-ahrefs-keyword-research-masterclass
tags: [procedure, nexus]
---

# SEO-203: Ahrefs Keyword Research Masterclass

---
id: SEO-203
domain: seo
type: implementation
complexity: medium
forge_score: 3.50
source: "Ahrefs Academy — SEO Course for Beginners Module 1 (Sam Oh, 4 lessons), How to Use Ahrefs (Keywords Explorer modules)"
created: 2026-03-20
status: ACTIVE
version: "1.0"
rule: META-H-002
scope: "End-to-end keyword research workflow using Ahrefs — seed discovery, keyword expansion, intent analysis, difficulty assessment, content gap analysis, topic clustering, and ranking monitoring."
---

## 1. Obiectiv

End-to-end keyword research workflow using Ahrefs Keywords Explorer and Site Explorer — from seed discovery to keyword mapping, intent analysis, difficulty assessment, and content planning. Without a systematic approach, teams target keywords that are too competitive, too low-volume, or misaligned with searcher intent.

Situații acoperite:
- Building a keyword strategy from scratch for a new site
- Expanding keyword coverage for an existing site
- Finding content gaps vs. competitors
- Quarterly keyword strategy refresh
- Niche/market entry keyword assessment

> **Note**: Requires Ahrefs Standard plan or above (Keywords Explorer, Site Explorer, Content Explorer). Google Search Console access recommended for existing sites.

---

## 2. Pași

### Pas 1: Define Seed Keywords and Business Topics
List 5-10 broad "seed" topics that describe what the business sells or does (e.g., "project management software", "CRM for small business", "email marketing"). For each seed, note: the product/service it maps to, the target audience, and the commercial intent level (informational, commercial, transactional). This grounds all keyword research in business objectives, not vanity metrics.

### Pas 2: Expand Seeds in Keywords Explorer
Open Ahrefs > Keywords Explorer. Enter all seed keywords. Select target country. Review the overview: search volume, keyword difficulty (KD), clicks, CPC. Go to "Matching Terms" report — this expands seeds into hundreds/thousands of related keywords. Apply filters: Volume > 100 (adjust for niche), KD < 40 (realistic difficulty for your DR), filter by word count > 3 (long-tail, more specific). Export full list. Repeat with "Related Terms" for semantically connected keywords not in matching terms.

### Pas 3: Analyze Searcher Intent for Each Keyword Cluster
For each keyword group, check the SERP (click "SERP" button in Keywords Explorer). Classify intent based on what currently ranks:
- **Informational** (blog posts, guides, how-tos rank) — create content to capture awareness
- **Commercial Investigation** (comparison, review, "best X" pages rank) — create comparison/review content
- **Transactional** (product/landing pages rank) — optimize product/service pages
- **Navigational** (brand-specific, homepage results) — usually skip unless your brand
Tag each keyword with its intent type. This prevents creating the wrong content type for a keyword.

### Pas 4: Assess Keyword Difficulty Realistically
Do NOT rely solely on Ahrefs KD score. For each target keyword: check the top 10 SERP results manually. Note: DR of ranking domains (if all are DR 70+, a DR 30 site will struggle), number of referring domains to ranking pages (more = harder), content quality and depth of ranking pages, whether SERP features dominate (featured snippet, PAA, video). Create a realistic difficulty matrix: Easy (DR < yours, <10 referring domains), Medium (DR similar, 10-50 RDs), Hard (DR >> yours, 50+ RDs). Prioritize Easy and Medium.

### Pas 5: Find Competitor Content Gaps
Open Ahrefs > Site Explorer > enter competitor domain. Go to "Organic Keywords" report. Filter: Position 1-10 (keywords they rank for). Export. Repeat for 2-3 competitors. In Keywords Explorer > "Content Gap" tool: enter your domain vs. 3 competitors. This shows keywords ALL competitors rank for but YOU do not. Filter by volume and difficulty. These are immediate content opportunities — the topic is proven, you just need to create better content.

### Pas 6: Cluster Keywords by Topic
Group related keywords into topic clusters. Each cluster = one piece of content. Rules: keywords with the same SERP overlap (>50% same URLs ranking) belong in one cluster. Use Ahrefs "Parent Topic" feature — it shows the broader topic a keyword belongs to. Structure: 1 pillar page (broad topic, high volume) + 5-10 cluster pages (specific long-tails) linked to pillar. Map each cluster to a URL (existing or to-create).

### Pas 7: Create Keyword Map Document
Build a keyword map spreadsheet with columns: Target Keyword, Search Volume, KD, Intent Type, Parent Topic, Cluster Name, Mapped URL (existing or planned), Current Position (if any), Priority (P0/P1/P2), Content Status (exists/needs update/to create). Sort by priority. P0 = high volume + low difficulty + high commercial intent. P1 = medium volume or medium difficulty. P2 = informational/long-tail for topical authority.

### Pas 8: Validate with Existing Data (for Established Sites)
If site has Google Search Console data: cross-reference keyword map with GSC "Performance" report. Find keywords where site already appears on page 2-3 (positions 11-30) — these are "striking distance" keywords. Prioritize these for quick wins: optimize existing page, add internal links, refresh content. Also check Ahrefs Site Explorer > "Organic Keywords" > filter Position 11-30 for the same purpose.

### Pas 9: Build Content Calendar from Keyword Map
Convert the keyword map into a content calendar. For each P0 keyword cluster: assign target publish date, content type (based on intent from Step 3), word count target (based on competing content length), unique angle/differentiator, and internal linking plan. Set quarterly review: re-run Steps 2 and 5 to find new opportunities and check ranking progress on published content.

### Pas 10: Monitor Keyword Rankings
Set up Ahrefs Rank Tracker project. Add all target keywords from the keyword map. Configure: tracking frequency = weekly, device = desktop + mobile, location = target country/city. After 30 days of tracking: identify keywords trending up (double down with internal links), keywords flat (check content quality, add unique data), keywords declining (investigate — algorithm change, competitor improvement, technical issue). Monthly reporting: tracked keywords in top 3/10/20, total estimated organic traffic from tracked keywords.

---

## 3. Verificare

- [ ] Seed keywords tied to business objectives (not vanity)?
- [ ] Keywords expanded via Matching + Related Terms?
- [ ] Intent classified for each keyword cluster?
- [ ] Difficulty assessed with manual SERP analysis, not just KD score?
- [ ] Competitor content gaps identified (3+ competitors)?
- [ ] Keywords clustered with topic/pillar mapping?
- [ ] Keyword map document created with all required columns?
- [ ] Striking distance keywords identified (positions 11-30)?
- [ ] Content calendar built from keyword map?
- [ ] Rank Tracker configured for monitoring?
- [ ] All steps from §2 executed?
- [ ] VK emis in sesiune?

---

## 4. Instrumente

| Instrument | Rol |
|-----------|-----|
| Ahrefs Keywords Explorer | Keyword expansion, difficulty scoring, SERP analysis |
| Ahrefs Site Explorer | Competitor keywords, content gap, organic traffic data |
| Ahrefs Content Explorer | Linkable content research for topic validation |
| Ahrefs Rank Tracker | Ongoing position monitoring (weekly) |
| Google Search Console | Striking distance keyword validation, CTR data |
| Google Sheets | Keyword map document, content calendar |

---

## 5. Note

- Never rely solely on Ahrefs KD score for difficulty — always check actual SERP results manually
- Never skip intent analysis before content creation — wrong content type for a keyword wastes resources
- Always run competitor content gap analysis — proven topics with existing demand are safer bets
- Always cluster keywords before planning content — one piece of content can target an entire cluster
- Striking distance keywords (positions 11-30) are the highest-ROI quick wins — optimize these first
- Quarterly refresh is mandatory — keyword landscape shifts with algorithm updates and competitor moves
- Long-tail keywords (3+ words) often have lower competition and higher conversion intent
- Keyword map should be a living document — update positions and status monthly

---

## 6. Cortex Logging

```json
{
  "text": "Executed SEO-203: Ahrefs Keyword Research Masterclass — seeds defined, keywords expanded, intent classified, difficulty assessed, content gaps found, clusters built, keyword map created, rankings tracked.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SEO-203",
    "domain": "seo",
    "rule_id": "META-H-002",
    "tags": ["seo", "keyword-research", "ahrefs", "keywords-explorer", "content-gap", "serp-analysis", "topic-clusters"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 7. Enforcement Loop

### WHERE
WISH Step H + Training Mode

### WHEN
La fiecare execuție

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Keywords selected without intent analysis (Pas 3) → violation
- KD used as sole difficulty indicator without SERP check (Pas 4) → violation
- No competitor content gap analysis performed → violation
- Keywords not clustered before content planning → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum seo
- SEO-201 → depends on (link building targets keyword priorities)
- SEO-202 → complements (technical health enables ranking)
- SEO-204 → complements (competitive analysis informs keyword strategy)

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Seed keywords tied to business objectives?
- [ ] Keywords expanded via Matching + Related Terms?
- [ ] Intent classified for each keyword cluster?
- [ ] Difficulty assessed with SERP analysis, not just KD score?
- [ ] Competitor content gaps identified?
- [ ] Keywords clustered with topic/pillar mapping?
- [ ] Keyword map document created with all required columns?
- [ ] Content calendar built from keyword map?
- [ ] Rank Tracker configured for monitoring?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] SEO-203 | S1-S10 DONE | keyword research complete`
2. `VK [CORTEX] "Ahrefs Keyword Research Masterclass" | FORGE 1.3 | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 8. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs Keywords Explorer | Keyword expansion, difficulty, SERP analysis |
| Ahrefs Site Explorer | Competitor keywords, content gap, organic traffic |
| Ahrefs Content Explorer | Linkable content research for topic validation |
| Ahrefs Rank Tracker | Ongoing position monitoring |
| Google Search Console | Striking distance keyword validation |

---

## 9. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Keywords mapped | > 100 per project |
| Content gaps identified | > 20 opportunities |
| Topic clusters defined | > 5 pillar topics |
| Striking distance keywords found | > 10 (positions 11-30) |
| Rank Tracker configured | Yes, weekly cadence |
| Quarterly refresh | Scheduled |
