---
type: procedure
created: 2026-03-17
status: active
slug: finance-plugin-prism-handoff
tags: [procedure, nexus]
---

# Finance Plugin → PRISM Handoff Protocol — Procedura Standard de Operare

**Status**: ACTIVE
**Creat**: 2026-03-03
**Versiune**: 1.0
**Regula asociata**: PRISM-H-001
**Scope**: Standardized data handoff from Finance Plugin operational outputs to PRISM L1 evaluation pipeline. Eliminates manual re-processing, ensures data quality, and pre-computes variances to accelerate L2 analysis.
**ID**: F-PRISM-H01
**Complexitate**: INTERMEDIATE
**Locatie PRISM**: Pre-L1 (optional accelerator — runs before L1 when Finance Plugin data is available)

---

## 1. Problema

When financial data has been prepared using the Finance Plugin (`~/.claude/plugins/finance/`), transitioning that data to the PRISM evaluation pipeline introduces three categories of waste:

1. **Manual re-processing**: PRISM L1 procedures (F-010, F-011, F-002/3/4) extract and verify financial statements from scratch, even though the Finance Plugin has already produced reconciled statements via `/reconciliation` and structured P&L via `/income-statement`. This duplicates 60-80% of L1 data ingestion work.

2. **Data quality gaps**: Without a standardized handoff, there is no guarantee that the data entering PRISM was reconciled. The evaluator may import raw exports that bypass the Finance Plugin's reconciliation workflows, introducing undetected errors into L1 outputs that cascade through L2-L5.

3. **Redundant variance computation**: PRISM L2 (F-PRISM-V01 Variance Decomposition, F-015 Trend Analysis) recomputes YoY and budget variances from raw data. If `/variance-analysis` has already been run in the Finance Plugin, this work is entirely redundant — the decomposition, driver identification, and narratives already exist.

**Situations covered**:
- Any PRISM evaluation where the target company's financials were prepared or analyzed using the Finance Plugin
- Pre-investment due diligence where Finance Plugin was used for initial data gathering
- Recurring evaluations (quarterly re-evaluation of a portfolio company) where Finance Plugin data is updated each period
- **NOT suitable for**: Companies whose financials were NOT processed through the Finance Plugin (use standard PRISM L1 entry instead)

---

## 2. Procedura

### Pas 1: Verify Finance Plugin Data Availability

Confirm that the Finance Plugin has been used for the target company and period. Check for existence of:
- `/reconciliation` outputs for key accounts (Cash, AR, AP, Fixed Assets)
- `/income-statement` output for the period
- `/variance-analysis` output for at least one comparison (YoY or vs budget)

If none exist, STOP — this procedure does not apply. Use standard PRISM L1 entry.

### Pas 2: Run Reconciliation Status Check

Execute or retrieve the most recent `/reconciliation` results for all key balance sheet accounts:
- Cash / Bank accounts
- Accounts Receivable
- Accounts Payable
- Fixed Assets
- Intercompany (if consolidated entity)

For each account, record: reconciled status (Yes/No), count of open reconciling items, count of items aged >30 days. This feeds the Data Quality Gate.

### Pas 3: Apply Data Quality Gate

Evaluate reconciliation results against gate criteria:
- **PASS**: All key accounts reconciled, no items aged >90 days, total unreconciled difference < 1% of Total Assets
- **WARNING**: Some accounts not reconciled OR aged items present, but total unreconciled difference < 3% of Total Assets
- **FAIL**: Cash/Bank not reconciled, OR total unreconciled difference > 3% of Total Assets, OR >3 accounts with items aged >90 days

If FAIL: Do not proceed with handoff. Remediate by running `/reconciliation` for failed accounts first. Document the failure and required remediation steps.

### Pas 4: Extract Financial Statement Package

Pull from Finance Plugin outputs:
- Trial balance at account level (from ERP/data warehouse via Plugin connectors, or from user-provided data)
- Income Statement with current and prior period (from `/income-statement` output)
- Balance Sheet classified current/non-current (from Plugin's financial-statements skill)
- Cash Flow Statement indirect method (from Plugin's financial-statements skill)

Format all statements in the standard PRISM Input Package structure (see `/prism-handoff` command for template).

### Pas 5: Compute or Retrieve Variance Context

If `/variance-analysis` has been run for this entity and period:
- Retrieve the variance results: YoY variances, budget variances, waterfall decomposition, driver narratives
- Verify materiality thresholds match PRISM standards (Revenue >5%, Gross Margin >200bp, OpEx >10%, BS items >3% of Total Assets, CF >10%, Ratios >20% change)
- If Plugin thresholds differ from PRISM thresholds, recompute materiality flags using PRISM thresholds

If no prior variance analysis exists:
- Compute YoY variances for all major P&L line items (Revenue, COGS, Gross Profit, OpEx by category, Operating Income, Net Income)
- Compute budget variances if budget data is available
- Apply PRISM materiality thresholds
- For each material variance, provide a 1-2 sentence driver narrative and classify as Structural / Cyclical / One-time

### Pas 6: Assess Close Status

Document the accounting close completeness:
- Subledger close status (complete/open)
- Journal entries posted (complete/open, count remaining)
- Accruals booked (complete/open, list any pending)
- Intercompany eliminations (complete/N/A)
- Account reconciliations (complete/open, count remaining)
- Management review (complete/open)
- Audit adjustments (none/pending, list any)

Classify close quality: Hard close (all final) / Soft close (mostly final) / Preliminary (material adjustments possible).

### Pas 7: Calculate Data Confidence Score

Compute the 0-100 composite score using five weighted components:

| Component | Weight | Scoring |
|-----------|--------|---------|
| Reconciliation pass rate | 30% | 100% reconciled = 30pts; 80-99% = 20pts; 60-79% = 10pts; <60% = 0pts |
| Open items count | 20% | 0 items = 20pts; 1-5 = 15pts; 6-15 = 10pts; 16-30 = 5pts; >30 = 0pts |
| Aged items (>30 days) | 15% | 0 items = 15pts; 1-3 = 10pts; 4-10 = 5pts; >10 = 0pts |
| Close completeness | 20% | Hard close = 20pts; Soft close = 12pts; Preliminary = 5pts |
| Audit status | 15% | Audited = 15pts; Reviewed = 10pts; No adj pending = 8pts; Adj pending = 3pts |

Score interpretation:
- 80-100: HIGH CONFIDENCE — PRISM L2 uses standard thresholds
- 60-79: MODERATE CONFIDENCE — PRISM L2 widens thresholds by 50%
- 40-59: LOW CONFIDENCE — additional F-PRISM-R01 validation required before L3
- 0-39: INSUFFICIENT — do not proceed to PRISM L1 without remediation

**Minimum score for PRISM L1 ingestion: 60.**

### Pas 8: Assemble PRISM Input Package

Combine all outputs into the standardized package:
1. Header: company, period, generation date, data confidence score, Finance Plugin version
2. Section 1: Data Quality Gate (from Pas 3)
3. Section 2: Financial Statements (from Pas 4)
4. Section 3: Variance Context (from Pas 5)
5. Section 4: Close Status (from Pas 6)
6. Section 5: Data Confidence Score breakdown (from Pas 7)
7. Section 6: PRISM Procedure Mapping — table showing which L1 procedures can consume each package section

### Pas 9: Pass to PRISM L1

Deliver the assembled package. If score >= 60:
- PRISM L1 procedures F-010, F-011, F-002, F-003, F-004 can reference the package directly instead of re-extracting data
- F-PRISM-R01 (Reconciliation Validation) can skip Steps 3-7 if Data Quality Gate = PASS (reconciliation already verified)
- F-PRISM-V01 (Variance Decomposition) can use pre-computed variances from Section 3 instead of full recomputation

Log the handoff to Cortex (see Section 3 below).

---

## 3. Cortex Logging

```json
{
  "text": "Finance Plugin → PRISM Handoff | {company_name} | Period: {period} | Confidence: {score}/100 ({classification}) | Gate: {PASS|WARNING|FAIL} | Variances pre-computed: {yes|no}",
  "collection": "procedures",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-PRISM-H01",
    "rule_id": "PRISM-H-001",
    "domain": "finance",
    "pipeline_level": "pre-L1",
    "company": "{company_name}",
    "period": "{period}",
    "data_confidence_score": "{score}",
    "data_quality_gate": "{PASS|WARNING|FAIL}",
    "has_enforcement_loop": true,
    "forge_version": "1.3",
    "tags": ["prism-pipeline", "finance-plugin", "data-handoff", "data-quality"]
  }
}
```

---

## 4. Enforcement Loop (META-H-002)

### WHERE
- Before PRISM L1 execution, when the target company's financials were prepared using the Finance Plugin
- During recurring evaluations where Finance Plugin data is updated each period
- Any context where Finance Plugin outputs exist and PRISM evaluation is requested

### WHEN
- A PRISM evaluation is initiated AND Finance Plugin data exists for the same company/period
- The Finance Plugin close workflow has been completed (at minimum: reconciliation + income statement generated)
- Before any PRISM L1 procedure begins processing financial data

### HOW (violation detection)
- PRISM L1 initiated with Finance Plugin data available but `/prism-handoff` not run -> warning (missed acceleration opportunity, potential data quality gap)
- Data Confidence Score < 60 but PRISM L1 proceeded without remediation -> violation (unreliable data entering pipeline)
- Handoff package generated but Data Quality Gate = FAIL and L1 proceeded anyway -> violation (unreconciled data entering pipeline)
- Runner: Check Cortex for F-PRISM-H01 execution record when PRISM pipeline starts and Finance Plugin usage is detected for the same entity

### CONNECT
- **PRISM.md**: Master pipeline — this procedure is an optional pre-L1 accelerator (`~/.nexus/procedures/PRISM.md`)
- **`/prism-handoff` command**: User-facing command that executes this procedure (`~/.claude/plugins/finance/commands/prism-handoff.md`)
- **Finance Plugin skills**: `reconciliation`, `variance-analysis`, `financial-statements`, `close-management` — source data for the handoff package
- **F-PRISM-R01 (Reconciliation Validation)**: When handoff Gate = PASS, L2 can skip redundant reconciliation checks
- **F-PRISM-V01 (Variance Decomposition)**: Pre-computed variances from handoff accelerate L2 variance analysis
- **`/reconciliation` command**: Prerequisite — must have been run for Data Quality Gate
- **`/variance-analysis` command**: Optional prerequisite — if run, variances are carried over; if not, they are computed during handoff

### VERIFY
- [ ] Finance Plugin data availability confirmed (Pas 1)?
- [ ] Data Quality Gate evaluated (PASS/WARNING/FAIL) and documented (Pas 3)?
- [ ] Data Confidence Score >= 60 before PRISM L1 ingestion (Pas 7)?
- [ ] PRISM Input Package assembled with all 6 sections (Pas 8)?
- [ ] Handoff logged to Cortex (Pas 9)?

**VK**:
✅ `[PROC] F-PRISM-H01 Finance Plugin → PRISM Handoff | {company} | Confidence: {score}/100 | Gate: {status} | complete`
✅ `[CORTEX] "Finance Plugin PRISM Handoff — {company}" | FORGE | rule: PRISM-H-001 | v1.0`

### Forbidden Actions

- **NEVER** proceed with PRISM L1 ingestion when Data Confidence Score < 60 without explicit remediation and re-scoring
- **NEVER** skip the Data Quality Gate — even if the user claims "the data is fine", the gate must be evaluated and documented
- **NEVER** use raw (unreconciled) Finance Plugin exports as the handoff package — data must pass through `/reconciliation` first or be flagged as WARNING/FAIL

---

## 5. Dependente

| Componenta | Rol | Path/Endpoint |
|-----------|-----|---------------|
| Finance Plugin | Source of reconciled financial data and variance analyses | `~/.claude/plugins/finance/` |
| `/reconciliation` command | Prerequisite — provides reconciliation status for Data Quality Gate | `~/.claude/plugins/finance/commands/reconciliation.md` |
| `/variance-analysis` command | Optional — provides pre-computed variances for Section 3 | `~/.claude/plugins/finance/commands/variance-analysis.md` |
| `/income-statement` command | Provides structured IS for Section 2 | `~/.claude/plugins/finance/commands/income-statement.md` |
| PRISM.md | Target pipeline that consumes the handoff package | `~/.nexus/procedures/PRISM.md` |
| F-PRISM-R01 | L2 reconciliation validation — can be accelerated by handoff gate | `~/.nexus/procedures/tech/prism-reconciliation-validation.md` |
| F-PRISM-V01 | L2 variance decomposition — can consume pre-computed variances | `~/.nexus/procedures/tech/prism-variance-decomposition.md` |
| Cortex | Logging handoff execution and data quality metadata | MCP cortex_store |

---

## 6. Metrics

| Metrica | Ce masoara | Target |
|---------|-----------|--------|
| Data Confidence Score | Composite reliability of handoff package (0-100) | >= 60 for PRISM ingestion, >= 80 preferred |
| Reconciliation pass rate | % of key accounts fully reconciled at handoff time | >= 80% |
| L1 time saved | Hours saved by PRISM L1 when using handoff vs raw data entry | > 50% reduction |
| Variance coverage | % of material variances pre-computed in handoff package | >= 90% of material items |
| Handoff adoption rate | % of PRISM evaluations using handoff when Plugin data is available | 100% (once established) |
