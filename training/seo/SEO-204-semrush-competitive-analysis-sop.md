---
type: procedure
created: 2026-03-22
status: active
slug: seo-204-semrush-competitive-analysis-sop
tags: [procedure, nexus]
---

# SEO-204: Semrush Competitive Analysis SOP

---
id: SEO-204
domain: seo
type: implementation
complexity: medium
forge_score: 3.50
source: "Semrush Academy — Off-Page SEO and AI Search Essentials (Brian Dean), Content Marketing Essentials for SEO and AI Search (Rita Cidre), AI Visibility Essentials (Rita Cidre)"
created: 2026-03-20
status: ACTIVE
version: "1.0"
rule: META-H-002
scope: "Comprehensive competitive SEO analysis using Semrush — competitor identification, traffic estimation, keyword gap analysis, backlink comparison, content strategy reverse-engineering, and actionable gap-closing plan."
---

## 1. Obiectiv

Comprehensive competitive SEO analysis using Semrush. Without systematic competitive intelligence, SEO strategy becomes guesswork. This procedure provides a data-driven approach to understanding who ranks for your target keywords, how much traffic competitors capture, what content strategies work, and where exploitable gaps exist.

Situații acoperite:
- Market entry competitive assessment for a new website/business
- Quarterly competitive landscape review for existing sites
- Client onboarding competitive analysis for agency SEO
- Post-algorithm update competitive shift analysis
- Identifying new competitors gaining organic share

> **Note**: Requires Semrush Pro plan or above (Domain Overview, Organic Research, Keyword Gap, Backlink Analytics, Backlink Gap, Traffic Analytics). Some features require Business plan (Traffic Analytics with full data).

---

## 2. Pași

### Pas 1: Identify True Organic Competitors
Open Semrush > Domain Overview > enter your domain. Scroll to "Main Organic Competitors" — these are domains competing for the same keywords (NOT necessarily business competitors). Note top 10. Then: Organic Research > "Competitors" tab for expanded list. Cross-reference with your known business competitors. Create a shortlist of 5 direct competitors (same business model + overlapping keywords). Export competitor list with: domain, Authority Score, organic traffic estimate, total organic keywords.

### Pas 2: Analyze Competitor Traffic and Trends
For each competitor: Domain Overview > check estimated organic traffic, paid traffic, traffic trend (12-month graph). Note: is their traffic growing, flat, or declining? Which months show spikes (seasonal content)? Check "Traffic Analytics" (if available): total visits, bounce rate, pages/visit, avg. visit duration. Compare all 5 competitors side-by-side. Identify the market leader and the fastest-growing challenger — both are key targets for analysis.

### Pas 3: Run Keyword Gap Analysis
Open Semrush > Keyword Gap tool. Enter your domain + up to 4 competitors. Run comparison. Review tabs:
- **Missing** — keywords ALL competitors rank for, you do NOT. Highest priority.
- **Weak** — keywords where you rank but below all competitors. Quick win opportunities.
- **Strong** — keywords where you outrank competitors. Defend these.
- **Untapped** — keywords only 1 competitor ranks for. Emerging opportunities.
Filter Missing/Weak by: Volume > 100, KD < 60, Intent = Commercial or Transactional. Export top 50 from each category. These directly inform content strategy.

### Pas 4: Reverse-Engineer Competitor Content Strategy
For each top competitor: Organic Research > "Pages" tab. Sort by estimated traffic. Identify their top 20 traffic-driving pages. Analyze patterns: content type (blog, tool, landing page), topic clusters they dominate, content length and depth, update frequency (check "Last Seen" dates). Use Content Analyzer (if available) to check social shares and backlinks per page. Document: what content formats drive most traffic for competitors? Which topics do ALL competitors cover (table stakes) vs. unique angles?

### Pas 5: Compare Backlink Profiles
Open Semrush > Backlink Analytics. Enter your domain. Note: total backlinks, referring domains, Authority Score distribution, dofollow ratio, anchor text distribution. Repeat for each competitor. Use Backlink Gap tool: enter your domain + 4 competitors. Filter results to show referring domains linking to competitors but NOT to you. Sort by Authority Score. Export top 100 — these are link prospecting targets (sites already link to your competitors, likely open to linking to you). Check for toxic backlinks in competitor profiles — avoid replicating those sources.

### Pas 6: Analyze Competitor SERP Features Presence
For each competitor: Organic Research > filter by SERP Features. Check which features they capture: Featured Snippets, People Also Ask, Local Pack, Knowledge Panel, Image Pack, Video, AI Overviews. Note: which keywords trigger featured snippets that competitors own? Can you create better-structured content to steal them? Check Semrush's "AI Overviews" report (new) to see if competitors appear in AI-generated search results. This informs AI visibility strategy per Semrush Academy's AI Visibility Essentials course.

### Pas 7: Assess Competitor PPC Strategy for SEO Intel
Advertising Research > enter competitor domain. Check: which keywords they bid on (high-value commercial intent confirmed by ad spend), ad copy messaging (reveals value propositions and offers), landing page URLs (reveals conversion-optimized pages). If a competitor pays for a keyword AND ranks organically = that keyword is extremely valuable to their business. Prioritize these for organic targeting. Export keyword + estimated CPC data.

### Pas 8: Build Competitive Gap Action Plan
Synthesize findings into actionable plan:
- **Content priorities**: topics from Keyword Gap "Missing" + competitor top pages analysis
- **Link targets**: domains from Backlink Gap export
- **SERP feature opportunities**: snippets/PAA to capture
- **Quick wins**: "Weak" keywords where small improvements can overtake competitors
- **Defensive actions**: "Strong" keywords to protect with content freshness and link building
Assign each action: priority (P0/P1/P2), responsible person, target completion date, success metric (ranking position, traffic estimate).

### Pas 9: Set Up Competitive Monitoring Dashboard
In Semrush: add all 5 competitors to Position Tracking project alongside your keywords. Enable "Competitors Discovery" for automatic alerts when new competitors enter your keyword space. Set up Brand Monitoring for competitor brand mentions (reveals PR and content marketing activity). Schedule monthly Keyword Gap re-runs to track gap closure. Configure Semrush email reports: weekly position changes, monthly competitive overview.

### Pas 10: Quarterly Competitive Review
Every 90 days: re-run full analysis (Steps 1-7). Track: your keyword overlap growth/shrinkage vs. each competitor, traffic share changes, new content strategies competitors adopted, link velocity comparison, SERP feature gains/losses. Update action plan. Identify if competitor landscape has shifted — new entrants, exits, or strategy pivots. Adjust your SEO roadmap accordingly.

---

## 3. Verificare

- [ ] True organic competitors identified (not just business competitors)?
- [ ] Traffic trends analyzed for all 5 competitors?
- [ ] Keyword Gap exported with Missing/Weak/Strong/Untapped?
- [ ] Competitor content strategy reverse-engineered (top 20 pages each)?
- [ ] Backlink Gap analyzed with export of 100+ link prospects?
- [ ] SERP feature opportunities identified (snippets, PAA, AI Overviews)?
- [ ] PPC data cross-referenced for high-value keywords?
- [ ] Action plan built with priorities, owners, and deadlines?
- [ ] Monitoring dashboard configured (Position Tracking, Brand Monitoring)?
- [ ] Quarterly review cadence scheduled?
- [ ] All steps from §2 executed?
- [ ] VK emis in sesiune?

---

## 4. Instrumente

| Instrument | Rol |
|-----------|-----|
| Semrush Domain Overview | Traffic estimates, Authority Score, competitor discovery |
| Semrush Keyword Gap | Missing/Weak/Strong keyword comparison |
| Semrush Backlink Gap | Referring domain comparison and link prospecting |
| Semrush Organic Research | Competitor keywords, top pages, SERP features |
| Semrush Advertising Research | PPC keyword intel for SEO prioritization |
| Semrush Position Tracking | Ongoing competitive position monitoring |
| Semrush Traffic Analytics | Visit metrics comparison (Business plan) |
| Semrush Brand Monitoring | Competitor mention tracking |

---

## 5. Note

- Organic competitors (keyword overlap) are often different from business competitors — always check both
- Traffic trends matter more than absolute numbers — a declining DR 70 site is weaker than a growing DR 40
- Keywords where competitors both rank AND bid on PPC are the highest-value targets — prioritize these
- Backlink Gap prospects are highest-probability targets — they already link to similar content
- Avoid replicating toxic/spammy backlink sources from competitor profiles — check quality first
- AI Overviews are a new SERP feature — track which competitors appear in AI-generated results
- Action plan without owners and deadlines is just a wish list — assign every item
- Quarterly reviews prevent strategic drift — the competitive landscape shifts constantly

---

## 6. Cortex Logging

```json
{
  "text": "Executed SEO-204: Semrush Competitive Analysis SOP — competitors identified, traffic analyzed, keyword gaps mapped, content strategy reverse-engineered, backlink profiles compared, SERP features assessed, PPC intel gathered, action plan built, monitoring active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SEO-204",
    "domain": "seo",
    "rule_id": "META-H-002",
    "tags": ["seo", "competitive-analysis", "semrush", "keyword-gap", "backlink-gap", "serp-features", "ai-visibility", "content-strategy"],
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
- Keyword Gap analysis skipped → violation
- Backlink Gap not compared → violation
- Action plan without priorities and assignments → violation
- Competitive monitoring not configured → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum seo
- SEO-201 → depends on (link targets from Backlink Gap)
- SEO-203 → depends on (keyword priorities from Keyword Gap)

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] True organic competitors identified (not just business competitors)?
- [ ] Traffic trends analyzed for all competitors?
- [ ] Keyword Gap exported with Missing/Weak/Strong/Untapped?
- [ ] Competitor content strategy reverse-engineered?
- [ ] Backlink Gap analyzed with export of link prospects?
- [ ] SERP feature opportunities identified?
- [ ] PPC data cross-referenced for high-value keywords?
- [ ] Action plan built with priorities and assignments?
- [ ] Monitoring dashboard configured?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] SEO-204 | S1-S10 DONE | competitive analysis complete`
2. `VK [CORTEX] "Semrush Competitive Analysis SOP" | FORGE 1.3 | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 8. Dependențe

| Componentă | Rol |
|-----------|-----|
| Semrush Domain Overview | Traffic estimates, Authority Score, competitor discovery |
| Semrush Keyword Gap | Missing/Weak/Strong keyword comparison |
| Semrush Backlink Gap | Referring domain comparison and link prospecting |
| Semrush Organic Research | Competitor keywords, top pages, SERP features |
| Semrush Advertising Research | PPC keyword intel for SEO prioritization |
| Semrush Position Tracking | Ongoing competitive position monitoring |
| Semrush Traffic Analytics | Visit metrics comparison (Business plan) |

---

## 9. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Competitors analyzed | 5 direct |
| Keyword gaps identified (Missing) | > 50 actionable |
| Backlink gap prospects exported | > 100 domains |
| SERP feature opportunities flagged | > 10 |
| Action plan items assigned | 100% with owner + deadline |
| Quarterly review cadence | Scheduled |
| Keyword overlap growth | Trending up vs. competitors |
