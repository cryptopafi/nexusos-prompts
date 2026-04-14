---
type: procedure
created: 2026-03-22
status: active
slug: m-019-competitive-seo-analysis
tags: [procedure, nexus]
---

# Competitive SEO Analysis — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Structured competitive SEO analysis covering competitor identification, domain authority comparison, keyword and content gaps, backlink gap analysis, SERP feature ownership, and competitive content strategy benchmarking to inform SEO prioritization.

---

## 1. Problema

SEO strategy built without competitive intelligence is directionally blind. Without knowing what competitors rank for, what content they publish, and where they earn links, campaigns allocate effort toward wrong priorities and miss high-leverage opportunities that competitors have already validated.

Types of situations covered:
- Campaign kickoff — establishing competitive baseline before strategy formulation
- Keyword and content planning — identifying proven topics worth targeting
- Link building prioritization — finding link sources already linking to competitors
- SERP position defense — monitoring competitive ranking moves
- Market entry — understanding the SEO landscape before entering a new niche

---

## 2. Procedura

### Pas 1: Identify Top 5 Organic Competitors
In Semrush or Ahrefs, enter the target domain and pull the "Organic Competitors" report. Select the top 5 competitors by keyword overlap percentage — these are the sites competing for the same SERP real estate. Verify they are true business competitors, not just topical overlaps (e.g., Wikipedia, Reddit). If needed, supplement with manual SERP analysis: search top 10 priority keywords and record which domains appear most frequently across all SERPs. Finalize the competitor set of 5 and document all 5 in the analysis brief.

### Pas 2: Compare Domain Authority and Traffic Estimates
For each of the 5 competitors and the target domain, record:
- Domain Rating (Ahrefs) or Domain Authority (Moz)
- Estimated monthly organic traffic (Semrush/Ahrefs)
- Number of referring domains (total + dofollow)
- Number of indexed pages
- Top-performing content pages by organic traffic
Create a benchmark table. Identify the authority gap: how far ahead or behind each competitor is. This sets realistic timeline expectations for organic growth.

### Pas 3: Keyword Gap Analysis
Run Semrush Keyword Gap or Ahrefs Content Gap: input the target domain and all 5 competitors. Filter for keywords that competitors rank for (positions 1-20) but the target domain does not rank for at all. Sort by volume descending. Export top 200 keyword gaps. These are proven topics — competitors have already validated there is search demand. Cross-reference with keyword research (M-011) clusters to identify which gaps are highest strategic priority.

### Pas 4: Content Gap Analysis
Identify the top 20 traffic-driving content pieces for each competitor using Semrush "Top Pages" or Ahrefs "Top Content" report. Categorize competitor content by type: pillar guides, comparison articles, tool/calculator pages, data studies, listicles. Flag content formats that appear in multiple competitors' top pages — these are proven formats for the niche. Compare against target domain's existing content. Pages with no equivalent = content gap. Create a "Competitor Content Replication + Improvement" list.

### Pas 5: Backlink Gap Analysis
Run Semrush Backlink Gap or Ahrefs Link Intersect: find domains linking to 2 or more competitors but not to the target domain. These websites have already demonstrated willingness to link to content in this niche — they are the highest-probability outreach targets. Export top 100 by DR, sorted highest to lowest. Feed into M-013 (Link Building) as the outreach queue. Document the backlink gap count and average DR of missed link opportunities.

### Pas 6: SERP Feature Analysis
For the target keyword set, analyze SERP feature distribution: which competitors own featured snippets, AI Overviews, People Also Ask boxes, local packs, image packs, and video carousels. Document feature ownership by competitor. SERP features owned by competitors represent specific optimization opportunities — each owned feature can be targeted with the corresponding procedure (M-018 for featured snippets, M-015 for local pack, M-016 for AI Overviews).

### Pas 7: Competitive Content Strategy Benchmarking
Synthesize findings into a competitive strategy brief:
- Top 3 keyword gap opportunities to target first
- Top 5 content pieces to create or improve (based on competitor content gaps)
- Top 20 link building prospects from backlink gap
- SERP features to pursue with associated procedures
- Authority timeline: realistic milestones given the current gap
Deliver to client/stakeholder as the competitive intelligence foundation for the SEO roadmap.

---

## 3. Cortex Logging

```json
{
  "text": "Competitive SEO Analysis completed for [domain]. Competitors: [5 domains]. Keyword gaps: [n]. Content gaps: [n]. Backlink gap opportunities: [n]. SERP features analyzed. Strategy brief delivered.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-019",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["competitive-analysis", "keyword-gap", "backlink-gap", "serp-features", "seo-strategy"],
    "has_enforcement_loop": true,
    "forge_version": "1.3"
  }
}
```

---

## 4. Enforcement Loop

### WHERE
WISH Step H (Post-H skill extraction) + Training Mode execution

### WHEN
La fiecare execuție a procedurii în context real sau training

### HOW (violation detection)
- Procedura executată fără toți pașii din §2 → violation
- Output fără VK → violation
- Competitor set definit fără keyword overlap verification → violation
- Backlink gap analysis absent din deliverable → violation
- Analiza competiției realizată pe mai puțin de 3 competitori → violation
- Gap analysis realizat fără tool SEO (Ahrefs/SEMrush/Moz) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Outputs alimentează M-011 (keyword), M-013 (link building), M-014 (content audit)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] 5 competitori identificați și documentați?
- [ ] Keyword + content + backlink gap toate analizate?
- [ ] Strategy brief cu priorități livrat?

**VK-uri obligatorii:**
1. `✅ [PROC] M-019 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "Competitive SEO Analysis" | FORGE ✓ | rule: META-H-002 | v1.0`

### MODEL ROUTING
| Activitate | Model |
|-----------|-------|
| Execuție procedură | Sonnet (Genie) |
| Audit periodic | Opus |
| Validare structură | AUDIT-PRO |

---

## 5. Dependențe

| Componentă | Rol |
|-----------|-----|
| Semrush / Ahrefs | Competitor identification and gap analysis |
| Semrush Backlink Gap / Ahrefs Link Intersect | Backlink gap analysis |
| Google Search (manual) | SERP feature verification |
| M-011, M-013, M-014 | Downstream procedures fed by analysis outputs |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| Competitors analyzed | Exactly 5 |
| Keyword gaps exported | Minimum 200 keywords |
| Backlink gap prospects | Minimum 100 domains documented |
| Strategy brief delivery | Within 5 business days of analysis |
