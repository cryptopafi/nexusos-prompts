---
type: procedure
created: 2026-03-22
status: active
slug: prism
tags: [procedure, nexus]
---

# PRISM — Business Evaluation Pipeline (Master Procedure)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.2
**Updated**: 2026-03-03 — Added P1-P6 improvements: Reconciliation Validation, Variance Decomposition, Operational Maturity Score + GAAP compliance, Audit Trail, Materiality Framework
**Rule**: TRAIN-PRISM-001
**Scope**: End-to-end structured pipeline for evaluating any business, from spreadsheet tool proficiency through investment decision. 6 levels, 65 FORGE'd procedures.

---

## S1 Problem

Evaluating a business requires dozens of interconnected skills: reading financial statements, computing ratios, building valuation models, stress-testing assumptions, and synthesizing everything into an investment recommendation. Without a structured pipeline that enforces prerequisite sequencing, two categories of failure occur:

1. **Skills gap failure**: The evaluator attempts a DCF valuation without knowing how to build a 3-statement model, or builds a model without mastering the spreadsheet functions it depends on (VLOOKUP, Pivot Tables, TVM functions, Goal Seek). The output looks plausible but contains structural errors that cascade into wrong valuations.

2. **Sequencing failure**: The evaluator jumps to valuation (L3) without completing foundational analysis (L1-L2), skips sensitivity analysis (L4), or issues an investment recommendation (L5) without modeling the business. Each skipped level introduces unquantified risk into the decision.

PRISM solves both by defining a mandatory 6-level progression: Tools (spreadsheet mastery) -> L1 (read data) -> L2 (analyze) -> L3 (value) -> L4 (model) -> L5 (decide). Each level has specific procedures with defined prerequisites, outputs, and enforcement rules.

**Situations covered**:
- Training an AI system to perform complete business evaluations from scratch
- Ensuring no evaluation step is skipped when assessing a real company
- Onboarding new analysts to a standardized evaluation methodology
- Auditing whether a completed evaluation followed proper procedure sequencing
- **NOT suitable for**: Quick screening passes where only 2-3 ratios are needed (use individual procedures directly), or non-financial evaluations (product-market fit, team assessment) which require separate frameworks

---

## S2 Procedure

### Pipeline Overview

PRISM is a 6-level pipeline. Each level builds on the outputs of the previous level. The Tools level is a prerequisite for all subsequent levels.

```
TOOLS (11 procedures) — Master the spreadsheet instruments
  |
  v
L1: READ THE NUMBERS (12 procedures) — Extract and verify financial data
  |
  v
L2: ANALYZE THE BUSINESS (16 procedures) — Interpret ratios, detect anomalies
  |
  v
L3: VALUATION (12 procedures) — Calculate intrinsic and relative value
  |
  v
L4: MODEL THE BUSINESS (8 procedures) — Build forward-looking projections
  |
  v
L5: INVESTMENT DECISION (6 procedures) — Synthesize into actionable recommendation
```

### Optional: Finance Plugin Input Package

If the target company's financials were prepared using the Finance Plugin (`~/.claude/plugins/finance/`), run `/prism-handoff <company> <period>` before starting L1. This generates a pre-validated data package with:
- Reconciled financial statements (data quality gate passed)
- Pre-computed variances (accelerates L2)
- Data Confidence Score (calibrates analytical depth)

Mandatory handoff metadata for this package:

```json
{
  "handoff_prompt": "...",
  "why": "...",
  "confidence": 0.0,
  "confidence_flags": []
}
```

When this package is available, L1 data ingestion procedures (F-010, F-011, F-002, F-003, F-004) can reference it directly instead of re-processing raw files.

### Level Descriptions

#### Tools Level — Excel & Google Sheets Mastery (11 Procedures)

The Tools level ensures the evaluator can USE the spreadsheet instruments required by L1-L5. Every procedure has dual Excel + Google Sheets implementation paths. This level has no business evaluation content — it is pure skill-building for the tools that downstream procedures depend on.

**Key skill areas**: Lookup functions (VLOOKUP, XLOOKUP, INDEX-MATCH), Pivot Tables, TVM financial functions (PV, FV, NPV, IRR, PMT), SUMIFS/database aggregation, Dashboard construction, Macro automation, Goal Seek/optimization, Financial model sheet architecture.

**Files**:
- `tech/excel-sheets-mastery-batch1.md` — F-066, F-068, F-070, F-080 (4 procedures)
- `tech/excel-sheets-mastery-batch2.md` — F-081, F-082, F-083, F-084 (4 procedures)
- `tech/excel-sheets-mastery-batch3.md` — F-085, F-086, F-089 (3 procedures)

#### L1 — Read the Numbers (12 Procedures)

Extract, verify, and structure raw financial data from statements and filings. Outputs: verified balance sheet, cash flow analysis, ratio calculations, filing interpretation.

**File**: `tech/business-eval-L1-L2.md` (procedures 1-12)

#### L2 — Analyze the Business (16 Procedures)

Interpret financial data through DuPont analysis, trend analysis, variance decomposition (price/volume/mix drivers), peer comparison, earnings quality checks, fraud screening, reconciliation validation (data integrity check), credit assessment, and operational maturity scoring (finance function quality). Outputs: ratio dashboards, peer benchmarks, anomaly flags, credit ratings, variance narratives, reconciliation scores, maturity classifications.

**Files**: `tech/business-eval-L1-L2.md` (13 of 25 in this file are L2) + `tech/prism-variance-decomposition.md` (F-PRISM-V01) + `tech/prism-reconciliation-validation.md` (F-PRISM-R01) + `tech/prism-operational-maturity-score.md` (F-PRISM-O01)

#### L3 — Valuation (12 Procedures)

Calculate intrinsic value using NPV, IRR, DCF, EVA, residual income, and relative value using trading comps, deal comps, P/E, and P/Revenue. Outputs: valuation range, football field chart, multi-method consensus.

**File**: `tech/business-eval-L3.md`

#### L4 — Model the Business (8 Procedures)

Build forward-looking financial models: revenue forecasts, 3-statement models, sensitivity analysis, multi-segment models, capital budgeting. Outputs: projection models, sensitivity matrices, tornado charts.

**File**: `tech/business-eval-L4-L5.md` (procedures 1-8)

#### L5 — Investment Decision (6 Procedures)

Synthesize all analysis into an investment recommendation: research process, VC evaluation, M&A analysis, IPO analysis, risk registers, scam detection. Outputs: investment memo, risk assessment, go/no-go recommendation.

**File**: `tech/business-eval-L4-L5.md` (procedures 9-14)

### Materiality Framework (Cross-Cutting)

All PRISM procedures apply a consistent materiality definition to determine what requires investigation:

| Benchmark | Threshold for Material Variance |
|-----------|--------------------------------|
| Revenue | > 5% variance from prior period or peer median |
| Gross Margin | > 200 basis points change |
| Operating Expenses | > 10% variance on any single category |
| Balance Sheet items | > 3% of Total Assets |
| Cash Flow | > 10% variance on Operating Cash Flow |
| Ratios | > 20% change in any PRISM-tracked ratio |

When a variance exceeds the materiality threshold, it must be: (1) decomposed to identify the driver (use F-PRISM-V01), (2) classified as structural/cyclical/one-time, (3) documented in the PRISM report under the relevant section.

### Execution Flow

**For training**: Execute Tools level first (all 11 procedures in sequence). Then proceed L1 -> L2 -> L3 -> L4 -> L5. Within each level, follow the order in the Table of Contents.

**For live evaluation**: Tools level is assumed complete (prerequisite skills already acquired). Execute L1 -> L2 -> L3 -> L4 -> L5 for the target company. Individual procedures may be skipped only if their output is not needed for the specific evaluation scope (e.g., skip F-049 ESG if ESG analysis is out of scope), but never skip an entire level.

### Report Generation (Post-Pipeline Output)

After completing L1-L5 analysis, the pipeline produces an investor-facing report. Two output formats are supported:

#### Option A: Google Sheets (Preferred)

Direct creation via Google Workspace MCP tools + Sheets API. Preferred because:
- Live collaboration (share link, comment, iterate in real-time)
- No file download/upload friction
- GOOGLEFINANCE formulas can pull live market data into the report
- Accessible from any device

**Tools used**:
- `mcp__google-workspace-mcp__create_spreadsheet` — create with named tabs
- `mcp__google-workspace-mcp__modify_sheet_values` — write data (USER_ENTERED for formulas)
- `mcp__google-workspace-mcp__format_sheet_range` — colors, fonts, number formats, alignment
- **Cell merging**: Not supported by MCP tools. Use Sheets API `batchUpdate` with `mergeCells` requests directly. OAuth token stored at `~/.google_workspace_mcp/credentials/{email}.json`. Refresh via `refresh_token` + `token_uri`, then POST to `https://sheets.googleapis.com/v4/spreadsheets/{id}:batchUpdate`.

**Auth**: Google OAuth via `richie.aiceo@gmail.com` (or user's workspace email). Credentials cached by MCP server after first auth.

#### Option B: Excel (.xlsx)

Generated via Python `openpyxl` library. Useful when:
- Recipient doesn't use Google Workspace
- Offline access needed
- Complex VBA macros required post-generation

**Tools used**: Python script with `openpyxl` (write data, format cells, merge cells natively).

#### Standard Report Structure (8 Tabs)

| Tab | Content | Data Source |
|-----|---------|-------------|
| 1. Summary | Key metrics, verdict, valuation range, conditions, report navigation | All levels |
| 2. Revenue Breakdown | Business lines, segment analysis, per-event economics, 2026 indicators | L1 (F-002, F-004) |
| 3. Cost Structure | Direct costs by category, indirect overhead, salary breakdown, P&L summary | L1 (F-002, F-033) |
| 4. What If Scenarios | Contribution scenarios, breakeven, sensitivity tables, tornado ranking, path-to-profit matrix | L4 (F-069, F-067) |
| 5. Valuation | DCF (3 scenarios), comparable multiples, composite range, assumptions, milestones | L3 (F-039, F-041, F-043) |
| 6. Recommendations | Strategic options ranked, GO conditions with targets vs gaps, industry benchmarks | L5 (F-062, F-094) |
| 7. Action Items | Prioritized actions across 4 timeframes (0-3mo, 3-6, 6-12, 12+) with owners and KPIs | L5 (F-062) |
| 8. Risks | Risk register (severity-coded), data gaps, credit assessment (5C framework) | L2 (F-092), L5 (F-090) |

**Formatting standard**: Formal investor-deck tone. Green (#2e7d32) for profits, red (#d32f2f) for losses, amber (#e65100) for conditional. Currency with thousand separators (#,##0). Percentages at 0.0%. Section headers with navy (#1a237e) background. Merged cells for all explanation text spanning full tab width. Alternating row colors (#f5f5f5) in data tables.

**First execution**: Saraimanic (S.C. VRTW Artists SRL) — 2026-03-03, Google Sheets, 8 tabs, 67 merged text cells. URL: `https://docs.google.com/spreadsheets/d/1fCdFpHJYTzqe6LZ0pKxk8ASsum1BYrDJ3WWpEFmc-vg/edit`

---

### Google Sheets Integration (Tools Level)

All Tools-level procedures support dual Excel + Google Sheets implementation paths. Key Sheets-specific patterns used across the pipeline:

| Pattern | Excel Equivalent | Google Sheets Implementation |
|---------|-----------------|------------------------------|
| Live market data | Manual data entry or Bloomberg add-in | `GOOGLEFINANCE("AAPL", "price")` — real-time stock data |
| Multi-criteria aggregation | SUMIFS | `QUERY(range, "SELECT ... WHERE ... GROUP BY ...")` — SQL-like, faster on large datasets |
| Dynamic filtering | Advanced Filter or helper columns | `FILTER(range, criteria)` — array-native, no helper columns |
| Array broadcasting | Ctrl+Shift+Enter CSE formulas | `ARRAYFORMULA()` — native array support, no special entry |
| VBA macros | VBA in .xlsm files | Apps Script (JavaScript-based, cloud-hosted, no file format restriction) |
| Inline sparklines | Insert > Sparklines | `SPARKLINE(data, options)` — formula-based, inline |
| Goal Seek | Data > What-If > Goal Seek | No native equivalent. Three alternatives: (1) Solver add-on, (2) Custom Apps Script bisection, (3) Python scipy.optimize |

Applicable L1-L5 procedures that benefit from Sheets alternatives:
- **F-014** (Comprehensive Dashboard): SPARKLINE for inline trend indicators, GOOGLEFINANCE for live valuation ratios
- **F-016** (Cross-Sectional Analysis): QUERY for multi-criteria peer aggregation
- **F-066-linked procedures**: QUERY replaces SUMIFS for P&L aggregation in any procedure using database-style aggregation

### Complete Procedure Index

> **ID Namespace Note**: Procedures using `F-NNN` / `C-NNN` format are from the master training index (Finance/Crypto domains). Procedures using `F-PRISM-X01` format are PRISM-specific additions that extend the pipeline beyond the base training curriculum.

#### Tools Level — Excel & Google Sheets Mastery (11 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 1 | F-066 | FMCG Report with SAP Data (P&L Database Case Study) | INTERMEDIATE | excel-sheets-mastery-batch1.md |
| 2 | F-068 | Loan Amortization Schedule Construction | BASIC | excel-sheets-mastery-batch1.md |
| 3 | F-070 | Financial Model Mapping and Sheet Architecture | INTERMEDIATE | excel-sheets-mastery-batch1.md |
| 4 | F-080 | Excel Financial Functions Mastery (TVM) | BASIC | excel-sheets-mastery-batch1.md |
| 5 | F-081 | VLOOKUP/XLOOKUP/INDEX-MATCH for Financial Data | BASIC | excel-sheets-mastery-batch2.md |
| 6 | F-082 | Pivot Table Analysis for Financial Reporting | BASIC | excel-sheets-mastery-batch2.md |
| 7 | F-083 | Professional Dashboard Construction | INTERMEDIATE | excel-sheets-mastery-batch2.md |
| 8 | F-084 | Portfolio Dashboard with Live Market Data | INTERMEDIATE | excel-sheets-mastery-batch2.md |
| 9 | F-085 | Excel Macro for Recurring Financial Reports | INTERMEDIATE | excel-sheets-mastery-batch3.md |
| 10 | F-086 | Goal Seek for Financial Decision-Making | BASIC | excel-sheets-mastery-batch3.md |
| 11 | F-089 | P&L Database Construction with SUMIF | INTERMEDIATE | excel-sheets-mastery-batch3.md |

#### L1 — Read the Numbers (12 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 12 | F-010 | Balance Sheet Construction and Review | BASIC | business-eval-L1-L2.md |
| 13 | F-011 | Cash Flow Statement Analysis | BASIC | business-eval-L1-L2.md |
| 14 | F-012 | Double-Entry Bookkeeping and T-Account Analysis | BASIC | business-eval-L1-L2.md |
| 15 | F-002 | Profitability Ratio Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 16 | F-003 | Liquidity Ratio Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 17 | F-004 | Activity Ratio Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 18 | F-033 | Operating Breakeven Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 19 | F-013 | Annual Report / 10-K Reading | INTERMEDIATE | business-eval-L1-L2.md |

> **NOTE (P3 — GAAP Compliance Cross-Reference)**: During 10-K reading, cross-reference against key ASC standards: ASC 606 (revenue recognition), ASC 842 (leases — verify ROU assets and lease liabilities present if company has significant leases), ASC 350 (goodwill — verify impairment testing disclosed), ASC 718 (stock comp — verify SBC breakdown by function). Departures from these standards are red flags even if audited.

| 20 | F-017 | Inventory Valuation (FIFO vs LIFO) | BASIC | business-eval-L1-L2.md |
| 21 | F-018 | Depreciation Method Analysis | BASIC | business-eval-L1-L2.md |
| 22 | F-021 | EPS Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 23 | F-095 | Personal Net Worth Tracking | BASIC | business-eval-L1-L2.md |

#### L2 — Analyze the Business (16 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 24 | F-001 | DuPont Analysis | ADVANCED | business-eval-L1-L2.md |
| 25 | F-014 | Comprehensive Ratio Dashboard | ADVANCED | business-eval-L1-L2.md |
| 26 | F-015 | Time Series / Trend Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 27 | F-PRISM-V01 | Variance Decomposition | INTERMEDIATE | prism-variance-decomposition.md |
| 28 | F-016 | Comparative / Cross-Sectional Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 29 | F-019 | Leverage and Solvency Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 30 | F-023 | Working Capital Management | INTERMEDIATE | business-eval-L1-L2.md |
| 31 | F-034 | Financial Leverage Impact Analysis | ADVANCED | business-eval-L1-L2.md |
| 32 | F-022 | Executive Compensation Analysis | INTERMEDIATE | business-eval-L1-L2.md |
| 33 | F-006 | Earnings Management Detection | ADVANCED | business-eval-L1-L2.md |
| 34 | F-PRISM-R01 | Reconciliation Validation | INTERMEDIATE | prism-reconciliation-validation.md |
| 35 | F-008 | Benford's Law Fraud Screen | ADVANCED | business-eval-L1-L2.md |
| 36 | F-009 | Non-Financial KPI Linking | ADVANCED | business-eval-L1-L2.md |
| 37 | F-PRISM-O01 | Operational Maturity Score | BASIC | prism-operational-maturity-score.md |
| 38 | F-092 | Credit Analysis Framework | ADVANCED | business-eval-L1-L2.md |
| 39 | F-093 | Macroeconomic Analysis | INTERMEDIATE | business-eval-L1-L2.md |

#### L3 — Valuation (12 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 40 | F-029 | NPV Calculation for Project Evaluation | INTERMEDIATE | business-eval-L3.md |
| 41 | F-030 | IRR Calculation and Multi-Method Project Ranking | INTERMEDIATE | business-eval-L3.md |
| 42 | F-035 | WACC Calculation | ADVANCED | business-eval-L3.md |
| 43 | F-036 | Beta Calculation and Interpretation | ADVANCED | business-eval-L3.md |
| 44 | F-037 | EVA (Economic Value Added) | ADVANCED | business-eval-L3.md |
| 45 | F-039 | DCF Valuation (Company-Level) | ADVANCED | business-eval-L3.md |
| 46 | F-040 | Accounting-Based Valuation (Residual Income) | ADVANCED | business-eval-L3.md |
| 47 | F-041 | Comparable Company Analysis (Trading Comps) | INTERMEDIATE | business-eval-L3.md |
| 48 | F-042 | Comparable Transaction Analysis (Deal Comps) | INTERMEDIATE | business-eval-L3.md |
| 49 | F-043 | Price-to-Revenue Valuation | INTERMEDIATE | business-eval-L3.md |
| 50 | F-044 | P/E Ratio Valuation | INTERMEDIATE | business-eval-L3.md |
| 51 | F-049 | ESG Scoring and Integration | ADVANCED | business-eval-L3.md |

#### L4 — Model the Business (8 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 52 | F-005 | Revenue Financial Forecasting | INTERMEDIATE | business-eval-L4-L5.md |
| 53 | F-065 | 3-Statement Financial Model | ADVANCED | business-eval-L4-L5.md |
| 54 | F-045 | 7-Step Financial Modeling Template | ADVANCED | business-eval-L4-L5.md |
| 55 | F-069 | Sensitivity Analysis | ADVANCED | business-eval-L4-L5.md |
| 56 | F-067 | Tesla-Style Full Financial Model | ADVANCED | business-eval-L4-L5.md |
| 57 | F-046 | Revenue Modeling Case Study (IPO) | ADVANCED | business-eval-L4-L5.md |
| 58 | F-071 | ChatGPT/AI-Assisted Financial Analysis | INTERMEDIATE | business-eval-L4-L5.md |
| 59 | F-031 | Capital Budgeting Full Model Build | ADVANCED | business-eval-L4-L5.md |

#### L5 — Investment Decision (6 Procedures)

| # | ID | Name | Complexity | File |
|---|-----|------|-----------|------|
| 60 | F-062 | 8-Step Investment Research Process | ADVANCED | business-eval-L4-L5.md |
| 61 | F-094 | Venture Capital / Private Investment Evaluation | ADVANCED | business-eval-L4-L5.md |
| 62 | F-096 | Investment Banking M&A Process | ADVANCED | business-eval-L4-L5.md |

> **NOTE (P5 — Audit Trail)**: For institutional-grade due diligence, document the following per each L1-L5 step: data source, date retrieved, assumptions made, methodology chosen and rationale. Use reconciliation validation output (F-PRISM-R01) as data quality gate before building any valuation.

| 63 | F-047 | S-1 / IPO Prospectus Analysis | ADVANCED | business-eval-L4-L5.md |
| 64 | F-090 | Comprehensive Risk Register | ADVANCED | business-eval-L4-L5.md |
| 65 | C-075 | Investment Opportunity Evaluation (Scam Detection) | ADVANCED | business-eval-L4-L5.md |

**Total: 65 procedures** (11 Tools + 12 L1 + 16 L2 + 12 L3 + 8 L4 + 6 L5)

### Tools-to-Pipeline Cross-Reference Map

This table shows which Tools procedures are prerequisites for which L1-L5 procedures:

| Pipeline Cross-Reference | Used By (L1-L5) | Skill Provided |
|----------------|-----------------|----------------|
| F-080 (TVM Functions) | F-029, F-030, F-031, F-039, F-040, F-068 | PV, FV, NPV, IRR, PMT calculations |
| F-081 (VLOOKUP/XLOOKUP) | F-002, F-003, F-004, F-010, F-014, F-041, F-044 | Data retrieval from financial databases |
| F-082 (Pivot Tables) | F-014, F-016, F-066 | Multi-dimensional financial aggregation |
| F-083 (Dashboard) | F-014, F-023, F-093 | Dashboard layout, KPI cards, visual design |
| F-084 (Portfolio Dashboard) | F-062, F-094 | Live market data, portfolio tracking |
| F-086 (Goal Seek) | F-033, F-034, F-069 | Breakeven analysis, what-if solving |
| F-089 (SUMIF P&L) | F-016, F-066 | Database-style P&L aggregation |
| F-070 (Model Mapping) | F-065, F-045, F-067, F-031 | Sheet architecture, cross-sheet references |
| F-085 (Macros) | F-065, F-045 | Report automation, recurring model refresh |
| F-066 (FMCG Report) | F-014, F-016 | Management reporting from transactional data |
| F-068 (Loan Amortization) | F-031, F-092 | Debt schedule construction |
| F-PRISM-R01 (Reconciliation Validation) | L2 F-006, F-008 | Data integrity check feeds anomaly detection |
| F-PRISM-V01 (Variance Decomposition) | L2 F-015, L4 F-005 | Driver analysis for trend and forecast |
| F-PRISM-O01 (Operational Maturity) | L5 F-094, F-090 | Risk discount factor in valuation |

---

## S3 Cortex Logging

```json
{
  "text": "PRISM Pipeline — {action} for {company_name}: {summary}",
  "handoff_prompt": "{prompt_trimisa_catre_executor}",
  "why": "{justificare_decizie}",
  "confidence": 0.0,
  "confidence_flags": [],
  "collection": "business_evaluation",
  "metadata": {
    "type": "pipeline_execution",
    "procedure": "PRISM",
    "domain": "finance",
    "pipeline_level": "{tools|L1|L2|L3|L4|L5}",
    "company": "{company_name}",
    "procedures_completed": "{list_of_procedure_ids}",
    "procedures_skipped": "{list_with_justification}",
    "tags": ["business-evaluation", "prism-pipeline", "training-path"]
  }
}
```

---

## S4 Enforcement

### WHERE
- Training sessions (PRISM pipeline execution for skill-building)
- Live business evaluation tasks (applying PRISM to a real company)
- Procedure audits (verifying pipeline completeness)

### WHEN
- New business evaluation started: verify Tools prerequisites are complete
- Training progression check: verify level completion before advancing
- Investment recommendation issued: verify all mandatory levels were executed

### HOW (violation detection)
- **Skipped level**: L3 valuation attempted without L1-L2 ratio analysis complete -> violation (valuation built on unverified data)
- **Missing tools prerequisites**: Financial model built (L4) without F-070 (model architecture) or F-080 (TVM functions) training -> violation (model may have structural errors)
- **Excel-only execution**: Tools procedure completed in Excel only without documenting the Google Sheets alternative path -> violation for training context (both paths required for tool mastery)
- **No Cortex logging**: Procedure execution not logged to Cortex -> violation (MEM-H-001)
- **Procedure sequencing**: Within a level, executing a procedure before its prerequisite is complete -> warning (acceptable if prerequisite output was obtained from another source)
- Runner: Pipeline completion check against the 65-procedure index. Cortex query for procedure execution records matching the target company.

### CONNECT
- **FORGEBUILD.md**: Template standard for all 65 procedures (`~/.nexus/procedures/FORGEBUILD.md`)
- **AUDIT-PRO**: Audit framework for procedure quality (`~/.nexus/audit/AUDIT-PRO.md`)
- **L1-L2 File**: `tech/business-eval-L1-L2.md` (25 procedures)
- **L3 File**: `tech/business-eval-L3.md` (12 procedures)
- **L4-L5 File**: `tech/business-eval-L4-L5.md` (14 procedures)
- **Tools Batch 1**: `tech/excel-sheets-mastery-batch1.md` (F-066, F-068, F-070, F-080)
- **Tools Batch 2**: `tech/excel-sheets-mastery-batch2.md` (F-081, F-082, F-083, F-084)
- **Tools Batch 3**: `tech/excel-sheets-mastery-batch3.md` (F-085, F-086, F-089)
- **Reconciliation Validation**: `tech/prism-reconciliation-validation.md` (F-PRISM-R01)
- **Variance Decomposition**: `tech/prism-variance-decomposition.md` (F-PRISM-V01)
- **Operational Maturity Score**: `tech/prism-operational-maturity-score.md` (F-PRISM-O01)
- **Finance Plugin Handoff**: `~/.claude/plugins/finance/commands/prism-handoff.md` — optional pre-L1 data package

### VERIFY
- [ ] Tools level: all 11 procedures executed with dual Excel + Google Sheets paths documented?
- [ ] L1: all 12 procedures executed, financial statements verified?
- [ ] L2: all 16 procedures executed, dashboard and anomaly flags produced?
- [ ] L3: at least 3 valuation methods applied (intrinsic + relative)?
- [ ] L4: financial model built with sensitivity analysis?
- [ ] L5: investment recommendation issued with risk assessment?
- [ ] Reconciliation validation (F-PRISM-R01) passed as data quality gate before proceeding to L3?
- [ ] Operational Maturity Score (F-PRISM-O01) factored into L5 valuation discount?
- [ ] No levels skipped without documented justification?
- [ ] All executions logged to Cortex?
- [ ] Google Sheets alternatives documented for applicable procedures?

**VK**:
✅ `[PRISM] Business Evaluation Pipeline | {company_name} | Levels completed: {levels} | {procedure_count}/65 procedures`
✅ `[CORTEX] "PRISM Pipeline — {company_name}" | FORGE | rule: TRAIN-PRISM-001 | v1.2`
