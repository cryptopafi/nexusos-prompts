---
type: procedure
created: 2026-03-22
status: active
slug: m-093-geo-metrics-tracking
tags: [procedure, nexus]
---

# GEO Metrics Tracking and Optimization — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-04
**Versiune**: 1.0
**Regulă asociată**: META-H-002
**Scope**: Systematic GEO (Generative Engine Optimization) metrics tracking covering tool setup, KPI definition, AI visibility benchmarking, weekly monitoring cadence, content-change correlation, and data-driven optimization cycles to measure and improve presence in AI-generated search results.

---

## 1. Problema

Generative AI search (Google AI Overviews, Perplexity, ChatGPT) represents a new traffic layer that traditional SEO metrics do not capture. Without GEO-specific tracking, teams are blind to AI visibility changes — they cannot measure the impact of GEO content optimizations, detect competitor AI visibility gains, or demonstrate GEO ROI to stakeholders.

Types of situations covered:
- New GEO program launch — establishing baseline and tracking infrastructure
- Post-optimization measurement — correlating content changes with AI citation changes
- Monthly GEO reporting — reporting AI visibility alongside traditional SEO metrics
- Competitive GEO intelligence — monitoring competitor AI citation rates
- Google SGE / AI Overview impact assessment — measuring traffic displacement from traditional rankings

---

## 2. Procedura

### Pas 1: Set Up GEO Tracking Tools
Configure the following tracking stack:
- **Google Search Console**: Enable "Search Type: AI Overviews" filter under Performance report. This shows impressions and clicks from queries where an AI Overview appeared. Set up a saved filter for this view.
- **Semrush AI Overview tracker**: Add target domain and top 50 keywords to track. Semrush flags when your URLs appear as sources in Google AI Overviews.
- **Manual query log**: Create a spreadsheet with columns — Date, Query, Platform (Google/Perplexity/ChatGPT/Bing Copilot), Your site cited (Yes/No), Competitor cited (name), Citation URL, Response excerpt.
- **Brand monitoring**: Set up Google Alerts for brand name + key product/service terms.

### Pas 2: Define GEO KPIs
Establish the core GEO metric set:
- **AI Citation Rate**: Percentage of tracked queries where your domain is cited in AI-generated responses. Formula: (Queries with your citation / Total tracked queries) x 100.
- **AI Overview Impression Share**: From Search Console — impressions where AI Overview appeared / total impressions. Tracks how often your content is in AI Overview-triggering SERPs.
- **AI-attributed CTR**: Click-through rate on queries where AI Overview appears (typically lower than non-AI SERPs due to zero-click behavior).
- **AI Visibility Score**: Composite score = citations across platforms weighted by platform traffic share.
Document baseline values for each KPI at program start. All future tracking measures against this baseline.

### Pas 3: Benchmark Current AI Visibility
Execute the baseline measurement:
- Manually query all 50 tracked keywords in Google (AI Overviews enabled), Perplexity, and ChatGPT. Record results in the manual query log.
- Export Search Console AI Overview impressions for the past 3 months.
- Run Semrush AI Overview tracker initial scan.
Calculate baseline AI Citation Rate. Identify which competitor domains appear most frequently across all AI responses — these are the current "AI-trusted" sources in your niche. Document the top 5 AI-favored domains per topic cluster.

### Pas 4: Track Changes Weekly
Every Monday morning, run the weekly GEO tracking cycle:
1. Query all 50 tracked keywords in Google AI Overviews — record citation changes
2. Spot-check 10 queries in Perplexity and 10 in ChatGPT
3. Update the manual query log with new data
4. Review Semrush AI Overview tracker for automated citation alerts
5. Note any Search Console AI Overview impression changes vs. prior week
Calculate weekly AI Citation Rate delta. Flag queries where citation was lost (negative signal) or gained (positive signal). Note any Search Console AI impression trend changes.

### Pas 5: Correlate GEO Changes with Content Updates
Maintain a content change log with: date of change, URL modified, type of change (new content, E-E-A-T addition, schema update, structural reformat). Overlay content change dates on GEO metric trend charts. After each content change, allow 2-4 weeks for AI systems to re-index and update responses. Identify which content change types correlate with positive GEO signals: E-E-A-T additions, structured Q&A format, new statistics or original data, schema markup additions.

### Pas 6: Optimize Based on GEO Data
Monthly optimization cycle based on tracking data:
- **Queries with lost citations**: Review competing content that displaced yours. Identify what they have that you don't (more recent data, better structure, stronger credentials). Update your content accordingly.
- **Queries with no citations**: These are acquisition targets. Apply M-016 (GEO Content Strategy) to optimize these pages specifically.
- **High AI impression but low CTR**: Likely AI Overview is satisfying the query fully. Consider whether to optimize for click-through (add value beyond what AI shows) or accept zero-click visibility as a brand signal.
- **New citation opportunities**: Identified through competitor monitoring — new queries competitors own in AI that you don't.

---

## 3. Cortex Logging

```json
{
  "text": "GEO Metrics Tracking cycle completed for [domain]. AI Citation Rate: [x]%. AI Overview impressions: [n]. Queries tracked: [n]. Changes correlated with [n] content updates. Optimization actions: [n].",
  "collection": "training",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "M-093",
    "domain": "SEO & Content",
    "rule_id": "META-H-002",
    "tags": ["geo-tracking", "ai-overview", "ai-citations", "perplexity", "sge", "geo-metrics"],
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
- GEO tracking executat fără baseline establit → violation
- Content change log absent → corelarea cu GEO changes imposibilă → violation
- Tracking GEO setup fără baseline pre-implementare → violation
- Metrici GEO raportate fără segmentare geografică → violation
- Runner: Opus audit periodic + AUDIT-PRO batch

### CONNECT
- META-H-002 → enforcement FORGE standard
- `procedure-health.json` → adaugă entry
- Training Mode → procedura face parte din curriculum SEO & Content
- Depinde de M-016 (GEO Strategy) pentru optimization actions
- Output alimentează monthly SEO report (M-017)

### VERIFY
La finalul execuției, verifică:
- [ ] Toți pașii din §2 executați?
- [ ] Output satisface criteriile din §1?
- [ ] VK emis în sesiune?
- [ ] Toate 4 tracking tools configurate?
- [ ] Baseline AI Citation Rate documentat?
- [ ] Weekly tracking cadence setat și activ?

**VK-uri obligatorii:**
1. `✅ [PROC] M-093 | §1✓ §2✓ §3✓ §4✓ VER✓ | complete`
2. `✅ [CORTEX] "GEO Metrics Tracking and Optimization" | FORGE ✓ | rule: META-H-002 | v1.0`

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
| Google Search Console | AI Overview impression and CTR data |
| Semrush AI Overview Tracker | Automated citation monitoring |
| Manual query log (spreadsheet) | Cross-platform citation tracking |
| M-016 GEO Content Strategy | Optimization actions triggered by tracking data |
| M-017 SEO Reporting | GEO metrics integration in client reports |

---

## 6. Metrics

| Metrică | Target |
|---------|--------|
| Completion rate | 100% pași |
| VK emisie | 2/2 per execuție |
| AI Citation Rate (baseline) | Documented at program start |
| AI Citation Rate trend | Month-over-month increase |
| Weekly tracking consistency | 52/52 weeks executed |
| Content-change correlation | 100% changes logged with dates |
