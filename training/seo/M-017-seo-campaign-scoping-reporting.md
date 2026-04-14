---
type: procedure
created: 2026-03-22
status: active
slug: m-017-seo-campaign-scoping-reporting
tags: [procedure, nexus]
---

# SEO Campaign Scoping and Client Reporting — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: End-to-end SEO campaign scoping process from client discovery through KPI definition, reporting dashboard setup, and monthly reporting cadence, ensuring alignment between client business goals and SEO deliverables.

---

## 1. Problema

SEO campaigns fail not because of technical deficiencies but because of misaligned expectations, undefined KPIs, and reports that present data without connecting it to business outcomes. Without structured scoping and reporting, clients question the value of SEO investment and agencies lose retainers.

Types of situations covered:
- New client onboarding — defining scope, deliverables, and success metrics
- Campaign renewal — re-scoping based on initial results
- Reporting improvement — transitioning from vanity metrics to business impact metrics
- Multi-channel client — integrating SEO reporting with broader digital marketing reporting
- Agency pitch — preparing data-backed scope proposals

---

## 2. Procedura

### Pas 1: Client Discovery Call
Conduct a structured 60-minute discovery call. Document: business model and revenue drivers, primary products/services to promote, target audience and buyer personas, geographic markets, existing digital marketing channels, historical SEO performance (if any), competitor perception ("who do you see as your biggest online competitors?"), timeline expectations, and budget range. Record or take detailed notes. Discovery outputs feed all subsequent steps.

### Pas 2: Technical Audit Baseline
Execute M-010 (Technical SEO Audit) as the first deliverable. Technical baseline establishes: current crawl health, indexability status, Core Web Vitals scores, and any blockers that must be resolved before traffic growth is possible. Document the current Domain Rating, organic traffic baseline (last 3 months from GA4), and top 20 ranking keywords from Search Console. This is the campaign starting point.

### Pas 3: Keyword Universe Definition
Execute M-011 (Keyword Research) to define the keyword universe for the campaign. Deliver a prioritized keyword list with clusters mapped to existing and target pages. Confirm with the client which keyword clusters are highest priority for their business. This alignment step prevents scope creep and focuses the campaign on terms that drive actual business outcomes (leads, sales) not just traffic.

### Pas 4: Content Strategy Outline
Based on keyword gaps and the content audit (M-014), define the content deliverables for the campaign:
- Monthly new content pieces (blog posts, landing pages, pillar pages)
- Content updates for existing underperforming pages
- Technical on-page work (M-012) deliverables
Define ownership: who writes, who reviews, who publishes. Set editorial calendar for first 3 months. Content plan should map each piece to a keyword cluster and a business objective (awareness, lead generation, conversion).

### Pas 5: KPI Definition and Target Setting
Define 3-5 primary KPIs aligned to business goals:
- **Traffic KPI**: X% increase in organic sessions over 6 months
- **Rankings KPI**: X keywords in top 10 positions within 6 months
- **Conversion KPI**: X organic-attributed leads or sales per month
- **Visibility KPI**: Domain Rating improvement from X to Y
- **GEO KPI** (if applicable): AI Overview appearances for primary keywords
Set realistic targets based on baseline data and competitive benchmarks. Document targets in the campaign brief. Avoid committing to ranking guarantees — frame as directional targets.

### Pas 6: Reporting Dashboard Setup
Build a reporting dashboard in Looker Studio (Google Data Studio) connected to: Google Analytics 4 (organic traffic, conversions, revenue), Google Search Console (impressions, clicks, average position, CTR), Semrush or Ahrefs (keyword rankings, Domain Rating trend). Create views for: executive summary (high-level KPI status), rankings tracker (top 50 keywords with trend), traffic overview (organic sessions, pages, conversion paths). Share dashboard access with client.

### Pas 7: Monthly Reporting Cadence
Deliver a monthly SEO report covering: KPI status (on track / behind / ahead), organic traffic trend vs. previous month and year-over-year, top ranking improvements and losses, new content published and initial performance, technical issues resolved, and next month's planned activities. Lead with business impact (leads, revenue, cost-per-acquisition reduction) before presenting technical metrics. Conduct a 30-minute review call per report cycle.

---

## 3. Cortex Logging

```json
{
  "text": "SEO Campaign Scoped for [client/domain]. KPIs defined: [n]. Dashboard live. Keyword universe: [n] clusters. Content plan: [n] pieces/month. Reporting cadence: monthly.",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-017",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["seo-reporting", "campaign-scoping", "kpi", "client-management", "looker-studio"],
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
- KPIs definite fără baseline data → violation
- Dashboard livrat fără connection la GA4 și GSC → violation
- Raport SEO livrat fără comparație față de perioada anterioară → violation
- KPIs raportate fără segmentare pe tipuri de trafic (organic/direct) → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Depinde de M-010, M-011, M-014 pentru baseline data

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] KPIs definite și documentate în campaign brief?
- [ ] Looker Studio dashboard live și shared cu client?
- [ ] First monthly report scheduled?

**VK-uri obligatorii:**
1. `✅ [PROC] M-017 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "SEO Campaign Scoping and Client Reporting" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Analytics 4 | Organic traffic and conversion tracking |
| Google Search Console | Ranking and impression data |
| Looker Studio | Reporting dashboard |
| Semrush / Ahrefs | Keyword rankings and domain metrics |
| M-010, M-011, M-014 | Baseline data inputs for scoping |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| KPI alignment | 100% KPIs tied to business objectives |
| Dashboard delivery | Within 5 business days of campaign start |
| Monthly report delivery | Within 5th business day of each month |
