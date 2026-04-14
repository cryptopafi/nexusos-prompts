---
type: procedure
created: 2026-03-17
status: active
slug: business-eval-l4-l5
tags: [procedure, nexus]
---

# Business Evaluation Training Path — Levels 4 & 5 (FORGE Standard)

**Procedure Count**: 14 (8 Level 4 + 6 Level 5)
**Created**: 2026-03-03
**Version**: 1.0
**Format**: FORGE Standard Procedure Template

**Note on §5 (Dependencies)**: All procedures in this file include a §5 Dependencies section beyond the FORGE Standard requirement (§1-§4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**PRISM Pipeline**: This file is part of the PRISM Business Evaluation Pipeline. Master procedure: `~/.nexus/procedures/PRISM.md`
**Tools Prerequisites**: Excel/Sheets mastery procedures in `tech/excel-sheets-mastery-batch1.md`, `batch2.md`, `batch3.md`
**Google Sheets**: All Tools-level procedures support dual Excel + Google Sheets paths. Key Sheets functions: GOOGLEFINANCE, QUERY, FILTER, ARRAYFORMULA, Apps Script.

---

## Table of Contents

### Level 4 — Model the Business
1. [F-005: Revenue Financial Forecasting](#f-005-revenue-financial-forecasting)
2. [F-065: 3-Statement Financial Model](#f-065-3-statement-financial-model)
3. [F-045: 7-Step Financial Modeling Template](#f-045-7-step-financial-modeling-template)
4. [F-069: Sensitivity Analysis](#f-069-sensitivity-analysis)
5. [F-067: Tesla-Style Full Financial Model](#f-067-tesla-style-full-financial-model)
6. [F-046: Revenue Modeling Case Study (IPO)](#f-046-revenue-modeling-case-study-ipo)
7. [F-071: ChatGPT/AI-Assisted Financial Analysis](#f-071-chatgptai-assisted-financial-analysis)
8. [F-031: Capital Budgeting Full Model Build](#f-031-capital-budgeting-full-model-build)

### Level 5 — Investment Decision
9. [F-062: 8-Step Investment Research Process](#f-062-8-step-investment-research-process)
10. [F-094: Venture Capital / Private Investment Evaluation](#f-094-venture-capital--private-investment-evaluation)
11. [F-096: Investment Banking M&A Process](#f-096-investment-banking-ma-process)
12. [F-047: S-1 / IPO Prospectus Analysis](#f-047-s-1--ipo-prospectus-analysis)
13. [F-090: Comprehensive Risk Register](#f-090-comprehensive-risk-register)
14. [C-075: Investment Opportunity Evaluation (Scam Detection)](#c-075-investment-opportunity-evaluation-scam-detection)

---
---

# F-005: Revenue Financial Forecasting

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-005

---

## §1 Problem

Revenue forecasting is the foundation of every financial model — the top line drives all downstream projections (costs, margins, cash flows, valuation). Without a rigorous, multi-scenario revenue forecast grounded in historical data and industry context, the entire financial model rests on guesswork. Skipping this procedure means the 3-statement model, DCF, and investment decision all inherit unquantified error.

**Situations covered**:
- Building revenue projections for any public or private company as part of a full financial model
- Evaluating whether a company's growth trajectory is sustainable relative to its industry
- Preparing base/bull/bear scenario inputs for sensitivity analysis (F-069) and DCF valuation (F-039)

---

## §2 Procedure

**Steps**:
1. Gather 3-5 years of historical revenue data from SEC EDGAR (10-K annual reports) or Yahoo Finance's Financials tab. Record total revenue for each fiscal year. If the company reports quarterly, collect 10-Q data as well for seasonal pattern analysis.
2. Decompose revenue by segment, product line, or geography as disclosed in the company's segment reporting (10-K, Item 7 or Note on Segment Information). For each segment, record revenue, year-over-year growth, and percentage contribution to total revenue. If segment data is unavailable, decompose by revenue type (product vs. service, recurring vs. one-time).
3. Calculate historical growth metrics for each segment and the total: (a) Year-over-year growth rate = (Revenue_t - Revenue_{t-1}) / Revenue_{t-1}. (b) Compound Annual Growth Rate (CAGR) = (Revenue_end / Revenue_start)^(1/n) - 1, where n = number of years. (c) 3-year trailing average growth rate. Flag any year with growth exceeding 2x the CAGR as anomalous (potential M&A, one-time events).
4. Identify and quantify the key revenue drivers for each segment. Common decomposition frameworks: (a) Volume x Price: forecast unit volumes and average selling price (ASP) separately. (b) Market Size x Market Share: estimate total addressable market (TAM) growth and the company's share trajectory. (c) Users x ARPU (Average Revenue Per User): for subscription/SaaS businesses. (d) Same-store growth + new store openings: for retail. Source TAM estimates from industry reports (IBISWorld, Statista, company investor presentations).
5. Model three scenarios for each revenue segment: (a) Base Case: continuation of recent trends adjusted for known factors (consensus analyst estimates for years 1-2 from Yahoo Finance Analysis tab, then taper to industry growth). (b) Bull Case: assume favorable driver outcomes — higher market share gains, pricing power, new product launches succeed. Typically 15-30% above base case in year 5. (c) Bear Case: assume headwinds — competitive pressure, pricing erosion, market slowdown. Typically 15-30% below base case in year 5.
6. Cross-check projections against industry growth rates. Obtain industry revenue growth from IBISWorld, Statista, or Damodaran's industry data (pages.stern.nyu.edu/~adamodar/). If the company's projected growth significantly exceeds industry growth, explicitly state the market share gain assumptions required. Flag if the implied market share exceeds 40% in any segment as potentially unrealistic.
7. Produce quarterly and annual revenue projections for the forecast period (typically 5-10 years). Apply seasonal adjustments if quarterly modeling is required: calculate each quarter's historical share of annual revenue and apply to annual projections. Taper growth rates toward the long-term nominal GDP growth rate (2-3%) by the final projection year for mature businesses.
8. Document all assumptions in a structured assumption table: for each segment, list the driver, historical value, projected value, rationale, and confidence level (High/Medium/Low). Include sources for all external data points.

**Output**: A revenue forecast workbook containing: (a) 3-scenario revenue projections (base/bull/bear) for 5-10 years, quarterly and annual, (b) segment-level driver decomposition table, (c) historical growth metrics and CAGR, (d) assumption table with confidence intervals and source citations, (e) comparison of projections to industry and analyst consensus.

**AI Execution Notes**: Pull historical revenue from Yahoo Finance Financials tab or SEC EDGAR XBRL data. Analyst consensus revenue estimates from Yahoo Finance Analysis tab (years 1-2). Industry growth data from Damodaran's revenue growth by industry dataset. For segment decomposition, the 10-K's Management Discussion and Analysis (MD&A) section typically provides the most granular breakdown. If modeling a private company, request revenue data directly from the owner and benchmark against public comparables.

**Key Formulas**:
- YoY Growth Rate = (Revenue_t - Revenue_{t-1}) / Revenue_{t-1}
- CAGR = (Revenue_end / Revenue_start)^(1/n) - 1
- Revenue (Volume x Price) = Unit Volumes x ASP

---

## §3 Cortex Logging

```json
{
  "text": "F-005 Revenue Financial Forecasting — revenue model for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-005",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "revenue-forecasting"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: When building projections for any business — this is the first modeling step before any 3-statement model or valuation
**PREREQUISITES**: F-013 (10-K Reading), F-015 (Trend Analysis), L1/L2 financial statement data
**FEEDS INTO**: F-065 (3-Statement Model), F-045 (7-Step Template), F-067 (Tesla-Style Model), F-046 (IPO Revenue Model), F-039 (DCF Valuation)

**VK**: `✅ [F-005] Revenue Financial Forecasting | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-013 | Requires | 10-K filing provides historical revenue data and segment disclosures |
| F-015 | Requires | Trend analysis establishes growth patterns and anomaly flags |
| F-065 | Feeds into | Revenue projections are the top-line input to the 3-statement model |
| F-039 | Feeds into | Revenue drives free cash flow projections used in DCF |
| F-069 | Feeds into | Revenue growth rate is a key sensitivity variable |

---
---

# F-065: 3-Statement Financial Model

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-065

---

## §1 Problem

A fully linked 3-statement model (income statement, balance sheet, cash flow statement) is the core analytical engine of any business evaluation. Without it, valuation methods like DCF have no reliable cash flow inputs, and sensitivity analysis has no model to stress-test. Skipping this procedure means the AI cannot produce a credible intrinsic valuation or assess the financial health of a business over time.

**Situations covered**:
- Building a complete financial model for a public company from SEC filings
- Projecting future financial performance to support DCF valuation (F-039)
- Stress-testing financial health under different revenue and cost assumptions

---

## §2 Procedure

**Steps**:
1. Build the Revenue Schedule: input historical revenue for 3-5 years, then project forward using F-005 (Revenue Financial Forecasting). The revenue line is the primary input that drives the entire model. Link projected revenue to the income statement top line.
2. Build the Income Statement: (a) Cost of Goods Sold (COGS) = Revenue x COGS margin (use historical average, adjusted for trend). (b) Gross Profit = Revenue - COGS. (c) Operating Expenses: project SG&A, R&D, and other OpEx as percentages of revenue (use historical ratios, adjusted for operating leverage). (d) EBITDA = Gross Profit - OpEx. (e) Depreciation & Amortization = forecast from the D&A schedule (see step 4). (f) EBIT = EBITDA - D&A. (g) Interest Expense = Average Debt Balance x Interest Rate (from debt schedule, step 5). (h) EBT = EBIT - Interest Expense + Interest Income. (i) Taxes = EBT x Tax Rate (use effective tax rate from historical data, typically 18-25% for US companies). (j) Net Income = EBT - Taxes.
3. Build Working Capital Schedules: (a) Accounts Receivable = Revenue x (DSO / 365), where DSO = Days Sales Outstanding (historical average). (b) Inventory = COGS x (DIO / 365), where DIO = Days Inventory Outstanding. (c) Accounts Payable = COGS x (DPO / 365), where DPO = Days Payable Outstanding. (d) Net Working Capital = Current Assets (AR + Inventory + Prepaid + Other CA) - Current Liabilities (AP + Accrued Expenses + Other CL). (e) Change in NWC = NWC_t - NWC_{t-1}. A positive change means cash consumption (more capital tied up); negative means cash release.
4. Build the Capex and Depreciation Schedule: (a) Capital Expenditures: project as % of revenue (use historical average, adjusted for known capex plans from company guidance or 10-K disclosures). (b) Gross PP&E_t = Gross PP&E_{t-1} + Capex_t - Retirements. (c) Accumulated Depreciation_t = Accumulated Depreciation_{t-1} + Depreciation_t - Retirements. (d) Net PP&E_t = Gross PP&E_t - Accumulated Depreciation_t. (e) Depreciation: use historical D&A/Gross PP&E ratio or straight-line method based on average useful life of assets.
5. Build the Debt Schedule: (a) List each debt tranche (revolving credit facility, term loans, bonds) with principal, interest rate, and maturity. (b) Model mandatory repayments per the amortization schedule. (c) Model optional repayments or new issuances based on cash flow availability and target leverage ratio. (d) Calculate interest expense for each tranche: Interest = Average Balance x Rate. (e) Beginning Debt + New Issuances - Repayments = Ending Debt.
6. Build the Balance Sheet: (a) Assets: Cash (from cash flow statement ending balance), AR, Inventory, Prepaid, Other Current Assets, Net PP&E, Intangibles, Goodwill, Other LT Assets. (b) Liabilities: AP, Accrued Expenses, Current Portion of Debt, LT Debt (from debt schedule), Other LT Liabilities. (c) Equity: Common Stock, Additional Paid-In Capital, Retained Earnings (RE_t = RE_{t-1} + Net Income_t - Dividends_t), Treasury Stock, Accumulated Other Comprehensive Income. (d) Verify: Total Assets = Total Liabilities + Total Equity. If imbalanced, check the cash line or revolver drawdown as the balancing plug.
7. Build the Cash Flow Statement: (a) Operating Activities: Net Income + D&A + Stock-Based Compensation + Deferred Taxes + Changes in Working Capital (decrease in AR = source; increase in AR = use). (b) Investing Activities: -Capex + Proceeds from Asset Sales + Acquisitions. (c) Financing Activities: Debt Issuances - Debt Repayments - Dividends Paid - Share Buybacks + Equity Issuances. (d) Net Change in Cash = Operating + Investing + Financing. (e) Ending Cash = Beginning Cash + Net Change in Cash.
8. Link the three statements: (a) Net Income from the IS flows to Retained Earnings on the BS and is the starting point of the CFS. (b) D&A from the IS is added back in operating cash flow and reduces Net PP&E on the BS. (c) Ending Cash from the CFS flows to the Cash line on the BS. (d) Interest Expense on the IS comes from the Debt Schedule which feeds the BS. (e) Changes in WC items on the CFS correspond to BS changes in AR, Inventory, AP.
9. Perform a circular reference check: if interest expense depends on average debt, and debt depends on cash flow, which depends on interest expense, a circularity exists. Resolve by: (a) using beginning-of-period debt for interest calculation (breaks the circle), or (b) enabling iterative calculation and verifying convergence (values stabilize within 3-5 iterations to within $0.01).
10. Run balancing and integrity checks: (a) BS balances every period (Assets = L + E, difference = $0). (b) Cash on BS equals ending cash on CFS. (c) Retained Earnings reconciliation: RE_end = RE_begin + NI - Dividends. (d) Debt on BS equals ending debt from debt schedule. (e) Net PP&E on BS equals net PP&E from capex/depreciation schedule. Flag any discrepancy.
11. Add sensitivity input cells: key assumptions (revenue growth, COGS margin, OpEx % of revenue, capex % of revenue, interest rate, tax rate, DSO, DIO, DPO) should be isolated in a clearly labeled assumptions section so scenarios can be toggled easily.

**Output**: A fully linked 3-statement financial model containing: (a) income statement, balance sheet, and cash flow statement for 3-5 historical years and 5-10 projected years, (b) supporting schedules (revenue, working capital, D&A/capex, debt), (c) assumption input panel, (d) integrity check summary confirming all statements balance and link correctly.

**AI Execution Notes**: Source historical data from SEC EDGAR 10-K filings (XBRL for structured data) or Yahoo Finance Financials tab. Revenue projections from F-005. WACC from F-035 (used later for DCF but not directly in the 3-statement model). For the circular reference involving interest, prefer the beginning-of-period debt method for simplicity. When building for a specific company, check the 10-K's Notes to Financial Statements for debt terms, lease obligations, and depreciation methods. The model feeds directly into F-039 (DCF Valuation) and F-069 (Sensitivity Analysis).

**Tools prerequisites**: F-070 (Model Mapping) for sheet architecture and cross-sheet references. F-081 (VLOOKUP/XLOOKUP) for data retrieval via INDEX-MATCH. F-085 (Macros) for automating model refresh. Google Sheets: use ARRAYFORMULA for broadcasting calculations across projection years.

**Key Formulas**:
- UFCF = EBIT x (1 - T) + D&A - Capex - Change in NWC
- Accounts Receivable = Revenue x (DSO / 365)
- Net Change in Cash = Operating CF + Investing CF + Financing CF

---

## §3 Cortex Logging

```json
{
  "text": "F-065 3-Statement Financial Model — full model for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-065",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "3-statement-model"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: After revenue forecasting is complete, when building the full financial model for any company
**PREREQUISITES**: F-005 (Revenue Forecast), F-010 (Balance Sheet Analysis), F-011 (Cash Flow Analysis), L1/L2 historical financial data
**FEEDS INTO**: F-039 (DCF Valuation), F-069 (Sensitivity Analysis), F-067 (Tesla-Style Model), F-045 (7-Step Template)

**VK**: `✅ [F-065] 3-Statement Financial Model | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-005 | Requires | Revenue projections are the top-line driver of the entire model |
| F-010 | Requires | Balance sheet analysis provides historical BS data and ratios |
| F-011 | Requires | Cash flow analysis provides historical CFS structure and patterns |
| F-035 | Requires | WACC needed for debt schedule interest rate assumptions |
| F-039 | Feeds into | UFCF from the model is the input to DCF valuation |
| F-069 | Feeds into | Model assumptions become the variables for sensitivity analysis |

---
---

# F-045: 7-Step Financial Modeling Template

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-045

---

## §1 Problem

Analysts need a repeatable, standardized workflow that integrates data gathering, ratio analysis, assumption setting, 3-statement projection, and DCF valuation into a single end-to-end template. Without this meta-template, each new company analysis requires reinventing the workflow from scratch, increasing error risk and slowing execution. Skipping this means the AI lacks a structured blueprint for consistent, high-quality financial modeling across any company.

**Situations covered**:
- Running a complete financial analysis for any public company from raw filings to valuation
- Teaching or standardizing the financial modeling workflow across multiple analysts
- Rapid deployment of a full model by swapping only the historical data input for a new company

---

## §2 Procedure

**Steps**:
1. **Input Historical Data**: Collect 3-5 years of historical financial statements (income statement, balance sheet, cash flow statement) from SEC EDGAR 10-K filings or Yahoo Finance. Organize in a standardized layout: years as columns (left to right, oldest to newest), line items as rows. Color-code: blue font for hard-coded inputs, black for formulas. Record the source and date of each data point.
2. **Calculate Historical Ratios**: For each historical year, compute: (a) Profitability: Gross Margin = Gross Profit / Revenue, Operating Margin = EBIT / Revenue, Net Margin = Net Income / Revenue, ROE = Net Income / Average Equity, ROIC = NOPAT / Invested Capital. (b) Efficiency: Asset Turnover = Revenue / Average Total Assets, DSO, DIO, DPO. (c) Leverage: Debt/Equity, Debt/EBITDA, Interest Coverage = EBIT / Interest Expense. (d) Growth: Revenue growth, EBITDA growth, EPS growth. Compute 3-year and 5-year averages for each ratio to establish baseline trends.
3. **Build Projection Assumptions**: For each key driver, decide the projected value based on: (a) historical average (default starting point), (b) management guidance (from earnings calls, investor presentations), (c) industry benchmarks (Damodaran's industry data), (d) analyst consensus (Yahoo Finance Analysis tab). Document the assumption, its source, and whether it represents base/bull/bear case. Key assumptions to set: revenue growth rate, COGS % of revenue, SG&A % of revenue, R&D % of revenue, D&A % of revenue, capex % of revenue, tax rate, DSO, DIO, DPO, dividend payout ratio.
4. **Project the Income Statement**: Starting from revenue (driven by growth rate assumption), calculate each line item using the ratio assumptions from step 3. Revenue x COGS% = COGS. Revenue - COGS = Gross Profit. Continue through EBITDA, EBIT, EBT, Tax, Net Income. Verify that projected margins are within the historical range or have explicit justification for deviation.
5. **Project the Balance Sheet**: Use efficiency ratios to project working capital items (AR = Revenue x DSO/365, etc.). Use capex/D&A schedules to project PP&E. Use the debt schedule for debt balances. Retained Earnings = Prior RE + Net Income - Dividends. Use cash or a revolving credit facility as the balancing plug to ensure Assets = Liabilities + Equity.
6. **Project the Cash Flow Statement**: Derive from projected IS and BS: Operating CF = Net Income + non-cash charges + WC changes. Investing CF = -Capex. Financing CF = net debt changes - dividends - buybacks. Ending Cash = Beginning Cash + Net Change. Verify ending cash matches the BS cash line.
7. **Run DCF/Valuation from Projected Statements**: Extract Unlevered Free Cash Flow (UFCF) from projections: UFCF = EBIT x (1-T) + D&A - Capex - Change in NWC. Discount at WACC (from F-035) to calculate Enterprise Value (per F-039). Bridge to equity value. Perform sensitivity analysis on WACC and terminal growth rate. Present implied share price vs. current market price.

**Output**: A standardized 7-step modeling template that produces: (a) organized historical data, (b) ratio analysis dashboard, (c) assumption panel, (d) projected 3-statement model for 5-10 years, (e) DCF valuation with sensitivity matrix, (f) applicable to any public company by swapping the historical data input.

**AI Execution Notes**: This procedure is a meta-template that integrates F-005 (revenue forecasting), F-065 (3-statement model), F-035 (WACC), and F-039 (DCF). When executing for a specific company, run steps 1-2 as data gathering, steps 3-6 as the core model build (equivalent to F-065), and step 7 as the valuation layer (equivalent to F-039). The template standardizes the workflow so it can be applied to any company with minimal structural changes — only the historical data and assumptions change.

**Tools prerequisites**: F-070 (Model Mapping) for standardized sheet architecture. F-081 (VLOOKUP/XLOOKUP) for step 1 data retrieval. F-082 (Pivot Tables) for step 2 historical ratio computation.

**Key Formulas**:
- ROIC = NOPAT / Invested Capital
- UFCF = EBIT x (1 - T) + D&A - Capex - Change in NWC
- Interest Coverage = EBIT / Interest Expense

---

## §3 Cortex Logging

```json
{
  "text": "F-045 7-Step Financial Modeling Template — full template for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-045",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "7-step-template"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: When a complete end-to-end financial model is needed for any public company, from data gathering through valuation
**PREREQUISITES**: F-065 (3-Statement Model — conceptual framework understanding required; F-045 is a self-contained meta-template that integrates the 3-statement build as steps 4-6, so F-065 does not need to be executed separately before F-045)
**FEEDS INTO**: F-069 (Sensitivity Analysis), F-062 (Investment Research — step 5 valuation), Final Investment Memo for Pafi

**VK**: `✅ [F-045] 7-Step Financial Modeling Template | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-005 | Requires | Revenue forecasting drives the projection assumptions |
| F-065 | Requires | 3-statement model structure is the core of steps 4-6 |
| F-035 | Requires | WACC is the discount rate for the DCF in step 7 |
| F-039 | Requires | DCF methodology applied in step 7 |
| F-069 | Feeds into | Completed model is the input for sensitivity analysis |
| F-062 | Feeds into | Valuation output feeds into the investment research process |

---
---

# F-069: Sensitivity Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-069

---

## §1 Problem

Every financial model depends on assumptions that are inherently uncertain. Sensitivity analysis quantifies how changes in key variables affect valuation outcomes, revealing which assumptions matter most and where the margin of safety lies. Without it, the AI presents a single-point estimate that conceals the range of possible outcomes, making investment decisions fragile. Skipping this means Pafi sees only the base case with no understanding of upside/downside scenarios or breakeven thresholds.

**Situations covered**:
- Stress-testing a DCF valuation to understand which assumptions drive the most value
- Identifying breakeven thresholds for key variables (e.g., the minimum revenue growth for positive NPV)
- Producing probability-weighted expected values across bull/base/bear scenarios

---

## §2 Procedure

**Steps**:
1. Identify the key value drivers from the financial model (F-065 or F-045) that have the greatest impact on output metrics. Typical drivers: revenue growth rate, COGS margin, operating expense ratio, WACC, terminal growth rate, capex % of revenue, tax rate, working capital days (DSO/DIO/DPO). Rank by expected impact based on the model's structure.
2. Define a realistic range for each variable: (a) Revenue growth: base +/- 2-5 percentage points. (b) COGS margin: base +/- 1-3 percentage points. (c) WACC: base +/- 1-2 percentage points. (d) Terminal growth rate: 1.0% to 3.0%. (e) Capex % revenue: base +/- 1-2 percentage points. Source the ranges from historical volatility, management guidance ranges, and analyst estimate dispersion (high vs. low estimates from Yahoo Finance).
3. Build a one-variable sensitivity table for each key driver: hold all other variables at base case, vary the selected driver across its range (minimum 5 data points, ideally 7-9), and record the output metrics: NPV (from F-039), IRR (from F-030), implied equity value per share, and EBITDA margin for the terminal year.
4. Build two-variable sensitivity tables for the most impactful driver pairs: (a) WACC vs. terminal growth rate (the standard DCF sensitivity matrix — this is the most critical). (b) Revenue growth vs. COGS margin (shows operating leverage). (c) Revenue growth vs. capex intensity. Format as a grid: one variable on the horizontal axis (5-7 values), the other on the vertical axis (5-7 values), with the output metric at each intersection.
5. Create a tornado chart: for each driver, calculate the output metric at its low and high range values (all others at base). Rank drivers by the absolute spread (high value minus low value of the output metric). Display horizontally: the variable with the widest bar (highest impact) at the top, narrowest at the bottom. The tornado chart visually identifies which assumptions matter most.
6. Identify breakeven thresholds for each key variable: the value at which NPV = 0 (or implied share price = current market price). For example: "Revenue growth must fall below 2.1% for NPV to turn negative" or "WACC must exceed 11.3% to make the stock appear overvalued." These thresholds define the margin of safety.
7. Color-code the sensitivity tables: Green = scenarios where the investment is attractive (NPV > 0, upside > 15%). Yellow = marginal (NPV near zero, upside/downside within 15%). Red = scenarios where the investment is unattractive (NPV < 0, downside > 15%). Calculate the percentage of all tested scenarios that fall into each zone.
8. Produce a scenario probability assessment: assign subjective probabilities to the base/bull/bear scenarios (e.g., 50%/25%/25%) and calculate the probability-weighted expected value: E(NPV) = P_base x NPV_base + P_bull x NPV_bull + P_bear x NPV_bear. Report the expected value and the range of outcomes. **Warning**: Probability assignments are inherently subjective and susceptible to anchoring bias. Always test results with at least one alternative weighting scheme (e.g., 40/30/30 or equal-weighted 33/33/33) to assess sensitivity to the probability assumptions themselves. If the investment decision changes under alternative weightings, the analysis is inconclusive and requires additional scenario definition.

**Output**: A sensitivity analysis package containing: (a) one-variable sensitivity tables for each key driver, (b) two-variable sensitivity matrices (WACC vs. terminal growth, revenue growth vs. margin), (c) tornado chart ranking drivers by impact, (d) breakeven thresholds for each variable, (e) color-coded scenario classification (green/yellow/red), (f) probability-weighted expected value.

**AI Execution Notes**: The sensitivity analysis operates on top of an existing financial model (F-065 or F-045) and valuation (F-039). When building programmatically, use a parameter sweep approach: define the base model, then iterate each variable across its range while holding others constant. For the tornado chart, sort the output list by absolute spread in descending order. For the two-variable tables, use a nested loop. WACC comes from F-035; terminal growth rate is an assumption in F-039. If the model has a circular reference, ensure each scenario iteration converges before recording the output.

**Tools prerequisite**: F-086 (Goal Seek) for finding breakeven thresholds in step 6 (the exact value where NPV = 0). Google Sheets: use Solver add-on or Apps Script bisection (see F-086).

**Key Formulas**:
- E(NPV) = P_base x NPV_base + P_bull x NPV_bull + P_bear x NPV_bear
- Breakeven Threshold = value of variable where NPV = 0
- Tornado Spread = |Output_at_high - Output_at_low| for each driver

---

## §3 Cortex Logging

```json
{
  "text": "F-069 Sensitivity Analysis — sensitivity package for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-069",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "sensitivity-analysis"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: After a financial model and DCF valuation are complete, before any investment recommendation
**PREREQUISITES**: F-065 (3-Statement Model) or F-045 (7-Step Template), F-039 (DCF Valuation) or F-029 (NPV), F-035 (WACC)
**FEEDS INTO**: F-062 (Investment Research — risk assessment), F-067 (Tesla-Style Model — step 9), F-096 (M&A — synergy sensitivity), Final Investment Memo for Pafi

**VK**: `✅ [F-069] Sensitivity Analysis | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-065 / F-045 | Requires | The financial model provides the base case assumptions to vary |
| F-039 / F-029 | Requires | DCF/NPV is the base valuation output being stress-tested |
| F-035 | Requires | WACC is both a model input and a key sensitivity variable |
| F-030 | Requires | IRR calculation for each scenario iteration |
| F-062 | Feeds into | Sensitivity results inform risk assessment in investment research |
| F-096 | Feeds into | Synergy sensitivity in M&A analysis |

---
---

# F-067: Tesla-Style Full Financial Model

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-067

---

## §1 Problem

Complex multi-segment, high-growth companies (like Tesla, Amazon, or any conglomerate) cannot be modeled with a simple single-segment approach. Their revenue drivers, cost structures, and capex profiles differ radically across segments. Without a segment-level model that captures volume x price dynamics, per-unit economics, and project-level capex, the resulting valuation will be too coarse to support a credible investment decision. Skipping this means the AI produces a flat-line projection that misses operating leverage inflection points and segment-specific risks.

**Situations covered**:
- Building a comprehensive model for a multi-segment, high-growth company
- Modeling companies with complex revenue drivers (volume x price, capacity ramps, mix effects)
- Producing an investment recommendation for companies where terminal value assumptions dominate

---

## §2 Procedure

**Steps**:
1. Research company-specific revenue drivers for a complex multi-segment company (using Tesla as the archetype, but applicable to any multi-segment business). Identify each business segment and its unique drivers: (a) Automotive: vehicle delivery volumes by model, average selling price (ASP) including mix effects, regulatory credit revenue. (b) Energy Generation/Storage: solar deployments (MW), battery storage deployments (MWh), revenue per unit. (c) Services & Other: supercharging, insurance, merchandise, used vehicle sales, service revenue per vehicle in fleet. Source segment data from 10-K segment reporting, earnings call transcripts, and investor presentations.
2. Build a segment-level revenue model: for each segment, model the volume driver and the price/monetization driver separately. (a) Automotive Revenue = Deliveries x ASP. Project deliveries based on factory capacity (nameplate capacity x utilization rate x ramp schedule) and demand. Project ASP based on model mix, pricing trends, and geographic mix. (b) Energy Revenue = Deployments x Revenue/Unit. (c) Services Revenue = Active Fleet Size x Revenue/Vehicle. Sum segment revenues to get total company revenue. Cross-check total against analyst consensus (Yahoo Finance Analysis tab).
3. Model COGS with detailed cost structure: (a) Materials cost per unit (battery cells, raw materials — track commodity price trends for lithium, nickel, cobalt). (b) Direct labor per unit (adjusted for automation improvements). (c) Manufacturing overhead per unit (fixed costs / volume — this is where operating leverage emerges as volume scales). (d) Warranty provisions (typically 1-3% of automotive revenue). (e) Regulatory credit COGS = $0 (pure profit). (f) Total COGS = sum of per-unit costs x volume + segment-specific costs. Calculate gross margin by segment and total.
4. Model operating expenses: (a) R&D: project as % of revenue, adjusted for product development cycle (higher during new model development, lower during production ramp). Separate from stock-based compensation (SBC) for clarity. (b) SG&A: project as % of revenue, adjusting for operating leverage (SG&A% should decline as revenue scales). (c) Stock-Based Compensation: model separately as it is a non-cash expense but represents real dilution. Project based on historical SBC/Revenue ratio and headcount growth.
5. Build the capex schedule with project-level detail: (a) Factory buildouts: estimate cost per new gigafactory ($5-10B typical for auto/battery) and deployment timeline. (b) Equipment and tooling for new models. (c) Maintenance capex for existing facilities (typically 2-3% of gross PP&E). (d) Energy infrastructure (supercharger network, grid-scale storage manufacturing). Total capex = expansion capex + maintenance capex. Model the ramp: capex is front-loaded, revenue follows with a 1-3 year lag.
6. Model capital raises and debt/equity structure: (a) Project cash consumption during expansion phase — will the company need additional capital raises? (b) Model debt issuances (convertible notes, bonds) and equity raises (stock offerings, ATM programs). (c) Track diluted share count: base shares + options/warrants (using treasury stock method) + convertible note dilution (using if-converted method). (d) Update the debt schedule per F-065 step 5.
7. Integrate into a full 3-statement model per F-065: IS, BS, CFS all linked. Apply the same integrity checks (BS balance, cash reconciliation, RE reconciliation). The segment-level detail feeds into the consolidated statements.
8. Perform DCF valuation per F-039 with growth-specific considerations: (a) Use a longer explicit forecast period (10-15 years) for high-growth companies since a greater portion of value comes from the growth phase. (b) Calculate UFCF from consolidated projections. (c) Terminal value: use a fade-to-maturity approach — terminal margins based on mature industry benchmarks, terminal growth rate at 2-3%. (d) Discount at WACC (from F-035), noting that high-growth companies often have higher WACCs due to higher betas. **Critical note**: In high-growth company models, terminal value typically represents 60-80% of total enterprise value. This makes terminal assumptions (growth rate, exit multiple, fade period) the single highest-risk element of the entire valuation. Sensitivity-test terminal growth rate ±0.5% and exit multiple ±1.0x as mandatory checks. If terminal value exceeds 85% of EV, extend the explicit forecast period or re-examine growth assumptions.
9. Compare the implied equity value per share to the current market cap. Calculate the upside/downside percentage. Run the full sensitivity analysis (F-069) on key drivers: delivery volume, ASP, gross margin trajectory, capex intensity, WACC, and terminal growth rate.
10. Produce an investment recommendation: based on the base case valuation vs. market price, the sensitivity analysis results, and a qualitative assessment of execution risk (can the company actually deliver on volume/margin targets?). Classify as: Strong Buy (>30% upside with manageable risk), Buy (15-30% upside), Hold (-15% to +15%), Sell (>15% downside).

**Output**: A comprehensive financial model for a complex multi-segment company containing: (a) segment-level revenue models with volume x price decomposition, (b) detailed cost structure with per-unit economics, (c) project-level capex schedule, (d) fully linked 3-statement model (10-15 year projection), (e) DCF valuation with 3-scenario analysis, (f) sensitivity matrices on key growth drivers, (g) investment recommendation with bull/base/bear price targets.

**AI Execution Notes**: For Tesla specifically, delivery data is published quarterly in press releases and tracked by Bloomberg/analyst models. Factory capacity estimates are available from earnings calls and investor day presentations. ASP can be estimated from automotive revenue / deliveries. For other multi-segment companies, adapt the segment structure to match the company's reporting. This procedure integrates F-005 (revenue forecasting), F-065 (3-statement model), F-039 (DCF), and F-069 (sensitivity analysis) into a single comprehensive workflow.

**Tools prerequisites**: F-070 (Model Mapping) for multi-sheet architecture across segments. F-083 (Dashboard) for the visual output layer. Google Sheets: use GOOGLEFINANCE for live market data integration in step 9.

**Key Formulas**:
- Automotive Revenue = Deliveries x ASP
- Manufacturing Overhead per Unit = Fixed Costs / Volume (operating leverage)
- Diluted Shares = Base Shares + (In-the-money Options proceeds / Share Price) via Treasury Stock Method

---

## §3 Cortex Logging

```json
{
  "text": "F-067 Tesla-Style Full Financial Model — multi-segment model for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-067",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "multi-segment-model"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: When modeling any complex multi-segment or high-growth company that requires segment-level granularity
**PREREQUISITES**: F-005 (Revenue Forecast), F-065 (3-Statement Model), F-035 (WACC), F-039 (DCF Valuation), all L1-L3 outputs for the company
**FEEDS INTO**: F-062 (Investment Research — comprehensive model input), Final Investment Memo for Pafi

**VK**: `✅ [F-067] Tesla-Style Full Financial Model | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-005 | Requires | Segment-level revenue forecasting methodology |
| F-065 | Requires | 3-statement model structure for consolidation |
| F-035 | Requires | WACC for DCF discount rate (step 8) |
| F-039 | Requires | DCF valuation methodology (step 8) |
| F-069 | Requires | Sensitivity analysis framework (step 9) |
| F-062 | Feeds into | Comprehensive model feeds the investment research process |

---
---

# F-046: Revenue Modeling Case Study (IPO)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-046

---

## §1 Problem

IPO candidates present unique modeling challenges: limited historical data (often only 2-3 years), inflated pre-IPO growth rates, unproven path to profitability, and no established analyst consensus. Without a structured IPO revenue model that accounts for customer economics, TAM penetration, and growth deceleration, the AI cannot assess whether the IPO is fairly priced or determine if the business model is sustainable. Skipping this means accepting the S-1 narrative at face value without independent verification.

**Situations covered**:
- Modeling revenue for a company filing an S-1 registration statement ahead of IPO
- Evaluating whether an IPO is priced attractively relative to public comparables
- Assessing the sustainability of pre-IPO growth rates and the path to profitability

---

## §2 Procedure

**Steps**:
1. Obtain the S-1 registration statement from SEC EDGAR (search by company name, form type S-1 or S-1/A). Read the following sections in order: Business Overview, Risk Factors, MD&A (Management's Discussion and Analysis of Financial Condition and Results of Operations), and Use of Proceeds. Download the full filing for detailed financial data.
2. Identify the revenue model type from the S-1 disclosures: (a) Subscription/SaaS: recurring revenue, track ARR (Annual Recurring Revenue), MRR (Monthly Recurring Revenue), net retention rate. (b) Transactional: per-transaction fees, track transaction volume x take rate. (c) Advertising: track MAU/DAU, ad impressions, CPM/CPC. (d) Marketplace: track GMV (Gross Merchandise Value) x take rate. (e) Hybrid: identify which components are recurring vs. non-recurring and model separately.
3. Model customer acquisition and churn dynamics: (a) Extract customer count data from the S-1 (often disclosed in key metrics). (b) Calculate historical customer acquisition cost (CAC) = Sales & Marketing Expense / New Customers Added. (c) Calculate churn rate = Customers Lost / Beginning Customers. (d) Calculate net retention rate (NRR) = (Beginning Revenue + Expansion - Contraction - Churn) / Beginning Revenue. NRR > 100% indicates existing customers are growing; NRR > 120% is exceptional for SaaS. (e) Project future customer count: Customers_t = Customers_{t-1} x (1 - Churn) + New Customers.
4. Project revenue ramp using the TAM/SAM/SOM framework: (a) Total Addressable Market (TAM): the total market opportunity if 100% penetration. Source from industry reports cited in the S-1 or from independent research (Gartner, IDC, Grand View Research). (b) Serviceable Addressable Market (SAM): the portion of TAM the company can realistically target given geography, product, and pricing. (c) Serviceable Obtainable Market (SOM): the market share the company can realistically capture over the forecast period. (d) Revenue projection = SOM x implied revenue per unit of market captured. Cross-check against the bottom-up model (customers x ARPU).
5. Model the path to profitability: (a) Identify the company's current gross margin and its trajectory. (b) Model operating expense leverage: as revenue scales, S&M % should decline (customer payback period shortens), R&D % stabilizes, G&A % declines. (c) Calculate the implied breakeven revenue: the revenue level at which operating income = $0, based on the cost structure. (d) Project the timeline to profitability (operating profit positive, then net income positive, then FCF positive). (e) Flag if the S-1 shows no clear path to profitability within 5 years.
6. Estimate post-IPO growth trajectory: (a) Pre-IPO growth is often inflated by low-base effects and aggressive spending. (b) Model growth deceleration: apply the "law of large numbers" — growth rate typically declines as revenue base grows. (c) Use the S-curve framework: early hypergrowth (>50% YoY) tapers to moderate growth (15-25%) then to mature growth (5-10%) over 5-10 years. (d) Benchmark against historical IPO cohorts in the same industry: how did comparable companies' growth evolve post-IPO?
7. Compare the projected revenue to the IPO valuation range: (a) The S-1 will eventually include a proposed IPO price range (in the S-1/A filing or pricing amendment). (b) Calculate the implied valuation multiples at the midpoint of the IPO range: EV/Revenue (NTM), P/S, EV/ARR (for SaaS). (c) Compare to publicly traded comparables (use F-041 trading comps methodology). (d) If the implied multiple is above the peer median, the IPO is priced at a premium — assess whether the growth rate justifies it.
8. Assess IPO pricing attractiveness and produce a recommendation: (a) Calculate the fair value per share based on your revenue model and peer multiples. (b) Determine if the IPO price represents a discount or premium to fair value. (c) Consider the typical "IPO pop" (first-day premium of 10-20% on average) — the underwriters typically price below fair value to ensure a successful offering. (d) Recommendation: Subscribe (fair value > IPO price by >15%), Pass (fair value < IPO price or margin of safety insufficient), Watch (close to fair value, monitor post-IPO trading for entry point).

**Output**: An IPO revenue model containing: (a) revenue model type identification and decomposition, (b) customer acquisition/churn model, (c) TAM/SAM/SOM framework with market sizing, (d) 5-10 year revenue projection with path to profitability timeline, (e) IPO valuation analysis vs. public comps, (f) fair value estimate per share, (g) investment recommendation (Subscribe/Pass/Watch).

**AI Execution Notes**: S-1 filings are available on SEC EDGAR (search form type S-1). Pre-IPO financials may be limited to 2-3 years. For customer metrics not explicitly disclosed, estimate from revenue and ARPU data. Comparable public company data from Yahoo Finance. Post-IPO, the company must file 10-K and 10-Q, enabling more detailed analysis. This procedure integrates F-005 (revenue forecasting), F-041 (trading comps), and F-043 (P/Revenue valuation) with IPO-specific analysis.

**Key Formulas**:
- CAC = Sales & Marketing Expense / New Customers Added
- NRR = (Beginning Revenue + Expansion - Contraction - Churn) / Beginning Revenue
- Customers_t = Customers_{t-1} x (1 - Churn) + New Customers

---

## §3 Cortex Logging

```json
{
  "text": "F-046 Revenue Modeling Case Study (IPO) — IPO revenue model for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-046",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "ipo-revenue-model"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: When a company files an S-1 and is approaching IPO, before making a subscribe/pass decision
**PREREQUISITES**: F-005 (Revenue Forecast methodology), F-041 (Trading Comps for peer comparison), F-043 (P/Revenue valuation)
**FEEDS INTO**: F-047 (S-1 / IPO Prospectus Analysis — full qualitative + quantitative), Final Investment Memo for Pafi

**VK**: `✅ [F-046] Revenue Modeling Case Study (IPO) | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-005 | Requires | Revenue forecasting methodology adapted for limited IPO data |
| F-041 | Requires | Trading comps provide peer multiples for IPO pricing comparison |
| F-043 | Requires | P/Revenue valuation for pre-profit companies |
| F-047 | Feeds into | Revenue model feeds the comprehensive S-1 analysis |
| F-062 | Feeds into | IPO analysis contributes to the full investment research process |

---
---

# F-071: ChatGPT/AI-Assisted Financial Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-071

---

## §1 Problem

AI tools are powerful calculators and drafters but are unreliable for real-time financial data and can produce plausible-sounding but incorrect calculations. Without a structured workflow for AI-assisted analysis that enforces data provision, explicit formula requests, and rigorous output validation, the AI may generate unverified financial conclusions that lead to poor investment decisions. Skipping this meta-procedure means the AI operates without a quality-control framework for its own outputs.

**Situations covered**:
- Using AI tools to accelerate financial modeling and analysis while maintaining accuracy
- Establishing a validation protocol for AI-generated financial calculations
- Creating auditable AI-human collaboration records for financial work products

---

## §2 Procedure

**Steps**:
1. Structure the financial analysis request with precise context before engaging the AI tool. Define: (a) the company being analyzed (name, ticker, industry), (b) the specific analysis objective (valuation, credit analysis, competitive assessment, etc.), (c) the time frame (historical period and projection period), (d) the output format required (report, model, data table). Pre-gather the relevant financial data (10-K data, market data) — do not rely on the AI's training data for financial figures as they may be outdated.
2. Provide relevant data tables and financial statements directly in the prompt. Format data clearly: (a) use structured tables with labeled columns and rows, (b) specify units (millions, thousands, percentages), (c) include source and date of data. Example: "Here is Apple's income statement for FY2023-FY2025 (in $ millions, source: 10-K filings): [table]." Do not ask the AI to look up current data — provide it.
3. Request specific calculations with explicit formulas. Instead of "calculate the valuation," specify: "Calculate WACC using: Re = 3.8% + 1.15 x 5.5% = 10.13%, Rd = 4.2% after-tax at 21% = 3.32%, weights 85% equity / 15% debt. Then discount the following UFCF at WACC: [Year 1: $105B, Year 2: $112B, ...]." This reduces ambiguity and enables precise validation.
4. Validate AI outputs against manual checks: (a) Spot-check at least 3 individual calculations (e.g., verify one year's UFCF, verify the terminal value, verify the equity bridge). (b) Check that ratios and margins are within plausible ranges (compare to industry benchmarks from Damodaran). (c) Verify logical consistency: do revenue and cost trends move in defensible directions? Does the BS balance? (d) Flag any output where the AI's result differs from your manual calculation by more than 1%.
5. Iterate with follow-up questions to refine the analysis: (a) Challenge assumptions: "What if revenue growth is 3% instead of 5%?" (b) Request alternative methodologies: "Now value using comparable company analysis with these peers: [list]." (c) Ask for sensitivity: "Create a table varying WACC from 8% to 12% and terminal growth from 1% to 3%." (d) Request explanations: "Why does the terminal value represent 75% of enterprise value? Is this reasonable?"
6. Cross-reference AI insights with current market data: (a) Compare AI-generated valuation to current market price (Yahoo Finance). (b) Compare AI growth assumptions to analyst consensus estimates (Yahoo Finance Analysis tab). (c) Compare AI margin projections to recent quarterly results (10-Q). (d) Check that macroeconomic assumptions (risk-free rate, market risk premium) reflect current conditions, not historical averages.
7. Compile the AI-assisted analysis into a standardized report: (a) Executive Summary with key findings and recommendation. (b) Financial Overview (historical analysis). (c) Projection Assumptions and methodology. (d) Valuation Results with sensitivity analysis. (e) Risk Assessment. (f) Appendix with detailed calculations.
8. Document the AI-human collaboration process: for each section of the report, note: (a) which steps were AI-generated, (b) which steps were human-verified and how, (c) which data inputs were externally sourced vs. AI-generated, (d) any corrections made to AI outputs and the reason. This transparency is essential for audit trails and credibility.
9. **Mandatory human review gate**: No AI-generated financial analysis or investment recommendation produced by this procedure is final until reviewed and approved by the human decision-maker. The AI output is an input to human judgment, not a replacement for it. Document the human reviewer's name, date, and any modifications made to the AI-generated analysis before distribution.

**Output**: An AI-augmented financial analysis workflow containing: (a) structured prompt templates for common financial analysis tasks, (b) validation checklist for AI-generated financial calculations, (c) compiled analysis report with clear AI/human attribution, (d) quality gate criteria (when to accept, revise, or reject AI outputs), (e) documentation of the AI-human collaboration process.

**AI Execution Notes**: This is a meta-procedure about how to effectively use AI tools (including the executing AI itself) for financial analysis. The key principle is "AI as calculator and drafter, human as validator and decision-maker." Never trust AI-generated financial figures without verification against primary sources (SEC EDGAR, Yahoo Finance). AI is strongest at: structuring analyses, performing calculations from provided data, generating sensitivity tables, drafting narrative sections. AI is weakest at: accessing real-time market data, predicting future events, understanding company-specific qualitative factors. Always provide current data rather than asking the AI to recall it.

**Key Formulas**:
- Validation Threshold: flag if AI result differs from manual check by >1%
- Quality Gate: accept if 3+ spot-checks pass AND ratios within industry benchmarks
- AI Attribution: tag each output section as AI-generated, human-verified, or externally-sourced

---

## §3 Cortex Logging

```json
{
  "text": "F-071 AI-Assisted Financial Analysis — AI-augmented analysis for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-071",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "ai-assisted-analysis"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: Whenever AI tools are used for financial calculations or analysis — applies as a quality overlay to all other procedures
**PREREQUISITES**: All L1-L4 procedures as applicable (this is a meta-procedure that wraps around others)
**FEEDS INTO**: Any procedure that uses AI-generated financial outputs, Final Investment Memo for Pafi

**VK**: `✅ [F-071] AI-Assisted Financial Analysis | {company_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| All L1-L4 | Requires | This meta-procedure wraps around any financial analysis task |
| F-039 | Requires | DCF is a common AI-assisted calculation requiring validation |
| F-069 | Requires | Sensitivity tables are a natural AI-generated output |
| F-062 | Feeds into | Validated AI outputs feed the investment research process |

---
---

# F-031: Capital Budgeting Full Model Build

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 4
**Complexity**: ADVANCED
**Rule**: TRAIN-F-031

---

## §1 Problem

Capital budgeting determines whether a specific project or investment generates sufficient returns to justify the capital deployed. Without a rigorous model that calculates NPV, IRR, payback period, and profitability index from detailed cash flow projections, the AI cannot advise on whether to approve or reject a capital allocation decision. Skipping this means investment decisions are made without quantifying the expected return or comparing against the company's cost of capital.

**Situations covered**:
- Evaluating whether a specific project (factory, product launch, acquisition) should be approved
- Ranking mutually exclusive projects or allocating a limited capital budget across multiple options
- Performing investment committee-style analysis with sensitivity on key project assumptions

---

## §2 Procedure

**Steps**:
1. Identify and quantify all project cash flows: (a) Initial Investment (Year 0): capital expenditure for equipment/facility, installation costs, initial working capital requirement (inventory buildup, AR to fund), any opportunity costs (e.g., land that could be sold instead). (b) Annual Operating Cash Flows (Years 1-N): Incremental Revenue - Incremental Operating Costs - Incremental Taxes + Depreciation Tax Shield. Use the formula: OCF = (Revenue - Costs) x (1 - T) + Depreciation x T, where T = marginal tax rate. (c) Terminal/Salvage Value: estimated resale value of equipment at project end, net of taxes on gain: After-tax Salvage = Salvage Value - T x (Salvage Value - Book Value). Plus recovery of working capital invested.
2. Calculate Net Present Value (NPV) at the project's WACC (from F-035) or project-specific discount rate: NPV = -Initial Investment + sum of [OCF_t / (1 + WACC)^t] for t = 1 to N + Terminal Value / (1 + WACC)^N. Decision rule: Accept if NPV > 0. This is the primary decision metric. Cross-reference methodology with F-029.
3. Calculate Internal Rate of Return (IRR): solve for r in the equation: 0 = -Initial Investment + sum of [OCF_t / (1 + r)^t] + Terminal Value / (1 + r)^N. Use iterative solver. Decision rule: Accept if IRR > WACC (hurdle rate). Flag if cash flows have multiple sign changes (potential multiple IRRs — see F-030). Cross-reference methodology with F-030.
4. Calculate Payback Period: find the year t at which cumulative undiscounted cash flows first turn positive. Payback = Last year with negative cumulative CF + (Remaining amount / Next year's CF). Example: if cumulative CF at year 3 = -$50K and year 4 CF = $200K, payback = 3 + 50/200 = 3.25 years. Decision rule: Accept if payback < management's maximum threshold (typically 3-5 years). Limitation: ignores time value of money and cash flows beyond the payback period.
5. Calculate Discounted Payback Period: same as step 4 but using discounted cash flows (PV of each year's CF). This addresses the time-value-of-money limitation but still ignores post-payback cash flows.
6. Calculate Profitability Index (PI): PI = PV of Future Cash Flows / Initial Investment = (NPV + Initial Investment) / Initial Investment. Decision rule: Accept if PI > 1.0 (equivalent to NPV > 0). A PI of 1.15 means the project returns $1.15 in present value for every $1.00 invested. PI is especially useful for ranking projects under capital rationing.
7. Compare the project against the hurdle rate and alternative investments: (a) If the project is standalone: accept if NPV > 0, IRR > WACC, PI > 1.0. (b) If comparing mutually exclusive projects: rank by NPV (not IRR — due to scale and reinvestment rate differences). (c) If capital is rationed: rank all acceptable projects by PI (descending) and select from the top until the capital budget is exhausted. This maximizes NPV per dollar invested.
8. Perform sensitivity analysis on key assumptions (using F-069 methodology): (a) Vary revenue estimates +/- 20%. (b) Vary operating cost estimates +/- 15%. (c) Vary the discount rate +/- 2%. (d) Vary project life +/- 1-2 years. (e) Vary salvage value +/- 50%. Build a tornado chart showing which variable has the greatest impact on NPV.
9. Present the investment committee recommendation in a structured format: (a) Project description and strategic rationale. (b) Summary of financial metrics: NPV, IRR, payback, discounted payback, PI. (c) Sensitivity analysis results and key risks. (d) Clear recommendation: Approve/Reject/Request More Information. (e) Required approvals and next steps if approved.

**Output**: A capital budgeting model containing: (a) detailed cash flow projections for the project life, (b) NPV at WACC, (c) IRR, (d) payback and discounted payback periods, (e) profitability index, (f) sensitivity analysis with tornado chart, (g) project ranking table (if multiple projects), (h) investment committee recommendation memo.

**AI Execution Notes**: Cash flow estimates should come from the project sponsor's business case or from comparable historical projects. Discount rate from F-035 (WACC) for average-risk projects; adjust upward for higher-risk projects. Tax rate: use the company's marginal tax rate (not effective rate) for incremental project analysis. Depreciation method: use the tax depreciation schedule (MACRS in the US) for tax shield calculations, not accounting depreciation. For real estate or long-lived infrastructure projects, extend the projection period to 15-30 years. This procedure synthesizes F-029 (NPV), F-030 (IRR), and F-069 (sensitivity) into a complete capital budgeting workflow.

**Tools prerequisites**: F-080 (TVM Functions) for NPV, IRR, PV calculations in steps 2-6. F-070 (Model Mapping) for capital budgeting model sheet architecture. F-086 (Goal Seek) for finding breakeven assumptions in step 8.

**Key Formulas**:
- OCF = (Revenue - Costs) x (1 - T) + Depreciation x T
- NPV = -Initial Investment + sum of [OCF_t / (1 + WACC)^t] + Terminal Value / (1 + WACC)^N
- PI = (NPV + Initial Investment) / Initial Investment

---

## §3 Cortex Logging

```json
{
  "text": "F-031 Capital Budgeting Full Model Build — capital budgeting for {company_name}/{project_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "financial_model",
    "procedure": "F-031",
    "domain": "finance",
    "level": 4,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-4-modeling", "capital-budgeting"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 4
**WHEN**: When evaluating a specific project or capital allocation decision requiring NPV/IRR analysis
**PREREQUISITES**: F-029 (NPV methodology), F-030 (IRR methodology), F-035 (WACC for discount rate)
**FEEDS INTO**: F-062 (Investment Research — project-level evaluation), Final Investment Memo for Pafi

**VK**: `✅ [F-031] Capital Budgeting Full Model Build | {company_name}/{project_name} | Level 4 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-029 | Requires | NPV calculation methodology and decision rules |
| F-030 | Requires | IRR calculation methodology and multiple-IRR handling |
| F-035 | Requires | WACC provides the hurdle rate / discount rate |
| F-069 | Requires | Sensitivity analysis framework for step 8 |
| F-062 | Feeds into | Project-level analysis contributes to overall investment decision |

---
---

# F-062: 8-Step Investment Research Process

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: ADVANCED
**Rule**: TRAIN-F-062

---

## §1 Problem

The 8-Step Investment Research Process is the master orchestration procedure that synthesizes all Level 1-4 analyses into a coherent investment thesis. Without it, individual analyses (financial statements, ratios, valuations, sensitivity) remain disconnected fragments that cannot support a defensible buy/hold/sell recommendation. Skipping this means the AI produces partial analyses without a unifying investment thesis, margin-of-safety calculation, or actionable recommendation for Pafi.

**Situations covered**:
- Conducting a complete end-to-end investment analysis from screening to recommendation
- Synthesizing multiple valuation methods into a single "football field" view with price targets
- Producing a final buy/hold/sell recommendation with margin of safety and risk assessment

---

## §2 Procedure

**Steps**:
1. **Screen the investment universe**: Define screening criteria based on investment mandate: (a) Market cap range (e.g., >$1B for large-cap, $300M-$1B for mid-cap). (b) Sector/industry focus. (c) Valuation filters (P/E < 20, EV/EBITDA < 12, P/B < 3). (d) Quality filters (ROE > 15%, debt/equity < 1.0, positive FCF). (e) Growth filters (revenue growth > 10%, EPS growth > 12%). Apply filters using Yahoo Finance screener, Finviz, or SEC EDGAR full-text search. Generate a shortlist of 10-20 candidates, then narrow to 3-5 for deep analysis.
2. **Read the latest 10-K and recent 10-Qs**: For each shortlisted company, obtain filings from SEC EDGAR. In the 10-K, focus on: (a) Item 1 — Business description (understand what the company does). (b) Item 1A — Risk Factors (understand the key risks). (c) Item 7 — MD&A (understand management's view of performance and outlook). (d) Item 8 — Financial Statements and Notes (the data for L1+L2 analysis). For 10-Qs, focus on quarterly trends and any material changes since the 10-K. Read the most recent earnings call transcript for forward-looking commentary.
3. **Analyze competitive position**: (a) Apply Porter's Five Forces: threat of new entrants (barriers to entry), bargaining power of suppliers, bargaining power of buyers, threat of substitutes, competitive rivalry. Score each force (Low/Medium/High). (b) Identify the company's economic moat: brand (consumer loyalty), cost advantage (scale, proprietary process), switching costs (customer lock-in), network effects (value increases with users), intangible assets (patents, licenses, regulatory approvals). Classify moat as Wide/Narrow/None. Source: 10-K competitive landscape discussion, industry reports, Morningstar moat ratings.
4. **Perform financial statement analysis using L1+L2 procedures**: (a) Horizontal analysis: revenue, EBITDA, net income, FCF trends over 5 years. (b) Vertical analysis: common-size income statement and balance sheet. (c) Ratio analysis: profitability (gross margin, operating margin, net margin, ROE, ROIC), efficiency (asset turnover, DSO, DIO, DPO), leverage (D/E, interest coverage, debt/EBITDA), liquidity (current ratio, quick ratio). (d) DuPont decomposition of ROE. (e) Quality of earnings assessment: compare operating CF to net income, check for aggressive accounting.
5. **Value the company using 3+ methods from L3 procedures**: (a) DCF Valuation (F-039): intrinsic value based on projected free cash flows. (b) Comparable Company Analysis (F-041): relative value based on peer trading multiples. (c) At least one additional method: P/E valuation (F-044), Residual Income (F-040), or Deal Comps (F-042). (d) Triangulate: create a "football field" chart showing the valuation range from each method. Identify the consensus valuation zone where multiple methods overlap.
6. **Assess risk and margin of safety**: (a) Quantify downside risk using bear case scenario from sensitivity analysis (F-069). (b) Calculate margin of safety = (Intrinsic Value - Market Price) / Intrinsic Value. Per Benjamin Graham (The Intelligent Investor), require at least 25-35% margin of safety for common stocks. (c) Identify the top 3 risks that could destroy the investment thesis (competitive disruption, regulatory change, management failure, balance sheet risk). (d) Assess the probability and impact of each risk.
7. **Check market signals**: (a) Insider activity: net insider buying over the past 6-12 months from SEC Form 4 filings (EDGAR) or Yahoo Finance Insider Transactions. Net buying is a bullish signal; heavy selling may be a red flag. (b) Institutional ownership: percentage of shares held by institutional investors (from 13-F filings). Rising institutional ownership suggests growing interest; falling may indicate concern. (c) Short interest: shares sold short as % of float (from Yahoo Finance Key Statistics). Short interest > 10% may indicate significant bearish sentiment; very high short interest (>20%) could signal either valid concerns or potential short squeeze.
8. **Write the investment thesis**: (a) Bull Case: the optimistic scenario with key catalysts (new product, margin expansion, market share gains) and estimated upside. (b) Bear Case: the pessimistic scenario with key risks and estimated downside. (c) Base Case: the most likely outcome with probability-weighted expected return. (d) Final recommendation: Buy/Hold/Sell with a 12-month price target and the key assumption that would invalidate the thesis. (e) Position sizing guidance: allocate based on conviction level and risk — higher margin of safety = larger position.

**Output**: A complete investment research report containing: (a) screening criteria and shortlist rationale, (b) company overview from 10-K, (c) competitive analysis (Porter's 5 Forces + moat assessment), (d) financial statement analysis dashboard, (e) multi-method valuation with football field chart, (f) risk assessment with margin of safety calculation, (g) market signal summary (insider, institutional, short interest), (h) investment thesis with bull/base/bear cases and price targets.

**AI Execution Notes**: This is the master investment research procedure that orchestrates L1-L4 procedures into a cohesive workflow. Pull 10-K/10-Q from SEC EDGAR. Financial data from Yahoo Finance. Insider transactions from SEC Form 4 (EDGAR) or Yahoo Finance. Short interest from Yahoo Finance Key Statistics. Institutional holdings from SEC 13-F filings. Analyst consensus from Yahoo Finance Analysis tab. For the moat assessment, Morningstar provides economic moat ratings for many companies. The margin of safety threshold (25-35%) comes from Graham's The Intelligent Investor; more conservative investors (per Security Analysis) may require 40-50%.

**Tools prerequisites**: F-084 (Portfolio Dashboard) for screening dashboard in step 1 and market signal monitoring in step 7. F-083 (Dashboard) for financial analysis dashboard in step 4. Google Sheets: use GOOGLEFINANCE for live screening and signal monitoring.

**Key Formulas**:
- Margin of Safety = (Intrinsic Value - Market Price) / Intrinsic Value
- Graham minimum: 25-35% margin of safety for common stocks
- Football Field = valuation range from DCF, Comps, and Deal Comps overlaid

---

## §3 Cortex Logging

```json
{
  "text": "F-062 8-Step Investment Research — full research for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "F-062",
    "domain": "finance",
    "level": 5,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-5-decision", "investment-research"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5
**WHEN**: When conducting a full investment analysis to produce a buy/hold/sell recommendation
**PREREQUISITES**: ALL Level 1-4 procedures (this is the master orchestrator): F-013 (10-K), F-015 (Trends), F-005 (Revenue), F-065 (3-Statement), F-039 (DCF), F-041 (Comps), F-035 (WACC), F-069 (Sensitivity), C-075 (Scam Detection as pre-filter)
**FEEDS INTO**: Final Investment Memo for Pafi

**VK**: `✅ [F-062] 8-Step Investment Research | {company_name} | Level 5 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-013 | Requires | 10-K reading provides the foundational company understanding |
| F-005 | Requires | Revenue forecast drives the financial model |
| F-065 | Requires | 3-statement model provides projected financials |
| F-039 | Requires | DCF valuation is the primary intrinsic value method |
| F-041 | Requires | Trading comps provide relative valuation benchmarks |
| F-069 | Requires | Sensitivity analysis quantifies downside risk |
| F-090 | Requires | Risk register provides structured risk assessment |
| C-075 | Requires | Scam detection must clear before committing to full analysis |
| Final Memo | Feeds into | Final Investment Memo for Pafi |

---
---

# F-094: Venture Capital / Private Investment Evaluation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: ADVANCED
**Rule**: TRAIN-F-094

---

## §1 Problem

Private and early-stage investments cannot be evaluated using traditional public-company methods because they lack audited financials, public market pricing, and analyst coverage. Without a structured VC evaluation framework that weights team, market, product, unit economics, traction, and deal terms, the AI cannot produce a defensible GO/NO-GO recommendation for private investments. Skipping this means applying public-company valuation tools to early-stage companies where they do not apply, leading to unreliable conclusions.

**Situations covered**:
- Evaluating a seed, Series A, or later-stage private investment opportunity
- Assessing founder team quality, product-market fit, and unit economics for startups
- Benchmarking deal terms and expected returns against comparable private exits

---

## §2 Procedure

**Steps**:
1. **Assess the founding team**: (a) Experience: years in the industry, relevant domain expertise, prior startup experience. (b) Track record: previous exits (IPO, acquisition), revenue milestones achieved at prior companies. (c) Completeness: does the team cover the critical functions (CEO/vision, CTO/product, sales/GTM)? (d) Commitment: full-time founders, vesting schedules in place. (e) References: check with prior investors, colleagues, employees. Score: Strong (proven founders with exits) / Adequate (experienced but unproven) / Weak (first-time founders in unfamiliar domain). Weight: 25% of total evaluation.
2. **Evaluate market opportunity**: (a) Total Addressable Market (TAM): size the market using top-down (industry reports) and bottom-up (# of customers x ACV) approaches. (b) Growth rate: is the market growing >15% annually? (c) Market timing: is the market ready for this product now? (Premature markets kill startups as often as bad products.) (d) Market structure: fragmented (easier entry) vs. concentrated (harder to displace incumbents). (e) Regulatory environment: enablers or barriers? Score: Large (TAM >$10B, >20% CAGR) / Medium ($1-10B TAM, 10-20% CAGR) / Small (<$1B TAM or <10% CAGR). Weight: 20%.
3. **Analyze product/technology**: (a) Defensibility: is there proprietary technology, unique data, or technical moat? (b) IP protection: patents filed/granted, trade secrets, regulatory barriers to replication. (c) Product-market fit signals: user engagement metrics, NPS score, organic growth, customer testimonials. (d) Technical risk: how far is the product from a scalable, production-ready state? (e) Differentiation: can a competitor with 10x resources replicate this in 12 months? Score: High (strong IP, proven PMF) / Medium (emerging PMF, some defensibility) / Low (easily replicable, unproven). Weight: 20%.
4. **Review business model and unit economics**: (a) LTV (Lifetime Value) = ARPU x Gross Margin x Average Customer Lifespan (or ARPU x Gross Margin / Churn Rate for subscription). (b) CAC (Customer Acquisition Cost) = Total S&M Spend / New Customers Acquired. (c) LTV/CAC ratio: >3.0x is healthy, 1.0-3.0x is concerning, <1.0x is unsustainable. (d) CAC Payback Period = CAC / (Monthly ARPU x Gross Margin). Healthy: <18 months. (e) Gross margin: >60% for software, >40% for marketplace, >20% for hardware. (f) Contribution margin: positive and improving. (g) Path to profitability: when will the company reach cash flow breakeven?
5. **Assess traction**: (a) Revenue growth: >3x YoY for seed/Series A, >2x for Series B+. (b) User/customer growth: trajectory and acceleration/deceleration. (c) Retention metrics: monthly/annual retention rate, net revenue retention (NRR). NRR >120% for SaaS is exceptional. (d) Pipeline: contracted or highly probable future revenue. (e) Milestones: key product, partnership, or regulatory milestones achieved. Plot the growth curve and assess whether it suggests exponential, linear, or decelerating trajectory.
6. **Evaluate deal terms**: (a) Pre-money valuation: compare to revenue multiple (EV/ARR for SaaS, EV/Revenue for others) against comparable private rounds (use Pitchbook, Crunchbase, or recent funding announcements). (b) Round structure: equity vs. convertible note vs. SAFE. (c) Dilution: what percentage of the company does this round represent? What is the cumulative dilution across all rounds? (d) Liquidation preferences: 1x non-participating (standard) vs. participating (investor-friendly) vs. >1x (aggressive). (e) Anti-dilution provisions: weighted average (standard) vs. full ratchet (aggressive). (f) Board seats and control provisions. (g) Pro rata rights for follow-on investment.
7. **Benchmark against comparable exits**: (a) Identify 5-10 comparable exits (IPOs and acquisitions) in the same sector from the past 5 years. (b) Record exit valuation, time from founding to exit, revenue at exit, revenue multiple at exit. (c) Calculate the implied return multiple: if the company exits at a comparable multiple, what is the return on this investment? Return Multiple = (Exit Valuation x Ownership %) / Investment Amount. (d) Calculate implied IRR: IRR = (Return Multiple)^(1/Holding Period) - 1. Typical VC target: >3x return in 5-7 years (>25% IRR). (e) Assess realistic probability of each exit outcome (IPO, acquisition, failure).
8. **Produce GO/NO-GO recommendation**: (a) Aggregate the scores from steps 1-7 with the specified weights: Team (25%), Market (20%), Product (20%), Unit Economics (15%), Traction (10%), Deal Terms (10%). (b) Calculate a weighted composite score on a 1-5 scale. (c) Decision framework: Score >4.0 = Strong GO, 3.0-4.0 = Conditional GO (address specific concerns), 2.0-3.0 = Likely NO-GO (requires exceptional offsetting factor), <2.0 = Hard NO-GO. (d) State the expected return multiple and the key assumption that must hold true for the investment to succeed. (e) Identify the single biggest risk and whether it is mitigable.

**Output**: A VC evaluation memo containing: (a) team assessment with score, (b) market sizing (TAM/SAM/SOM) with growth analysis, (c) product/technology defensibility assessment, (d) unit economics analysis (LTV, CAC, LTV/CAC, payback period, margins), (e) traction scorecard, (f) deal terms analysis and comparison to market standards, (g) comparable exit benchmarking with implied return multiple and IRR, (h) weighted composite score and GO/NO-GO recommendation with expected return.

**AI Execution Notes**: Private company data is limited; rely on pitch decks, data rooms, and founder-provided metrics. Validate claims by cross-referencing with third-party sources where possible (App Annie for app data, SimilarWeb for web traffic, LinkedIn for headcount). Comparable fundraising data from Crunchbase or Pitchbook. Exit data from Pitchbook, CB Insights, or news reports. This procedure does not directly use the public company valuation procedures (F-039, F-041) but borrows frameworks from them (multiples-based valuation, DCF for later-stage companies approaching profitability). For deep-tech or biotech investments, weight Product/Technology at 30% and reduce Traction to 5%.

**Key Formulas**:
- LTV = ARPU x Gross Margin x Average Customer Lifespan
- LTV/CAC Ratio: >3.0x healthy, <1.0x unsustainable
- Return Multiple = (Exit Valuation x Ownership %) / Investment Amount

---

## §3 Cortex Logging

```json
{
  "text": "F-094 VC / Private Investment Evaluation — VC eval for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "F-094",
    "domain": "finance",
    "level": 5,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-5-decision", "vc-evaluation"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5
**WHEN**: Before committing capital to any private or early-stage investment
**PREREQUISITES**: F-005 (Revenue Forecast methodology, adapted for private data), F-029 (NPV for later-stage), F-090 (Risk Register), C-075 (Scam Detection as pre-filter)
**FEEDS INTO**: Final Investment Memo for Pafi

**VK**: `✅ [F-094] VC / Private Investment Evaluation | {company_name} | Level 5 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-005 | Requires | Revenue forecasting methodology adapted for limited private data |
| F-029 | Requires | NPV for later-stage companies approaching profitability |
| F-090 | Requires | Risk register provides structured risk assessment |
| C-075 | Requires | Scam detection must clear before proceeding with evaluation |
| Final Memo | Feeds into | Final Investment Memo for Pafi |

---
---

# F-096: Investment Banking M&A Process

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: ADVANCED
**Rule**: TRAIN-F-096

---

## §1 Problem

Mergers and acquisitions require a multi-layered analysis that goes beyond standalone valuation: synergy estimation, control premium calculation, deal structure optimization, accretion/dilution modeling, and regulatory risk assessment. Without this structured M&A framework, the AI cannot evaluate whether a proposed deal creates or destroys value for the acquirer, or whether the offer price is fair for the seller. Skipping this means M&A decisions are made without quantifying synergies, testing deal accretion, or assessing antitrust risk.

**Situations covered**:
- Evaluating a proposed acquisition from the buyer's perspective (is this deal accretive?)
- Assessing whether an offer price is fair from the seller's perspective (fairness opinion)
- Analyzing deal structure options (cash vs. stock vs. mixed) and their implications

---

## §2 Procedure

**Steps**:
1. **Determine transaction rationale**: (a) Strategic buyer: acquiring for revenue synergies (cross-selling, new markets), cost synergies (eliminate duplicate functions, achieve scale), technology/IP acquisition, or talent acquisition. (b) Financial buyer (PE): acquiring for financial engineering (leverage, operational improvement, multiple expansion). (c) Seller motivation: strategic realignment, need for capital, founder exit, competitive pressure. Document the specific rationale as it drives valuation methodology and synergy assumptions.
2. **Identify buyer and seller motivations and dynamics**: (a) Is this a hostile or friendly acquisition? (b) Are there competing bidders (auction process)? Multiple bidders typically drive price up 10-20%. (c) Is the seller distressed? Distressed sales typically result in 20-40% discount to fair value. (d) Regulatory landscape: are there antitrust concerns? Identify the relevant regulatory bodies (FTC/DOJ in the US, EU Commission in Europe). (e) Map the negotiation dynamics: who has leverage?
3. **Select and apply valuation methods**: (a) DCF Valuation (F-039): calculate standalone enterprise value of the target using projected UFCF discounted at the target's WACC. (b) Comparable Company Analysis (F-041): value the target using trading multiples from public peers (EV/EBITDA, EV/Revenue, P/E). (c) Precedent Transaction Analysis (F-042): value the target using multiples from comparable M&A deals (these embed a control premium). All three methods are standard in M&A; present a range from each.
4. **Calculate standalone value**: Using the three methods from step 3, establish the target's standalone value range. Present as a football field chart. The standalone value is the floor — the buyer must pay at least this plus a premium to incentivize shareholders to sell. Calculate control premium needed: Control Premium = (Offer Price - Unaffected Share Price) / Unaffected Share Price. Historical average control premiums: 20-40% for strategic acquirers, 15-30% for financial buyers.
5. **Estimate synergies in detail**: (a) Revenue synergies: cross-selling opportunities, new market access, combined product offerings. Quantify as incremental revenue over 3-5 years. Apply a realization probability (typically 30-50% for revenue synergies, as they are harder to achieve). (b) Cost synergies: headcount reductions (overlapping corporate functions), facility consolidation, procurement savings, technology platform consolidation. Quantify as annual cost savings at full run-rate (typically 2-3 years to fully realize). Apply a realization probability (typically 60-80% for cost synergies). (c) Total Synergy Value = PV of annual synergy savings (net of one-time integration costs) discounted at WACC. (d) Allocation: what portion of synergies is the buyer willing to share with the seller via the premium? Typically 25-50%.
6. **Determine the offer price range**: (a) Floor = standalone value (from step 4). (b) Ceiling = standalone value + full synergy value (from step 5). (c) Expected offer = standalone value + (% of synergies shared with seller). (d) Reasonableness check: calculate the implied control premium and compare to precedent transactions. If the premium is >50%, the buyer may be overpaying; if <15%, the offer may be rejected.
7. **Analyze deal structure**: (a) Cash deal: certainty for seller, financed by buyer's cash reserves and/or debt. Requires buyer to have borrowing capacity. (b) Stock deal: aligns buyer and seller interests post-close; risk-sharing. Calculate the exchange ratio: Exchange Ratio = Offer Price per Target Share / Buyer's Share Price. (c) Mixed (cash + stock): most common structure. (d) Earnouts: contingent consideration tied to post-close performance targets — used when there is disagreement on value. (e) Financing: model the debt required (LBO-style analysis for financial buyers) and assess whether the combined entity can service the debt.
8. **Model accretion/dilution for the acquirer**: (a) Calculate the acquirer's standalone EPS. (b) Estimate the combined entity's net income: Acquirer NI + Target NI + After-tax Cost Synergies - After-tax Interest on Acquisition Debt - Amortization of Intangibles (purchase price allocation). (c) Calculate combined diluted shares outstanding (acquirer shares + new shares issued if stock deal). (d) Combined EPS = Combined Net Income / Combined Diluted Shares. (e) Accretion/Dilution = (Combined EPS - Acquirer Standalone EPS) / Acquirer Standalone EPS. If positive, the deal is accretive (increases acquirer's EPS); if negative, it is dilutive. A dilutive deal is not automatically bad — it may be accretive by year 2-3 as synergies are realized. Flag if dilutive beyond year 2.
9. **Assess regulatory and antitrust risk**: (a) Calculate combined market share in each relevant product market. (b) Calculate HHI (Herfindahl-Hirschman Index) pre- and post-merger: HHI = sum of (market share %)^2 for all competitors. Post-merger HHI > 2,500 and change in HHI > 200 triggers a "presumption of illegality" under US DOJ Horizontal Merger Guidelines. (c) Identify potential divestitures required to obtain regulatory approval. (d) Estimate timeline for regulatory review (typically 3-12 months). (e) Assess the probability of regulatory challenge (low/medium/high).
10. **Produce the M&A fairness analysis**: (a) Summary of standalone valuation range. (b) Synergy analysis and combined entity value. (c) Recommended offer price range with implied control premium. (d) Deal structure recommendation (cash/stock/mixed) with accretion/dilution analysis. (e) Regulatory risk assessment. (f) Timeline and key milestones. (g) GO/NO-GO recommendation from both buyer and seller perspectives.

**Output**: An M&A analysis report containing: (a) transaction rationale and strategic context, (b) standalone valuation of the target (DCF + comps + deal comps football field), (c) synergy analysis with revenue and cost synergies quantified, (d) offer price range with control premium analysis, (e) deal structure analysis (cash vs. stock vs. mixed), (f) accretion/dilution analysis for the acquirer, (g) regulatory/antitrust risk assessment with HHI analysis, (h) fairness opinion summary and recommendation.

**AI Execution Notes**: Target and acquirer financials from SEC EDGAR 10-K/10-Q. Market data from Yahoo Finance. Precedent transactions from SEC EDGAR merger proxy statements (DEF 14A, S-4 filings). This procedure integrates F-039 (DCF), F-041 (trading comps), and F-042 (deal comps) for the valuation components. For accretion/dilution modeling, the acquirer's EPS and share count come from their most recent filings and Yahoo Finance. HHI calculation requires market share data, which may come from industry reports or the companies' own disclosures. Control premium data can be sourced from Mergerstat or academic studies.

**Key Formulas**:
- Control Premium = (Offer Price - Unaffected Share Price) / Unaffected Share Price
- Accretion/Dilution = (Combined EPS - Acquirer Standalone EPS) / Acquirer Standalone EPS
- HHI = sum of (market share %)^2 for all competitors

---

## §3 Cortex Logging

```json
{
  "text": "F-096 Investment Banking M&A Process — M&A analysis for {target_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "F-096",
    "domain": "finance",
    "level": 5,
    "company": "{target_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-5-decision", "ma-process"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5
**WHEN**: When evaluating any merger or acquisition opportunity, from either buyer or seller perspective
**PREREQUISITES**: F-039 (DCF Valuation), F-041 (Comparable Company Analysis), F-042 (Precedent Transaction / Deal Comps), F-069 (Sensitivity Analysis), F-090 (Risk Register)
**FEEDS INTO**: Final Investment Memo for Pafi

**VK**: `✅ [F-096] Investment Banking M&A Process | {target_name} | Level 5 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-039 | Requires | DCF provides standalone intrinsic value of the target |
| F-041 | Requires | Trading comps provide relative valuation benchmarks |
| F-042 | Requires | Deal comps embed control premiums from prior transactions |
| F-069 | Requires | Sensitivity analysis stress-tests synergy and valuation assumptions |
| F-090 | Requires | Risk register captures regulatory and integration risks |
| Final Memo | Feeds into | Final Investment Memo for Pafi |

---
---

# F-047: S-1 / IPO Prospectus Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: ADVANCED
**Rule**: TRAIN-F-047

---

## §1 Problem

An S-1 filing is the single most important document for evaluating a company approaching IPO, yet it contains hundreds of pages of legal, financial, and business disclosures that must be systematically analyzed. Without a structured framework for reading the S-1 — covering business model, risk factors, revenue quality, unit economics, competitive landscape, management, governance, and valuation — the AI produces a superficial assessment that misses critical red flags. Skipping this means investing in an IPO without understanding the company's genuine vulnerabilities, governance risks, or whether the pricing is justified.

**Situations covered**:
- Conducting a comprehensive analysis of an S-1 filing to decide whether to subscribe to an IPO
- Evaluating management quality, corporate governance, and insider alignment before an IPO
- Comparing IPO valuation to public peers to determine if the offering is fairly priced

---

## §2 Procedure

**Steps**:
1. **Read Business Overview and Risk Factors**: Obtain the S-1 from SEC EDGAR. Read Item 1 (Business) to understand: what the company does, its products/services, target customers, competitive advantages, and go-to-market strategy. Read Risk Factors section in full — these are legally required disclosures and often reveal the company's genuine vulnerabilities. Categorize risks as: business model risk, competitive risk, regulatory risk, financial risk, key person risk, and technology risk. Identify the top 5 risks by potential impact.
2. **Analyze revenue model and growth trajectory**: (a) Identify the revenue model type (per F-046 step 2). (b) Calculate historical revenue growth rates (YoY for each disclosed period). (c) Assess revenue quality: recurring vs. non-recurring, customer concentration (top 10 customers as % of revenue — disclosed in the S-1 risk factors), geographic concentration. (d) Calculate key unit economics if disclosed: ARPU, net retention rate, gross margin per customer cohort. (e) Flag if revenue growth is decelerating — this is common pre-IPO and must be modeled.
3. **Examine unit economics and path to profitability**: (a) Gross margin trend: improving (good) or declining (bad). (b) Operating expenses as % of revenue: are they declining (operating leverage) or increasing (burning cash faster)? (c) Net loss trajectory: is the company losing less per dollar of revenue over time? (d) Cash burn rate = quarterly operating cash flow (negative). (e) Calculate runway: Cash on Hand / Quarterly Cash Burn = quarters until cash runs out. The IPO itself is often a capital raise to extend the runway. (f) Estimate the breakeven revenue level based on the current cost structure.
4. **Review competitive landscape**: (a) Read the Competition section in the S-1. (b) Map the competitive landscape: direct competitors, indirect competitors, potential future competitors (large tech platforms that could enter). (c) Assess the company's differentiation and sustainable competitive advantage. (d) Check market share claims against independent sources. (e) Evaluate barriers to entry: if barriers are low, the company's position is fragile.
5. **Analyze Use of Proceeds**: (a) Read the Use of Proceeds section. (b) Categorize: general corporate purposes (vague — acceptable but uninformative), debt repayment (may indicate over-leverage), R&D/product development (growth investment), sales and marketing (growth investment), acquisitions (growth via M&A). (c) Flag if a significant portion goes to existing shareholders (secondary offering — the company does not receive the funds). (d) Calculate primary vs. secondary split: primary shares = new money to the company; secondary shares = existing shareholders selling.
6. **Evaluate management team and compensation**: (a) Review the executive biographies in the S-1. Assess: industry experience, previous startup/IPO experience, tenure at the company. (b) Read the Executive Compensation section: base salary, bonus, equity grants. (c) Check for related party transactions (red flag if excessive). (d) Assess alignment of interests: how much stock do the founders/executives own? Are there lockup provisions (typically 90-180 days post-IPO)? (e) Post-lockup selling by insiders is a potential supply overhang — estimate the number of shares that become eligible for sale.
7. **Assess corporate governance**: (a) Board composition: independent directors (should be majority), diversity, relevant expertise. (b) Dual-class share structure: does the founder retain super-voting shares? If so, public shareholders have limited governance influence — flag this. (c) Voting rights: what can public shareholders actually vote on? (d) Anti-takeover provisions: staggered board, poison pills, supermajority requirements. (e) Audit committee: independent, financially qualified members.
8. **Compare IPO valuation to public comps**: (a) Obtain the proposed IPO price range from the S-1/A or pricing amendment. (b) Calculate implied valuation multiples: Market Cap / Revenue (TTM), EV / Revenue (TTM and NTM), EV / Gross Profit, Price / ARR (for SaaS). (c) Select 5-10 publicly traded comparable companies (per F-041 methodology). (d) Compare the IPO's implied multiples to peer medians. (e) Calculate the premium or discount: IPO Premium = (IPO Multiple / Peer Median Multiple) - 1. A 10-20% discount to public peers is typical for IPO pricing (to allow for a first-day pop).
9. **Determine if the IPO is fairly priced and produce recommendation**: (a) Synthesize all findings: business quality, growth trajectory, unit economics, competitive position, governance, and valuation. (b) Classify the IPO: Category A (strong business + attractive valuation = Subscribe), Category B (strong business but full valuation = Watch, buy on pullback), Category C (weak fundamentals or overvalued = Pass). (c) If subscribing, determine appropriate position size based on conviction level and the expected allocation (retail investors often receive small allocations in hot IPOs). (d) Post-IPO monitoring plan: watch for first earnings report, lockup expiration, and insider selling patterns.

**Output**: An IPO analysis report containing: (a) business model summary, (b) revenue analysis with growth trajectory, (c) unit economics and path to profitability assessment, (d) competitive landscape analysis, (e) use of proceeds breakdown, (f) management team and governance evaluation, (g) valuation comparison to public comps with implied premium/discount, (h) investment recommendation (Subscribe / Watch / Pass) with rationale.

**AI Execution Notes**: All data comes from the S-1 filing on SEC EDGAR — this is a self-contained analysis based on a single primary document. Comparable company data from Yahoo Finance (F-041 methodology). This procedure is closely related to F-046 (IPO Revenue Modeling) but is broader — F-046 focuses on building the revenue model, while F-047 covers the full qualitative and quantitative IPO assessment. Run F-046 first for the revenue model, then F-047 for the comprehensive analysis.

**Key Formulas**:
- Cash Runway = Cash on Hand / Quarterly Cash Burn (quarters)
- IPO Premium = (IPO Multiple / Peer Median Multiple) - 1
- Primary vs. Secondary Split = Primary Shares / Total Shares Offered

---

## §3 Cortex Logging

```json
{
  "text": "F-047 S-1 / IPO Prospectus Analysis — IPO analysis for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "F-047",
    "domain": "finance",
    "level": 5,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-5-decision", "ipo-analysis"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5
**WHEN**: When a company files an S-1 and an IPO subscription decision is required
**PREREQUISITES**: F-046 (IPO Revenue Model — run first for revenue projections), F-005 (Revenue Forecast methodology), F-041 (Trading Comps for peer comparison), F-044 (P/E Valuation), C-075 (Scam Detection as pre-filter)
**FEEDS INTO**: Final Investment Memo for Pafi

**VK**: `✅ [F-047] S-1 / IPO Prospectus Analysis | {company_name} | Level 5 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-046 | Requires | IPO revenue model provides the quantitative projections |
| F-005 | Requires | Revenue forecast methodology adapted for S-1 data |
| F-041 | Requires | Trading comps for IPO valuation comparison |
| F-044 | Requires | P/E valuation for profitable IPO candidates |
| C-075 | Requires | Scam detection pre-filter before committing capital |
| Final Memo | Feeds into | Final Investment Memo for Pafi |

---
---

# F-090: Comprehensive Risk Register

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: ADVANCED
**Rule**: TRAIN-F-090

---

## §1 Problem

Investment decisions are only as good as their risk assessment. A comprehensive risk register systematically identifies, scores, and prioritizes all risks facing a company, from market and credit risk to operational, regulatory, and reputational threats. Without it, the AI identifies only the most obvious risks while missing tail risks that could destroy an investment thesis. Skipping this means the margin-of-safety calculation in F-062 lacks a structured risk foundation, and Pafi is exposed to unquantified downside.

**Situations covered**:
- Building a structured risk assessment for any company as part of the investment research process
- Identifying and scoring risks across 8 categories with probability x impact matrices
- Establishing monitoring triggers and escalation thresholds for ongoing risk management

---

## §2 Procedure

**Steps**:
1. **Define risk categories**: Establish the taxonomy for the risk assessment: (a) Market Risk: interest rate changes, currency fluctuations, commodity price volatility, equity market downturns, demand shifts. (b) Credit Risk: customer default, counterparty failure, concentration of receivables. (c) Operational Risk: supply chain disruption, IT system failure, key personnel departure, process failure, fraud. (d) Regulatory/Legal Risk: new regulations, compliance violations, litigation, tax law changes, sanctions. (e) Reputational Risk: PR crisis, product quality failure, data breach, ESG controversy. (f) Technology Risk: disruption by new technology, cybersecurity breach, tech debt, obsolescence. (g) Strategic Risk: competitive disruption, failed M&A integration, incorrect market assumptions. (h) Financial Risk: liquidity shortfall, covenant breach, refinancing risk, currency mismatch.
2. **Identify specific risks per category**: For each category, list 3-7 specific risk events relevant to the company. Source risk identification from: (a) 10-K Risk Factors section (SEC EDGAR). (b) Industry-specific risk databases and reports. (c) Historical incidents (company-specific and industry peers). (d) Management interviews or earnings call commentary. (e) Macro environment scan (interest rates, geopolitical, regulatory pipeline). Use the PESTLE framework (Political, Economic, Social, Technological, Legal, Environmental) as a supplementary lens to ensure completeness.
3. **Assess probability for each risk**: Rate on a 1-5 scale: 1 = Rare (<5% chance in next 12 months), 2 = Unlikely (5-20%), 3 = Possible (20-50%), 4 = Likely (50-80%), 5 = Almost Certain (>80%). Base the rating on: historical frequency, current conditions, leading indicators, and expert judgment. Document the rationale for each rating.
4. **Assess impact for each risk**: Rate on a 1-5 scale: 1 = Negligible (<1% impact on revenue or equity value), 2 = Minor (1-5% impact), 3 = Moderate (5-15% impact), 4 = Major (15-30% impact), 5 = Catastrophic (>30% impact or existential threat). Define impact in both financial terms (dollar impact on revenue, EBITDA, or equity value) and operational terms (disruption duration, customer loss, regulatory consequences).
5. **Calculate risk score and rank**: Risk Score = Probability x Impact (range: 1-25). Rank all risks by score in descending order. Categorize using thresholds: Critical (score 15-25), High (score 10-14), Medium (score 5-9), Low (score 1-4). The top 5 risks by score should receive the most detailed mitigation planning.
6. **Identify risk mitigation strategies for each critical and high-risk item**: (a) Avoid: eliminate the risk by not engaging in the activity. (b) Reduce: implement controls to lower probability or impact (insurance, hedging, diversification, redundancy). (c) Transfer: shift the risk to a third party (insurance, outsourcing, contracts, hedging instruments). (d) Accept: acknowledge the risk and set aside reserves or contingency plans. For each strategy, estimate the cost of mitigation and the expected reduction in risk score.
7. **Assign risk owners**: For each risk, assign a named individual or function responsible for: (a) monitoring the risk, (b) executing the mitigation strategy, (c) escalating if the risk materializes or worsens. Risk ownership ensures accountability — unowned risks are unmanaged risks.
8. **Determine residual risk after mitigation**: After applying mitigation strategies, re-assess probability and impact to calculate the residual risk score: Residual Score = Mitigated Probability x Mitigated Impact. Compare inherent risk score to residual risk score. The reduction represents the value of the mitigation strategy. If residual risk for any item remains Critical (>15), flag for executive attention and potential additional mitigation.
9. **Define monitoring triggers and KPIs**: For each risk, establish: (a) Leading indicators that signal the risk is increasing (e.g., customer churn rate >5% triggers credit risk alert, VIX >30 triggers market risk alert). (b) Monitoring frequency: Critical risks — weekly, High — monthly, Medium — quarterly, Low — annually. (c) Escalation thresholds: the specific metric level that triggers escalation to senior management or the board. (d) Review cadence: full risk register review quarterly; critical risks reviewed monthly.
10. **Produce risk heat map**: Create a visual matrix with Probability (1-5) on the Y-axis and Impact (1-5) on the X-axis. Plot each risk as a data point on the map. Color-code: Red (Critical, upper-right quadrant), Orange (High), Yellow (Medium), Green (Low, lower-left quadrant). The heat map provides an at-a-glance view of the risk profile for executive communication.

**Output**: A risk register containing: (a) comprehensive risk inventory organized by category, (b) probability and impact scores with rationale, (c) ranked risk list with risk scores, (d) mitigation strategies with cost estimates, (e) risk ownership assignments, (f) residual risk scores post-mitigation, (g) monitoring triggers and KPIs, (h) heat map visualization, (i) executive summary highlighting top 5 risks and recommended actions.

**AI Execution Notes**: The primary source for company-specific risks is the 10-K Risk Factors section (SEC EDGAR). Supplement with industry risk databases and macro indicators. For market risk quantification, use historical volatility data from Yahoo Finance. For credit risk, review AR aging and customer concentration from 10-K notes. ESG-related risks can be sourced from MSCI ESG or Sustainalytics (per F-049). This procedure is complementary to F-069 (Sensitivity Analysis) — F-069 quantifies the financial impact of key variable changes, while F-090 provides a broader qualitative-to-quantitative risk framework.

**Key Formulas**:
- Risk Score = Probability (1-5) x Impact (1-5), range 1-25
- Residual Risk Score = Mitigated Probability x Mitigated Impact
- Critical threshold: Risk Score >= 15

---

## §3 Cortex Logging

```json
{
  "text": "F-090 Comprehensive Risk Register — risk assessment for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "F-090",
    "domain": "finance",
    "level": 5,
    "company": "{company_name}",
    "decision": "{go/no-go/conditional}",
    "tags": ["business-evaluation", "level-5-decision", "risk-register"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5
**WHEN**: As part of any investment analysis, before finalizing the investment recommendation
**PREREQUISITES**: F-069 (Sensitivity Analysis — quantitative risk inputs), F-049 (ESG Analysis — if applicable), 10-K Risk Factors section
**FEEDS INTO**: F-062 (Investment Research — risk assessment step), F-094 (VC Eval — risk dimension), F-096 (M&A — regulatory and integration risk), Final Investment Memo for Pafi

**VK**: `✅ [F-090] Comprehensive Risk Register | {company_name} | Level 5 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-069 | Requires | Sensitivity analysis provides quantitative risk impact data |
| F-049 | Requires | ESG analysis feeds reputational and regulatory risk categories |
| F-013 | Requires | 10-K Risk Factors section is the primary risk source |
| F-062 | Feeds into | Risk register informs the investment research risk assessment |
| F-094 | Feeds into | Risk assessment is a dimension of VC evaluation |
| F-096 | Feeds into | Regulatory and integration risks feed M&A analysis |

---
---

# C-075: Investment Opportunity Evaluation (Scam Detection)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 5
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-C-075
**Origin**: Crypto domain procedure (C- prefix). Applied as a universal pre-filter in the Finance Business Evaluation Training Path because investment fraud screening is a prerequisite for all investment decisions regardless of asset class.

---

## §1 Problem

Before committing capital to any investment — especially alternatives, private placements, or crypto opportunities — the AI must first verify that the opportunity is legitimate. Scams, Ponzi schemes, and fraudulent offerings cause total loss of capital, which no amount of financial modeling can recover. Without a systematic scam detection procedure that checks registration, team identity, financial claim plausibility, and audit infrastructure, the AI may proceed to build a full financial model for a fraudulent entity. Skipping this means the entire Level 1-5 pipeline runs on a foundation of potentially fabricated data.

**Situations covered**:
- Pre-screening any investment opportunity before committing time or capital to full analysis
- Evaluating crypto/DeFi projects, private fund offerings, or unusual investment claims
- Detecting Ponzi scheme indicators, unregistered securities, and fabricated credentials

---

## §2 Procedure

**Steps**:
1. **Verify entity registration and regulatory status**: (a) For US securities: check SEC EDGAR for the entity's registration (search company name and CIK). (b) Check FINRA BrokerCheck (brokercheck.finra.org) for broker-dealer registration. (c) Check SEC Investment Adviser Registration (adviserinfo.sec.gov) for registered investment advisers. (d) For crypto/digital assets: check state money transmitter licenses. (e) For international entities: check the relevant national regulator (FCA in UK, BaFin in Germany, ASIC in Australia). (f) RED FLAG: if the entity claims to manage investments but is not registered with any regulatory body, this is a major warning sign.
2. **Check team identities and backgrounds**: (a) Verify key personnel on LinkedIn: look for complete profiles with verifiable employment history, education, and connections. (b) Cross-reference names against public records (state business registries, corporate filings). (c) Search for media coverage, conference appearances, or published work. (d) Check for any regulatory actions or legal proceedings against team members (SEC enforcement actions database, FINRA BrokerCheck disclosures, court records). (e) RED FLAGS: anonymous or pseudonymous team, fabricated credentials, stock photos used as team photos, team members with prior fraud convictions.
3. **Analyze financial claims against industry benchmarks**: (a) Compare claimed returns to industry benchmarks. Hedge funds average 8-12% annually; claims of consistent 30%+ annual returns are extraordinary and require extraordinary evidence. (b) Assess return volatility: legitimate investments have drawdowns and negative periods. Claims of "never losing money" or "consistent monthly returns of X%" are classic Ponzi indicators. (c) Compare the claimed strategy to known strategy returns: long/short equity averages 6-10%, arbitrage averages 4-8%, crypto is highly volatile. (d) Apply the Sharpe Ratio test: if the claimed Sharpe Ratio exceeds 2.0 consistently, it is likely fabricated (even the best funds rarely sustain >2.0). (e) Ask for audited financial statements — legitimate funds have independent audits.
4. **Identify red flags using standardized checklist**: (a) Guaranteed returns: no legitimate investment can guarantee returns — this violates securities law in most jurisdictions. (b) Urgency pressure: "invest now or miss out," limited-time offers, countdown timers. (c) Complex/opaque structure: multiple layers of entities, unclear fee structure, inability to explain the investment strategy in plain language. (d) Unlicensed securities: investments sold without proper registration or exemption filings (Regulation D, Regulation A, etc.). (e) Difficulty withdrawing funds: lockup periods that extend indefinitely, "technical issues" with withdrawals, requirement to reinvest returns. (f) Referral bonuses: recruiting new investors is rewarded with bonuses — this is a classic pyramid/Ponzi structure. (g) Unverifiable track record: no audited performance, no third-party custodian, self-reported returns only.
5. **Verify independent audits and attestations**: (a) Is the fund audited by a recognized accounting firm (Big 4 or reputable mid-tier firm)? (b) Is there a third-party custodian holding the assets (separate from the fund manager)? The absence of an independent custodian was a key Madoff Ponzi red flag. (c) Is there a third-party administrator handling subscriptions/redemptions? (d) Are NAV (Net Asset Value) statements produced by an independent administrator? (e) RED FLAG: the fund manager is also the custodian and administrator (self-custody) — this eliminates independent verification.
6. **Check for Ponzi scheme indicators using the "returns funded by new investors" test**: (a) Examine the fund's claimed assets under management (AUM) growth vs. reported returns. If returns + AUM growth far exceed what the claimed strategy could plausibly generate, new investor capital may be funding old investor returns. (b) Check withdrawal patterns: do early investors report successful withdrawals? (This does not prove legitimacy — Ponzi schemes pay early investors to build reputation.) (c) Assess the sustainability: if the fund claims X% returns and manages $Y, does the market for the claimed strategy have enough liquidity/capacity to support those returns at that scale? (d) Look for "affinity fraud" indicators: is the investment being marketed within a specific community (religious group, ethnic community, professional group) using trust rather than due diligence?
7. **Assess regulatory compliance**: (a) Is the offering properly registered or exempt under securities law? (b) For private placements: is there a valid Reg D filing (Form D on SEC EDGAR)? (c) For crypto tokens: has there been a Howey test analysis to determine if the token is a security? (d) Are required disclosures (PPM — Private Placement Memorandum, offering circular) provided? (e) Is the marketing compliant with advertising rules (no general solicitation for certain exemptions)? (f) RED FLAG: no offering documents, no regulatory filings, marketed via social media without disclaimers.
8. **Produce risk assessment with classification**: (a) Tally the red flags identified across all steps. (b) Classification: (i) LEGITIMATE: entity is properly registered, audited, has independent custodian, realistic returns, transparent strategy, verifiable team — 0-1 minor red flags. (ii) SUSPICIOUS: some red flags present (2-3), possibly explainable but warrant extreme caution and further investigation before investing. (iii) LIKELY SCAM: multiple red flags (4+), including any combination of: unregistered entity, unverifiable team, guaranteed returns, self-custody, no audit, Ponzi indicators. (c) Recommendation: LEGITIMATE → proceed with normal due diligence. SUSPICIOUS → do not invest until all concerns are resolved; consult a securities attorney. LIKELY SCAM → do not invest; consider reporting to the SEC (sec.gov/tcr), CFTC, or relevant state regulator.

**Output**: A due diligence report containing: (a) entity registration and regulatory status verification, (b) team background check summary, (c) financial claims analysis vs. industry benchmarks, (d) red flag checklist with findings, (e) independent audit/custodian verification, (f) Ponzi indicator assessment, (g) regulatory compliance evaluation, (h) overall classification (Legitimate / Suspicious / Likely Scam) with confidence level, (i) recommendation and next steps (including regulatory reporting if warranted).

**AI Execution Notes**: SEC EDGAR for registration checks (company search, Form D filings, enforcement actions). FINRA BrokerCheck (brokercheck.finra.org) for broker/adviser verification. LinkedIn for personnel verification. For crypto/DeFi projects, check blockchain explorers (Etherscan, etc.) for smart contract verification and token distribution. Benchmark return data from Hedge Fund Research (HFR) indices or published Sharpe Ratio distributions. This procedure is informed by principles from The Intelligent Investor (Graham's emphasis on margin of safety and skepticism of extraordinary claims) and Security Analysis (the importance of verified financial data and independent audits). When in doubt, the default recommendation should be "do not invest" — the cost of missing a legitimate opportunity is far less than the cost of falling for a scam.

**Key Formulas**:
- Sharpe Ratio test: sustained >2.0 = likely fabricated
- Red Flag tally: 0-1 = Legitimate, 2-3 = Suspicious, 4+ = Likely Scam
- AUM sustainability: claimed returns x AUM vs. strategy market capacity

---

## §3 Cortex Logging

```json
{
  "text": "C-075 Scam Detection — due diligence for {entity_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "investment_decision",
    "procedure": "C-075",
    "domain": "finance",
    "level": 5,
    "company": "{entity_name}",
    "decision": "{legitimate/suspicious/likely-scam}",
    "tags": ["business-evaluation", "level-5-decision", "scam-detection"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 5 (pre-filter)
**WHEN**: Before committing capital to ANY investment, especially alternatives, private placements, crypto, or any opportunity with extraordinary return claims
**PREREQUISITES**: None (standalone due diligence — this is the first gate before any other Level 5 procedure)
**FEEDS INTO**: F-062 (Investment Research — gate check), F-094 (VC Eval — legitimacy verification), F-047 (S-1 Analysis — only if entity passes), Final Investment Memo for Pafi

**VK**: `✅ [C-075] Scam Detection | {entity_name} | Level 5 complete | Classification: {legitimate/suspicious/likely-scam}`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| None | Standalone | Scam detection has no upstream dependencies — it is the first gate |
| F-062 | Feeds into | Investment research proceeds only if C-075 clears |
| F-094 | Feeds into | VC evaluation proceeds only if C-075 clears |
| F-047 | Feeds into | IPO analysis proceeds only if C-075 clears |
| Final Memo | Feeds into | Classification appears in Final Investment Memo for Pafi |

---
