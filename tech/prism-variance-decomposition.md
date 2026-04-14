---
type: procedure
created: 2026-03-17
status: active
slug: prism-variance-decomposition
tags: [procedure, nexus]
---

# Variance Decomposition — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regula asociata**: TRAIN-PRISM-001
**Scope**: Decomposes financial variances into causal drivers (price, volume, mix, timing, one-time). Transforms "what changed" into "why it changed" — essential for evaluating the quality of a business's growth.
**ID**: F-PRISM-V01
**Complexitate**: INTERMEDIATE
**Locatie PRISM**: L2 (after Trend Analysis F-015, before Cross-Sectional Analysis F-016)

---

## 1. Problema

Trend analysis (F-015) identifies that a metric changed, but not why. Without variance decomposition:

- **False attribution**: Revenue grew 20% but the evaluator assumes organic growth when it was entirely price-driven (no volume growth, potentially losing customers)
- **Hidden deterioration**: Gross margin improved, but the improvement is entirely mix-driven (high-margin segment grew while low-margin shrank) — not sustainable if the low-margin segment was deliberately invested in
- **Unquantified unknowns**: Large expense variances are noted but not decomposed, making it impossible to distinguish between structural cost increases (permanent) and one-time items (non-recurring)
- **Valuation garbage-in**: L3 valuation models project forward from historical growth rates without understanding whether that growth is structural, cyclical, or one-time

Situations covered:
- Any PRISM evaluation at L2 stage — variance decomposition is required for growth quality assessment
- Revenue decomposition for multi-product/segment businesses (Saraimanic: per-event, SMSads: per-campaign)
- Operating expense analysis to identify cost drivers
- Bridge analysis between periods for investor presentations
- NOT suitable for: Single-product businesses with no pricing variation (use simple growth rate analysis instead)

---

## 2. Procedura

### Pas 1: Select Key P&L Lines for Decomposition
Select 3 key lines: (1) Revenue, (2) Gross Profit, (3) the largest operating expense category. These three capture top-line dynamics, margin quality, and cost discipline. For businesses with unusual cost structures, substitute the third line with the most analytically relevant expense.

### Pas 2: Apply Price/Volume Decomposition on Revenue
Calculate:
- **Volume Effect** = (Actual Volume - Prior Volume) x Prior ASP
- **Price Effect** = (Actual ASP - Prior ASP) x Actual Volume
- **Verification**: Volume Effect + Price Effect = Total Revenue Variance (must tie within rounding)

If the business reports units sold and average selling price, use actuals. If not, derive from revenue / units or revenue / transactions. Document data source and any assumptions.

### Pas 3: Apply Rate/Mix Decomposition for Multi-Segment Businesses
If business has multiple products or segments:
- **Rate Effect** = Sum of (actual volume per segment x rate delta per segment) — captures pricing power within each segment
- **Mix Effect** = Blended rate change attributable to segment shift — captures whether higher-margin or lower-margin segments are growing faster
- Document the favorable/unfavorable direction of mix: favorable = higher-margin segments growing faster

### Pas 4: Decompose Operating Expense Variance
Separate total opex variance into:
- **Headcount-driven**: salary increases, new hires, severance (check headcount disclosures)
- **Volume-driven**: costs that scale with revenue (commissions, COGS variable component, shipping)
- **Discretionary**: marketing, R&D, travel, consulting (management can control period-to-period)
- **Contractual/Fixed**: rent, insurance, depreciation (expected to be stable)
- **One-time**: restructuring charges, legal settlements, write-downs (non-recurring by nature)

### Pas 5: Categorize Each Driver
For every identified driver from Steps 2-4, classify as:
- **Structural**: Permanent change to business model (e.g., new pricing tier, new product line, headcount step-up for new market)
- **Cyclical**: Expected to reverse with economic or seasonal cycle (e.g., holiday demand, commodity price swings)
- **One-Time**: Non-recurring by nature (e.g., legal settlement, asset write-down, pandemic impact)
- **Timing**: Shifted between periods but net-zero over time (e.g., deal slipped from Q4 to Q1, prepaid expense recognition)
- **Unknown**: Cannot determine from available data — requires further investigation

### Pas 6: Build Text Waterfall for Revenue
Construct a narrative waterfall:
```
Prior Period Revenue: $X
  + Volume Effect:    +$A (Y%)
  + Price Effect:     +$B (Z%)
  + Mix Effect:       +$C (W%)
  + One-Time Items:   +$D
  = Current Revenue:  $X'
```
Verify: starting value + all drivers = ending value (must tie exactly).

### Pas 7: Assess Quality of Growth
Based on the decomposition, classify overall growth quality:
- **Price-led growth**: Indicates pricing power — positive signal. Check if volume held (true pricing power) or declined (trading volume for price).
- **Volume-led growth**: Indicates market share gains — positive but verify customer acquisition cost is sustainable (check CAC if available).
- **Mix-led growth (favorable)**: Higher-margin products/segments growing faster — positive for profitability trajectory.
- **Mix-led growth (unfavorable)**: Lower-margin segments growing faster — negative, indicates commoditization pressure.
- **One-time driven**: Growth not repeatable — discount in forward projections at L4.

### Pas 8: Flag Unknown Drivers
Any driver classified as "Unknown" that exceeds 5% of revenue must be flagged for immediate investigation. Options:
- Request additional data from management (if available)
- Escalate to L5 risk assessment (F-090)
- Document as a data gap in the PRISM report

### Pas 9: Document Variance Narrative
For each major P&L line decomposed, produce a standardized narrative:
```
[Line]: [Favorable/Unfavorable] $X (Y%) vs [prior period / peer median / budget]
Driver: [Primary driver identified]
Classification: [Structural / Cyclical / One-Time / Timing / Unknown]
Outlook: [One-time (no further impact) / Continues (embedded in run-rate) / Improving / Deteriorating]
```

---

## 3. Cortex Logging

```json
{
  "text": "PRISM Variance Decomposition | {company_name} | Revenue: {price_pct}% price + {volume_pct}% volume + {mix_pct}% mix | Growth quality: {quality_assessment} | Unknown drivers: {count}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-PRISM-V01",
    "rule_id": "TRAIN-PRISM-001",
    "domain": "finance",
    "pipeline_level": "L2",
    "company": "{company_name}",
    "revenue_variance_pct": "{total_pct}",
    "growth_quality": "{price-led|volume-led|mix-led|one-time}",
    "unknown_driver_count": "{count}",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["business-evaluation", "prism-pipeline", "variance-analysis", "growth-quality"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- PRISM pipeline L2 execution (after F-015 Trend Analysis)
- Materiality framework trigger: any variance exceeding materiality threshold must be decomposed using this procedure

### WHEN
- After trend analysis (F-015) identifies material variances
- Before cross-sectional analysis (F-016) — decomposition informs peer comparison context
- When materiality thresholds are breached (per PRISM Materiality Framework)

### HOW (violation detection)
- Material variance identified at F-015 but no decomposition performed -> violation (unexplained variance passed to L3)
- Variance narrative missing classification (structural/cyclical/one-time) -> violation (forward projections at L4 cannot be properly built)
- Unknown drivers > 5% of revenue not flagged -> violation (unquantified risk)
- Runner: PRISM pipeline check verifies F-PRISM-V01 output present before L3

### CONNECT
- TRAIN-PRISM-001 -> PRISM master pipeline, L2 procedure
- F-015 (Trend Analysis) -> prerequisite; trend analysis identifies what changed, this procedure explains why
- F-016 (Cross-Sectional Analysis) -> downstream; decomposition provides context for peer comparison
- F-005 (Revenue Forecasting, L4) -> variance drivers inform projection assumptions
- F-069 (Sensitivity Analysis, L4) -> identified drivers become sensitivity variables
- `procedure-health.json` -> add entry for F-PRISM-V01

### VERIFY
- [ ] All 9 steps executed? (line selection through narrative documentation)
- [ ] Price/volume decomposition ties to total variance?
- [ ] Each driver classified (structural/cyclical/one-time/timing/unknown)?
- [ ] Unknown drivers > 5% revenue flagged?
- [ ] Variance narrative produced per major line?
- [ ] VK emitted?

**VK**:
1. `[PROC] F-PRISM-V01 Variance Decomposition | {company} | Rev: {price}%P/{volume}%V/{mix}%M | Quality: {assessment} | complete`
2. `[CORTEX] "Variance Decomposition — {company}" | FORGE | rule: TRAIN-PRISM-001 | v1.0`

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| PRISM.md | Master pipeline (this is L2 procedure) | `~/.nexus/procedures/PRISM.md` |
| F-015 Trend Analysis | Prerequisite — identifies variances to decompose | `tech/business-eval-L1-L2.md` |
| F-016 Cross-Sectional Analysis | Downstream — uses decomposition context | `tech/business-eval-L1-L2.md` |
| L1 Outputs (F-002, F-004) | Source data for revenue and cost breakdowns | `tech/business-eval-L1-L2.md` |
| F-005 Revenue Forecasting (L4) | Downstream — variance drivers inform projections | `tech/business-eval-L4-L5.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Variance tie-out accuracy | Price + Volume + Mix = Total variance | 100% tie (within rounding) |
| Driver classification rate | % of variance amount classified (non-Unknown) | > 95% of revenue variance |
| Unknown flag compliance | Unknown drivers > 5% revenue properly flagged | 100% flagged |

---

## 7. Business Applications

| Business | Application | Key Decomposition |
|----------|------------|-------------------|
| Saraimanic | Revenue per event type decomposition | Volume (# events) x Price (avg ticket) x Mix (event type shift) |
| SMSads | CPL variance by campaign | Volume (leads) x Rate (CPL) x Mix (campaign channel shift) |
| Any PRISM evaluation | Standard revenue and cost decomposition | Per Steps 1-9 above |
