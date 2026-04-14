---
type: procedure
created: 2026-03-17
status: active
slug: prism-operational-maturity-score
tags: [procedure, nexus]
---

# Operational Maturity Score — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regula asociata**: TRAIN-PRISM-001
**Scope**: Evaluates the maturity of a business's finance function — a non-financial but predictive indicator for data quality and operational risk. Produces a numerical score (0-14) mapped to valuation discount recommendations.
**ID**: F-PRISM-O01
**Complexitate**: BASIC
**Locatie PRISM**: L2 (after Non-Financial KPI Linking F-009)

---

## 1. Problema

Financial statements tell you what happened, but the maturity of the finance function tells you how much to trust those numbers and how likely the business is to execute on projections. Without an operational maturity assessment:

- **Overreliance on reported numbers**: A business with spreadsheet-based accounting and no internal audit is given the same data confidence as one with an ERP and Big4 audit — but error rates differ by orders of magnitude
- **Missing risk discount**: Valuation models (L3) apply discount rates based on market risk (beta, WACC) but ignore operational risk from immature finance functions, leading to overvaluation
- **Blind due diligence**: The evaluator assesses the business without understanding whether it has the operational infrastructure to deliver on the investment thesis

Situations covered:
- Any PRISM evaluation at L2 stage — operational maturity scoring provides a non-financial risk overlay
- Pre-investment due diligence (Saraimanic: evaluated as DEVELOPING)
- Portfolio monitoring (track maturity improvement post-investment)
- NOT suitable for: Very early-stage startups (pre-revenue) where formal finance functions are not expected

---

## 2. Procedura

### Pas 1: Assess Close Speed (0-2 pts)
Ask or infer monthly close speed from filing dates:
- Public companies: 10-K filing within 60 days of fiscal year end (accelerated filer) = mature
- Private companies: estimate from data request response time during due diligence
- **T+3 or faster** = 2 pts (strong controls, automated reconciliation)
- **T+4-7** = 1 pt (adequate, manual but functional)
- **T+8+** = 0 pts (slow close indicates reconciliation backlog)

### Pas 2: Evaluate Audit Opinion History (0-3 pts, possible negative)
Review last 3 years of audit opinions:
- **Unqualified opinion** = 3 pts (clean bill of health)
- **Qualified opinion** = 1 pt (specific issues but overall reliable)
- **Adverse opinion or Disclaimer** = 0 pts (immediate red flag — escalate to L5 risk assessment)
- **Restatements in any of last 3 years** = -2 pts (retroactive correction indicates prior data was unreliable)

### Pas 3: Check Internal Controls Disclosure (0-2 pts, possible negative)
- **Public companies**: Check SOX 404 disclosure. Any material weakness = -2 pts per weakness. No material weaknesses = 2 pts.
- **Private companies**: Ask if they have an internal audit function. Yes with documented procedures = 2 pts. Yes informal = 1 pt. No = 0 pts.

### Pas 4: Assess Financial System Maturity (0-2 pts)
- **ERP system** (NetSuite, SAP, Oracle, Microsoft Dynamics) = 2 pts (integrated, audit-trail capable)
- **Accounting software** (QuickBooks, Xero, FreshBooks) = 1 pt (functional but limited for complex entities)
- **Spreadsheet-based** (Excel/Sheets as primary accounting system) = 0 pts (high error risk, no audit trail)

### Pas 5: Evaluate Finance Team Composition (0-2 pts)
- **CFO or VP Finance** with Big4 or public company background = 2 pts (professional-grade financial leadership)
- **Controller** with relevant industry experience = 1 pt (competent but may lack strategic finance perspective)
- **Bookkeeper only** = 0 pts (transaction processing but no analytical or strategic finance capability)

### Pas 6: Check Budget/Forecast Process (0-2 pts)
- **Formal annual budget** + monthly actuals vs budget analysis with variance explanations = 2 pts
- **Informal budgeting** (targets exist but no structured tracking) = 1 pt
- **No budget process** = 0 pts (management has no financial planning discipline)

### Pas 7: Assess Compliance Track Record (0-2 pts)
- **Clean**: No tax issues, no regulatory fines, no litigation related to financial reporting = 2 pts
- **Minor issues**: Past issues fully resolved, no active disputes = 1 pt
- **Active disputes**: Ongoing tax disputes, regulatory investigations, or financial reporting litigation = 0 pts

### Pas 8: Score and Classify
Sum all points from Steps 1-7 (range: -6 to 15, practical range: 0-14):
- **12-14 pts**: MATURE — Finance function is an asset. Data is trustworthy. No valuation discount needed.
- **8-11 pts**: ADEQUATE — Functional but risks exist. Minor data quality caveats. Apply 5-10% risk discount in L3 valuation.
- **4-7 pts**: DEVELOPING — Significant improvement needed pre-investment. Apply 15-25% risk discount OR require remediation plan as investment condition.
- **0-3 pts**: IMMATURE — High operational risk. Apply 30%+ risk discount OR structure as milestone-based investment with finance function milestones.

### Pas 9: Factor Score into Investment Decision
At L5, the operational maturity score directly impacts:
- **Valuation**: Apply the risk discount from Step 8 to all L3 valuation methods
- **Deal structure**: For DEVELOPING/IMMATURE, recommend milestone-based investment with specific finance function improvement milestones (e.g., hire controller by month 3, implement ERP by month 6)
- **Post-investment monitoring**: Track score quarterly; improvement = positive signal, stagnation = risk escalation

---

## 3. Cortex Logging

```json
{
  "text": "PRISM Operational Maturity Score | {company_name} | Score: {score}/14 | Classification: {MATURE|ADEQUATE|DEVELOPING|IMMATURE} | Valuation discount: {discount_pct}% | Key gaps: {summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-PRISM-O01",
    "rule_id": "TRAIN-PRISM-001",
    "domain": "finance",
    "pipeline_level": "L2",
    "company": "{company_name}",
    "maturity_score": "{score}",
    "maturity_classification": "{MATURE|ADEQUATE|DEVELOPING|IMMATURE}",
    "valuation_discount_pct": "{discount}",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["business-evaluation", "prism-pipeline", "operational-maturity", "risk-assessment"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- PRISM pipeline L2 execution (after F-009 Non-Financial KPI Linking)
- Pre-investment due diligence checklist

### WHEN
- After L1 data collection and initial L2 analysis
- Before L5 investment decision (maturity score feeds into valuation discount)

### HOW (violation detection)
- L5 investment recommendation issued without operational maturity score -> violation (risk discount not applied)
- Score = DEVELOPING or IMMATURE but no remediation conditions in investment recommendation -> violation
- Runner: PRISM pipeline check verifies F-PRISM-O01 execution and score propagation to L5

### CONNECT
- TRAIN-PRISM-001 -> PRISM master pipeline, L2 procedure
- F-022 (Executive Compensation Analysis) -> prerequisite; finance team assessment (Step 5) builds on F-022 findings
- F-094 (VC/Private Investment Evaluation, L5) -> maturity score feeds risk adjustment
- F-090 (Comprehensive Risk Register, L5) -> immature finance function is a risk register entry
- `procedure-health.json` -> add entry for F-PRISM-O01

### VERIFY
- [ ] All 9 steps executed? (close speed through investment decision factor)
- [ ] Score calculated (0-14) and classification assigned?
- [ ] If DEVELOPING/IMMATURE: remediation recommendations documented?
- [ ] Valuation discount recommendation specified for L5?
- [ ] VK emitted?

**VK**:
1. `[PROC] F-PRISM-O01 Operational Maturity | {company} | {score}/14 | {MATURE|ADEQUATE|DEVELOPING|IMMATURE} | discount: {pct}% | complete`
2. `[CORTEX] "Operational Maturity — {company}" | FORGE | rule: TRAIN-PRISM-001 | v1.0`

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| PRISM.md | Master pipeline (this is L2 procedure) | `~/.nexus/procedures/PRISM.md` |
| F-022 Executive Compensation Analysis | Prerequisite — finance team context | `tech/business-eval-L1-L2.md` |
| F-094 VC/Private Investment Evaluation (L5) | Downstream — maturity score impacts risk | `tech/business-eval-L4-L5.md` |
| F-090 Comprehensive Risk Register (L5) | Downstream — immature = risk entry | `tech/business-eval-L4-L5.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Maturity score | Finance function quality (0-14) | >= 8 (ADEQUATE) for investment consideration |
| Score completeness | % of 7 dimensions assessed (vs N/A) | 100% assessed |
| Discount propagation | Whether L5 valuation reflects maturity discount | 100% propagated |

---

## 7. Business Applications

| Business | Last Score | Classification | Key Gaps |
|----------|-----------|---------------|----------|
| Saraimanic | TBD (evaluated as DEVELOPING per prior assessment) | DEVELOPING | Finance team, budget process, system maturity |
