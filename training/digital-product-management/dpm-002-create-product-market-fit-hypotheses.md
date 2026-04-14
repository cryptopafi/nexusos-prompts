---
type: procedure
created: 2026-03-20
status: active
slug: dpm-002-create-product-market-fit-hypotheses
tags: [procedure, nexus]
---

# DPM-002 — Create Product-Market Fit Hypotheses
**Version**: 2.0 · **FORGE**: PASS · **Domain**: digital-product-management

## Problema
Products are built without validating whether a real market exists — burning months of engineering time on products nobody wants, because PMF assumptions remain implicit and untested.

## Procedura
### Prerequisites
- Initial product concept or existing product seeking expansion
- Access to target customer segment (minimum 10 potential users for interviews)
- Lean Canvas or Business Model Canvas template
- Analytics tool (Amplitude, Mixpanel, or Google Analytics)

### Steps
1. **Define the target customer segment precisely** — Write a customer profile: industry, company size (or demographic for B2C), role/title, and the specific context where the pain occurs. Use the format: "[Role] at [Company Type] who [Specific Behavior/Context]." Validate with at least 3 data points (market reports, CRM data, or prior research). Avoid vague segments like "small businesses."
2. **Articulate the core problem using JTBD** — Write 3-5 Jobs-to-Be-Done statements: "When [situation], I want to [motivation], so I can [expected outcome]." Rank by frequency and severity. Use the Opportunity Score formula: Importance + (Importance - Satisfaction) to identify underserved jobs. Tools: Notion, Miro, or Productboard Insights.
3. **Map existing alternatives and switching barriers** — List every way customers currently solve the problem (competitors, workarounds, spreadsheets, manual processes, doing nothing). For each: document cost, time investment, satisfaction level (1-5), and switching barriers. This reveals the "good enough" threshold your product must exceed.
4. **Draft falsifiable PMF hypotheses** — Write 3-5 hypotheses: "We believe [target segment] will [adopt behavior / pay $X / switch from Y] because [our product] solves [specific JTBD] better than [current alternative] by [measurable improvement]." Each must have a clear pass/fail metric and timeframe.
5. **Set the Sean Ellis PMF benchmark** — Survey question: "How would you feel if you could no longer use [product]?" Target: >40% answering "Very disappointed." Pre-launch proxies: >30% email signup conversion from landing page, or >50% of interview subjects expressing willingness to pay at stated price point.
6. **Design validation experiments per hypothesis** — For each hypothesis, select: (a) Problem interviews (n=15-20, qualitative), (b) Landing page smoke test (Unbounce/Carrd — measure signup rate), (c) Concierge MVP (manual delivery to 5-10 users), (d) Wizard of Oz prototype. Set success criteria, sample size, and max 2-week timeline per experiment.
7. **Execute the Lean Startup Build-Measure-Learn loop** — Run the cheapest experiment first. Capture data in a structured experiment log: hypothesis, method, sample size, result, confidence level, decision (pivot/persevere/zoom-in). Track in Notion or Google Sheets.
8. **Analyze retention as PMF signal** — For products with existing users, check Week 1, Week 4, and Week 8 retention curves in Amplitude or Mixpanel. PMF benchmarks: Week 8 retention >25% for B2C, >60% for B2B SaaS. Flat or rising retention curve = PMF signal. Declining = hypothesis needs revision.
9. **Synthesize and classify results** — Consolidate across all experiments. Classify each hypothesis as Validated, Invalidated, or Inconclusive. For validated ones, write a PMF Statement: "[Product] achieves PMF for [segment] solving [job] with [evidence: metrics, quotes, retention data]." Present to leadership with recommendation: scale, iterate, or pivot.
10. **Set ongoing PMF monitoring** — Track Sean Ellis score quarterly, monitor DAU/MAU ratio (target >20% B2C, >40% B2B), and watch organic growth rate (word-of-mouth referral %). PMF is not permanent — set alerts for declining metrics in your analytics dashboard.

### Verification
- [ ] 3-5 JTBD statements written with Opportunity Scores
- [ ] 3-5 falsifiable PMF hypotheses documented with pass/fail criteria
- [ ] At least 2 validation experiments executed with results logged
- [ ] Retention curves analyzed (if applicable) with PMF benchmark comparison
- [ ] PMF Statement drafted for validated hypotheses

## Cortex Logging
- collection: procedures
- tags: digital-product-management, training, product-market-fit, lean-startup, JTBD, validation

## Enforcement Loop
- WHERE: Pre-launch validation, new market entry, pivot decisions
- WHEN: Before committing engineering resources to a new product/feature area
- HOW: Follow steps 1-10; each hypothesis must be tested before scaling investment
- CONNECT: DPM-009 (Lean Startup), DPM-010 (JTBD), DPM-021 (Concierge MVP), DPM-025 (JTBD statements)

## Dependente
- Access to 10+ target customers for interviews
- Landing page tool (Unbounce, Carrd) for smoke tests
- Analytics tool with retention analysis (Amplitude, Mixpanel)

## Metrics
- Sean Ellis PMF score: target >40% "Very disappointed"
- Hypothesis validation rate: validated / total tested
- Time-to-PMF: weeks from first hypothesis to validated PMF statement
- DAU/MAU ratio post-launch: >20% B2C, >40% B2B
