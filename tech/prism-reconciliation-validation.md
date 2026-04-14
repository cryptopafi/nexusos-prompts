---
type: procedure
created: 2026-03-17
status: active
slug: prism-reconciliation-validation
tags: [procedure, nexus]
---

# Reconciliation Validation — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regula asociata**: TRAIN-PRISM-001
**Scope**: Validates whether financial numbers presented by a business are reconciled — a direct indicator of fraud risk and financial data quality. Does not perform actual reconciliation (no GL access), but checks external signals of reconciliation quality.
**ID**: F-PRISM-R01
**Complexitate**: INTERMEDIATE
**Locatie PRISM**: L2 (after Earnings Management Detection, before Credit Analysis)

---

## 1. Problema

Financial statements can appear clean on the surface while containing unreconciled balances that mask errors, omissions, or deliberate manipulation. Without reconciliation validation:

- **Data integrity failure**: L1 outputs (ratios, trends) are built on numbers that don't internally agree, producing misleading analysis
- **Fraud blindness**: Unreconciled intercompany balances, AR/AP mismatches, and inventory breaks are classic concealment vectors
- **False confidence**: The evaluator proceeds to valuation (L3) trusting data that hasn't been verified for internal consistency

Situations covered:
- Any PRISM evaluation where L1 data has been collected — reconciliation validation is a prerequisite for trusting L1 outputs
- Pre-investment due diligence (data quality gate)
- Fraud screening supplement to F-006 (Earnings Management Detection) and F-008 (Benford's Law)
- NOT suitable for: Performing actual general ledger reconciliation (requires direct system access)

---

## 2. Procedura

### Pas 1: Request Reconciliation Status Documentation
Request from the target: last 3 months close reports, audit opinions (if available), bank reconciliation sign-off dates. Document what was provided vs. what was requested. Missing documentation is itself a signal.

### Pas 2: Check Close Speed as Reconciliation Proxy
Determine monthly close speed. T+3 or faster = strong controls (reconciliation is automated or well-staffed). T+5-7 = adequate (manual but functional). T+10+ = red flag (reconciliation likely incomplete or deferred). Source: ask management or infer from filing dates for public companies.

### Pas 3: Analyze Intercompany Elimination Completeness
For consolidated statements: intercompany balances should net to zero after elimination. Flag any disclosed intercompany amounts that remain uneliminated. Check notes to financial statements for intercompany transaction disclosures. Non-zero residuals after elimination = reconciliation failure.

### Pas 4: Cross-Check AR/AP Aging Reports Against Balance Sheet
AR on the balance sheet should match the total from the AR aging report. AP on the balance sheet should match the AP aging total. Discrepancy > 1% of the balance = potential reconciliation issue. Document the exact discrepancy amount and percentage.

### Pas 5: Verify Depreciation and Amortization Consistency
D&A on the Cash Flow Statement (operating section, add-back) should equal D&A disclosed in the notes or on the income statement. Discrepancy indicates either: (a) different D&A definitions being used inconsistently, or (b) potential manipulation of one or both figures.

### Pas 6: Check Inventory Movement Reconciliation
Verify the identity: Opening Inventory + Purchases - COGS = Closing Inventory. This should hold within rounding (< 0.5% of opening balance). Significant breaks may indicate: inventory write-downs not properly classified, consignment inventory issues, or inventory manipulation (channel stuffing).

### Pas 7: Validate Equity Roll-Forward
Verify across 3 periods: Opening Equity + Net Income +/- Other Comprehensive Income +/- Stock Issuance/Buybacks +/- Dividends = Closing Equity. The Statement of Changes in Equity should reconcile perfectly to the balance sheet equity line. Any unexplained delta = reconciliation failure.

### Pas 8: Score Reconciliation Quality
Count total red flags from Steps 2-7. Scoring:
- **0-2 red flags**: PASS — data is sufficiently reconciled for PRISM analysis to proceed with confidence
- **3-4 red flags**: WATCH — data quality is suspect; flag specific issues in L2 report, apply wider margins in L3 valuation
- **5+ red flags**: FAIL — escalate immediately to L5 fraud assessment; do NOT proceed to L3 valuation without resolution

---

## 3. Cortex Logging

```json
{
  "text": "PRISM Reconciliation Validation | {company_name} | Score: {PASS|WATCH|FAIL} | Red flags: {count}/7 | Key issues: {summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-PRISM-R01",
    "rule_id": "TRAIN-PRISM-001",
    "domain": "finance",
    "pipeline_level": "L2",
    "company": "{company_name}",
    "reconciliation_score": "{PASS|WATCH|FAIL}",
    "red_flag_count": "{count}",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["business-evaluation", "prism-pipeline", "reconciliation", "data-quality"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- PRISM pipeline L2 execution (after F-006/F-008, before F-092)
- Any live business evaluation where L1 data has been collected

### WHEN
- After L1 completion, before proceeding to L3 valuation
- When data quality concerns are raised at any pipeline stage

### HOW (violation detection)
- L3 valuation initiated without reconciliation validation score documented -> violation (building valuation on unverified data)
- Reconciliation score = FAIL but pipeline continued to L3 without escalation -> violation
- Runner: PRISM pipeline completion check verifies F-PRISM-R01 execution before L3 sign-off

### CONNECT
- TRAIN-PRISM-001 -> PRISM master pipeline, this procedure is a mandatory L2 gate
- F-006 (Earnings Management Detection) -> reconciliation failures feed into earnings quality assessment
- F-008 (Benford's Law) -> combined with Benford's provides dual fraud screening
- F-092 (Credit Analysis) -> reconciliation quality affects credit assessment confidence
- `procedure-health.json` -> add entry for F-PRISM-R01

### VERIFY
- [ ] All 8 steps executed? (documentation request through scoring)
- [ ] Reconciliation score assigned (PASS/WATCH/FAIL)?
- [ ] If FAIL: escalation to L5 documented?
- [ ] If WATCH: specific issues flagged in L2 report?
- [ ] VK emitted?

**VK**:
1. `[PROC] F-PRISM-R01 Reconciliation Validation | {company} | {PASS|WATCH|FAIL} | {count}/7 flags | complete`
2. `[CORTEX] "Reconciliation Validation — {company}" | FORGE | rule: TRAIN-PRISM-001 | v1.0`

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| PRISM.md | Master pipeline (this is L2 procedure) | `~/.nexus/procedures/PRISM.md` |
| L1 Outputs (F-010, F-011, F-002/3/4) | Source data for reconciliation checks | `tech/business-eval-L1-L2.md` |
| F-006 Earnings Management Detection | Feeds into combined fraud assessment | `tech/business-eval-L1-L2.md` |
| F-008 Benford's Law | Complementary fraud screen | `tech/business-eval-L1-L2.md` |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Red flag count | Number of reconciliation failures detected (0-7) | < 3 for PASS |
| Close speed | Monthly close time in days | T+5 or faster |
| Data request fulfillment | % of requested reconciliation docs provided | > 80% |
