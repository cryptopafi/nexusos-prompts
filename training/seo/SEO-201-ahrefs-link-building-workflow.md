---
type: procedure
created: 2026-03-22
status: active
slug: seo-201-ahrefs-link-building-workflow
tags: [procedure, nexus]
---

# SEO-201: Ahrefs Link Building Workflow

---
id: SEO-201
domain: seo
type: implementation
complexity: medium
forge_score: 3.50
source: "Ahrefs Academy — Advanced Link Building Course (Sam Oh, 6 modules, 15 lessons), SEO Course for Beginners Module 3"
created: 2026-03-20
status: ACTIVE
version: "1.0"
rule: META-H-002
scope: "Systematic link building using Ahrefs — seed and lookalike prospecting, blitz list validation, outreach execution, and link monitoring at scale."
---

## 1. Obiectiv

Systematic link building workflow using Ahrefs tools — from prospect discovery via "seed and lookalike" approach to outreach execution and link monitoring at scale. Most link building efforts fail because they rely on generic outreach to untargeted prospects, resulting in <2% response rates. This procedure provides a repeatable, data-driven pipeline.

Situații acoperite:
- Building a link acquisition pipeline from scratch for a new domain
- Scaling link building operations beyond manual one-by-one outreach
- Recovering from a link velocity drop or competitor link gap
- Training a link building team on repeatable processes

> **Note**: Requires Ahrefs Standard plan or above (Site Explorer, Content Explorer, Batch Analysis). Outreach email tool recommended (Pitchbox, BuzzStream, or similar).

---

## 2. Pași

### Pas 1: Analyze Current Backlink Profile Baseline
Open Ahrefs > Site Explorer > enter your domain. Go to "Backlinks" report. Note: total referring domains, DR distribution (check "Referring Domains" > filter by DR ranges: 0-30, 31-50, 51-70, 71+), link velocity (new/lost per month via "New/Lost" tab), anchor text distribution (check for over-optimization). Export top 100 referring domains. This is your baseline.

### Pas 2: Identify Competitor Link Gaps
In Site Explorer > "Link Intersect" tool. Enter your domain as target. Add 3-5 top-ranking competitors for your primary keywords. Click "Show link opportunities." This reveals sites linking to competitors but NOT to you. Filter: DR > 30, Traffic > 500, One link per domain. Export results — these are your highest-priority prospects (they already link to similar content).

### Pas 3: Build Seed List via Content Explorer
Open Ahrefs > Content Explorer. Search for your target topic (e.g., "best project management tools"). Filter: DR > 30, Language = English (or target), Published = Last 12 months, "One article per domain." Sort by "Referring Domains" — pages with many backlinks indicate linkable content types. Note the content formats that attract links (studies, tools, infographics, guides). Export top 50 as "seed" list.

### Pas 4: Expand with Lookalike Prospecting
For each top seed from Step 3: open in Site Explorer > "Backlinks." These are sites that linked to similar content — high probability they will link to yours. Filter: DR > 30, Dofollow only, Link type = Content. Add qualifying domains to your master prospect list. Deduplicate against Step 2 results. Target: 200-500 qualified prospects per campaign.

### Pas 5: Validate with Blitz List
Before full outreach, create a "blitz list" of 20-30 top prospects. Send quick-validation emails (personalized, benefit-focused, under 100 words). Track response rate over 72h. If response rate < 5%, revisit your angle/value proposition. If > 10%, proceed to full-scale outreach. This prevents wasting effort on a bad campaign angle.

### Pas 6: Craft Outreach Templates
Create 3-4 email templates based on prospect type: (a) Resource page — "I noticed you link to [competitor resource], we have an updated version..." (b) Broken link — "Found a broken link on [page], here's a working alternative..." (c) Skyscraper — "We published a more comprehensive guide on [topic] with [unique data]..." (d) Ego bait — "We featured your [quote/data/tool] in our latest post..." Personalize the first line for each prospect (reference specific content). Never use generic "Dear Webmaster" — find the author/editor name.

### Pas 7: Execute Outreach in Batches
Send 20-30 emails per day (avoid spam triggers). Use merge fields for personalization at scale. Follow up exactly once after 5-7 days (no more). Track in spreadsheet or outreach tool: Sent, Opened, Replied, Link Placed, Declined. Expected conversion: 5-15% of quality prospects.

### Pas 8: Monitor New Backlinks in Ahrefs
Set up Ahrefs Alerts > "New backlinks" for your domain. Check weekly: Site Explorer > "New" referring domains. For each new link: verify it is dofollow and on a relevant page, check the linking page traffic (Site Explorer > "Organic traffic"), confirm anchor text is natural (not exact-match keyword spam). Update your backlink baseline from Step 1 monthly.

### Pas 9: Measure Campaign ROI and Iterate
Monthly review: links acquired vs. outreach sent (conversion rate), average DR of acquired links, impact on target keyword rankings (Ahrefs Rank Tracker), referring domain growth rate vs. competitors. Kill campaigns with < 3% conversion after 100 sends. Double down on angles with > 10% conversion. Document winning templates and prospect sources for team playbook.

---

## 3. Verificare

- [ ] Backlink baseline documented (total RDs, DR distribution, velocity)?
- [ ] Competitor link gaps exported (3-5 competitors)?
- [ ] Seed + lookalike prospect list > 200 domains?
- [ ] Blitz list validated with > 5% response rate?
- [ ] Outreach templates personalized per prospect type (4 types)?
- [ ] Outreach executed in batches (20-30/day)?
- [ ] Ahrefs alerts configured for new backlinks?
- [ ] Monthly ROI review scheduled?
- [ ] All steps from §2 executed?
- [ ] VK emis in sesiune?

---

## 4. Instrumente

| Instrument | Rol |
|-----------|-----|
| Ahrefs Site Explorer | Backlink analysis, competitor gaps, link monitoring |
| Ahrefs Content Explorer | Seed list discovery, linkable content research |
| Ahrefs Link Intersect | Competitor backlink gap analysis |
| Ahrefs Alerts | New backlink notifications |
| Ahrefs Rank Tracker | Keyword ranking impact measurement |
| Pitchbox / BuzzStream | Email outreach sequencing and tracking |
| Google Sheets | Prospect tracking, campaign ROI analysis |

---

## 5. Note

- Never send generic "Dear Webmaster" outreach — always find the author/editor name and personalize
- Never send more than 1 follow-up email to the same prospect — respect declines
- Always validate with a blitz list before full-scale outreach — prevents wasting effort on a bad angle
- Always run competitor link gap analysis before prospecting — these are highest-probability targets
- Cap outreach at 20-30 emails/day to avoid spam triggers and maintain deliverability
- Kill campaigns with < 3% conversion after 100 sends — double down on > 10% campaigns
- Anchor text should be natural and varied — exact-match keyword spam triggers penalties
- Link building without technical SEO health (SEO-202) limits impact — fix crawl issues first

---

## 6. Cortex Logging

```json
{
  "text": "Executed SEO-201: Ahrefs Link Building Workflow — backlink baseline set, competitor gaps identified, seed+lookalike prospects built, blitz list validated, outreach executed, link monitoring active.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "SEO-201",
    "domain": "seo",
    "rule_id": "META-H-002",
    "tags": ["seo", "link-building", "ahrefs", "outreach", "backlinks", "site-explorer", "content-explorer"],
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
- Outreach sent without blitz list validation (Pas 5) → violation
- No competitor link gap analysis before prospecting → violation
- Generic non-personalized outreach templates used → violation
- More than 1 follow-up email sent to same prospect → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry la fiecare execuție
- Training Mode → curriculum seo
- SEO-202 → complements (technical SEO impacts link-worthiness)
- SEO-203 → depends on (keyword research informs link targets)
- SEO-204 → complements (competitive analysis reveals link opportunities)

### VERIFY
- [ ] Toți pașii din §2 executați?
- [ ] Backlink baseline documented?
- [ ] Competitor link gaps exported?
- [ ] Seed + lookalike prospect list > 200 domains?
- [ ] Blitz list validated with > 5% response rate?
- [ ] Outreach templates personalized per prospect type?
- [ ] Ahrefs alerts configured for new backlinks?
- [ ] Monthly ROI review scheduled?
- [ ] VK emis în sesiune?

**VK-uri obligatorii:**
1. `VK [PROC] SEO-201 | S1-S9 DONE | link building pipeline active`
2. `VK [CORTEX] "Ahrefs Link Building Workflow" | FORGE 1.3 | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție | Sonnet |
| Audit | Opus |

---

## 8. Dependențe

| Componentă | Rol |
|-----------|-----|
| Ahrefs Site Explorer | Backlink analysis, competitor gaps, link monitoring |
| Ahrefs Content Explorer | Seed list discovery, linkable content research |
| Ahrefs Link Intersect | Competitor backlink gap analysis |
| Ahrefs Alerts | New backlink notifications |
| Outreach tool (Pitchbox/BuzzStream) | Email sequencing and tracking |

---

## 9. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 |
| Outreach response rate | > 10% |
| Link conversion rate | > 5% of prospects |
| Average acquired link DR | > 40 |
| Link velocity | > 10 new referring domains/month |
| Campaign ROI review | Monthly |
