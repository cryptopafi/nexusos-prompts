---
type: procedure
created: 2026-03-17
status: active
slug: business-eval-l3
tags: [procedure, nexus]
---

# Business Evaluation Training Path — Level 3: Valuation Procedures (FORGE Standard)

**Procedure Count**: 12
**Status**: ACTIVE
**Created**: 2026-03-03
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Standard**: FORGE v1.0

**Note on §5 (Dependencies)**: All procedures in this file include a §5 Dependencies section beyond the FORGE Standard requirement (§1-§4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**PRISM Pipeline**: This file is part of the PRISM Business Evaluation Pipeline. Master procedure: `~/.nexus/procedures/PRISM.md`
**Tools Prerequisites**: Excel/Sheets mastery procedures in `tech/excel-sheets-mastery-batch1.md`, `batch2.md`, `batch3.md`
**Google Sheets**: All Tools-level procedures support dual Excel + Google Sheets paths. Key Sheets functions: GOOGLEFINANCE, QUERY, FILTER, ARRAYFORMULA, Apps Script.

---

## Table of Contents

| # | ID | Procedure | Complexity | Method |
|---|-----|-----------|-----------|--------|
| 1 | F-029 | NPV Calculation for Project Evaluation | INTERMEDIATE | NPV |
| 2 | F-030 | IRR Calculation and Multi-Method Project Ranking | INTERMEDIATE | IRR |
| 3 | F-035 | WACC Calculation | ADVANCED | WACC |
| 4 | F-036 | Beta Calculation and Interpretation | ADVANCED | Beta/CAPM |
| 5 | F-037 | EVA (Economic Value Added) | ADVANCED | EVA |
| 6 | F-039 | DCF Valuation (Company-Level) | ADVANCED | DCF |
| 7 | F-040 | Accounting-Based Valuation (Residual Income) | ADVANCED | Residual Income |
| 8 | F-041 | Comparable Company Analysis (Trading Comps) | INTERMEDIATE | Comps |
| 9 | F-042 | Comparable Transaction Analysis (Deal Comps) | INTERMEDIATE | Deal Comps |
| 10 | F-043 | Price-to-Revenue Valuation | INTERMEDIATE | P/Revenue |
| 11 | F-044 | P/E Ratio Valuation | INTERMEDIATE | P/E |
| 12 | F-049 | ESG Scoring and Integration | ADVANCED | ESG |

---
---

# F-029: NPV Calculation for Project Evaluation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-029

---

## §1 Problem

Net Present Value (NPV) answers the fundamental question: "Does this project or investment create or destroy shareholder value in today's dollars?" It translates a series of future cash flows into a single present-value figure, making go/no-go decisions quantifiable. Without NPV, decisions rely on gut feel or simplistic metrics like payback period, which ignore the time value of money and cash flows beyond the payback horizon.

**Situations covered**:
- Evaluating a capital expenditure project (new plant, equipment, product line) where the company needs a clear accept/reject decision
- Ranking mutually exclusive projects of different sizes or durations where IRR alone would give misleading results due to the scale problem
- NOT suitable as a standalone when the discount rate is highly uncertain or when comparing projects with very different risk profiles without adjusting the discount rate per project

---

## §2 Procedure

1. Identify the initial investment outlay (CF0) — the upfront capital expenditure required for the project. This is typically a negative cash flow at time zero. Source it from the project proposal, capital budget, or management estimates.
2. Forecast the expected future cash flows (CF1, CF2, ... CFn) for each year of the project's life. Use revenue projections minus operating costs, taxes, and changes in working capital. If valuing for an owner, request their revenue forecast, cost structure, and project timeline.
3. Determine the appropriate discount rate (r). For a single-division company, use WACC (see F-035). For a project with different risk than the parent company, use a project-specific discount rate adjusted for the project's beta. For a multi-country company, consider country-specific risk premiums.
4. Discount each future cash flow to its present value using the formula: PV(CFt) = CFt / (1 + r)^t, where t is the year number.
5. Sum all discounted cash flows including the initial investment: NPV = CF0 + CF1/(1+r)^1 + CF2/(1+r)^2 + ... + CFn/(1+r)^n. Note: CF0 is typically negative (an outflow) and is not discounted because it occurs at time zero.
6. Apply the decision rule: If NPV > 0, recommend the project (it creates shareholder value). If NPV < 0, reject the project (it destroys value). If NPV = 0, the project breaks even — neither creates nor destroys value.
7. For competing/mutually exclusive projects, rank by NPV magnitude: higher NPV is preferred. Do NOT rank by IRR alone when comparing projects of different scale (the "scale problem" — a smaller project can have higher IRR but lower absolute value creation).
8. Perform sensitivity analysis: vary the discount rate (+/- 2%), cash flow assumptions (+/- 10-20%), and project timeline to produce a range of NPV outcomes. Report the base case, optimistic, and pessimistic NPVs.

**Output**: A project valuation report containing: (a) the NPV in currency terms (e.g., "$2.7 million"), (b) a go/no-go recommendation, (c) sensitivity table showing NPV under varying discount rates and cash flow scenarios.

**AI Execution Notes**: Gather project cash flow estimates from the owner or from SEC EDGAR filings (for public company capex projects). Obtain the discount rate via WACC calculation (F-035) or from the owner's stated required return. Use the formula NPV = sum of [CFt / (1+r)^t] for t = 0 to n. For Excel-style computation, replicate the NPV function logic. Cross-reference cash flow forecasts against historical operating margins from Yahoo Finance or company 10-K filings. Flag if discount rate exceeds 20% (unusually high risk) or if cash flows assume >15% annual growth (optimistic).

**Tools prerequisite**: F-080 (TVM Functions) for NPV, PV, and FV Excel/Sheets functions used in discounting. Google Sheets: NPV/PV/FV functions are identical to Excel.

**Key Formulas** (quick reference):
- NPV = CF0 + CF1/(1+r)^1 + CF2/(1+r)^2 + ... + CFn/(1+r)^n
- PV(CFt) = CFt / (1 + r)^t
- Decision rule: NPV > 0 → accept; NPV < 0 → reject

---

## §3 Cortex Logging

```json
{
  "text": "F-029 NPV Calculation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-029",
    "method": "npv",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "npv", "project-evaluation"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When evaluating whether a specific project or capital expenditure creates shareholder value
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-035 (WACC — for discount rate)
**FEEDS INTO**: F-030 (IRR — used together for project ranking), Level 4 (Deal Structuring), Level 5 (Portfolio Construction)

**VK**: `✅ [F-029] NPV Calculation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | WACC provides the discount rate for NPV calculation |
| F-010 (Balance Sheet) | Requires output from | Balance sheet data needed for working capital projections |
| F-011 (Cash Flow) | Requires output from | Historical cash flows calibrate future projections |
| F-030 (IRR) | Used together with | NPV and IRR are companion project evaluation metrics; NPV is the tiebreaker |
| F-039 (DCF) | Cross-validates with | DCF uses the same discounting logic at company level; project NPV should be consistent with company-level value creation |

---
---

# F-030: IRR Calculation and Multi-Method Project Ranking

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-030

---

## §1 Problem

Internal Rate of Return (IRR) answers: "What annualized return does this project deliver on the invested capital?" It provides an intuitive percentage return that managers and investors can compare directly against the cost of capital. However, IRR has well-known pitfalls (multiple solutions, scale bias) that make it dangerous as a standalone metric. This procedure pairs IRR with NPV and PI to ensure correct project ranking.

**Situations covered**:
- Communicating project returns to non-finance stakeholders who think in percentage terms rather than absolute dollar NPV
- Ranking multiple independent projects under capital rationing using Profitability Index alongside IRR and NPV
- NOT suitable as the sole decision metric for mutually exclusive projects of different scale — always use NPV as the tiebreaker

---

## §2 Procedure

1. Collect the same cash flow series as F-029: initial investment (CF0, negative) and projected future cash flows (CF1 through CFn).
2. Examine the sign structure of the cash flows. If there is only one sign change (negative to positive), the IRR will be unique and reliable. If there are multiple sign changes (e.g., negative-positive-negative-positive), flag that multiple IRRs may exist — the Descartes rule allows up to as many IRRs as sign changes.
3. Calculate the IRR by solving: 0 = CF0 + CF1/(1+IRR)^1 + CF2/(1+IRR)^2 + ... + CFn/(1+IRR)^n. This requires iterative computation (Newton-Raphson method or equivalent). The IRR is the discount rate that makes NPV equal to zero.
4. Apply the decision rule for a single project: If IRR > WACC (or the required rate of return), accept the project. If IRR < WACC, reject it. The intuition: the project's return exceeds the cost of raising funds to invest in it.
5. If the IRR calculation yields multiple solutions or no solution (the NPV curve never crosses zero), abandon IRR and rely exclusively on NPV (F-029).
6. For mutually exclusive projects, calculate BOTH NPV and IRR for each project. If rankings conflict (Project A has higher IRR but Project B has higher NPV), always follow the NPV ranking. The IRR is biased toward smaller-scale investments — it is easier to get a high return on a small investment.
7. As a secondary check, calculate the incremental IRR: take the differential cash flows between the two projects (Project B minus Project A) and compute the IRR of that difference. If the incremental IRR > WACC, the larger project (B) is preferred.
8. Also compute the Profitability Index (PI = PV of future cash flows / initial investment) as a supplementary ranking metric when capital is rationed across multiple non-exclusive projects.
9. Report results in a multi-method comparison table: NPV, IRR, PI, and payback period for each project.

**Output**: A project ranking report containing: (a) IRR for each project (e.g., "15.0% vs. 7.2% WACC"), (b) NPV for each project, (c) clear recommendation stating which method was used as the tiebreaker and why, (d) flag any IRR anomalies (multiple solutions, no solution, scale problem).

**AI Execution Notes**: Use iterative solver logic to compute IRR (equivalent to Excel's IRR function). Source WACC from F-035. When cash flows have multiple sign changes, compute and plot the NPV profile (NPV vs. discount rate from 0% to 50%) to visualize where and if the curve crosses zero. Yahoo Finance and SEC filings provide historical capex and operating cash flow data for calibrating projections. Always present NPV as the primary decision metric with IRR as supplementary.

**Tools prerequisite**: F-080 (TVM Functions) for IRR and NPV Excel/Sheets functions. Google Sheets: IRR function is identical to Excel.

**Key Formulas** (quick reference):
- IRR: solve 0 = CF0 + CF1/(1+IRR)^1 + CF2/(1+IRR)^2 + ... + CFn/(1+IRR)^n
- Profitability Index: PI = PV of future cash flows / |CF0|
- Decision rule: IRR > WACC → accept; IRR < WACC → reject

---

## §3 Cortex Logging

```json
{
  "text": "F-030 IRR Calculation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-030",
    "method": "irr",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "irr", "project-ranking"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When ranking projects by return rate and comparing against the cost of capital
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-035 (WACC — for hurdle rate comparison)
**FEEDS INTO**: Level 4 (Deal Structuring — project selection inputs), Level 5 (Portfolio Construction)

**VK**: `✅ [F-030] IRR Calculation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | WACC is the hurdle rate against which IRR is compared |
| F-029 (NPV) | Used together with | NPV is the primary metric; IRR is supplementary. NPV is the tiebreaker when rankings conflict |
| F-010 (Balance Sheet) | Requires output from | Capital structure and working capital data feed cash flow projections |
| F-011 (Cash Flow) | Requires output from | Historical cash flows calibrate projected cash flows |

---
---

# F-035: WACC Calculation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-035

---

## §1 Problem

Weighted Average Cost of Capital (WACC) answers: "What is the minimum blended return a company must earn on its existing assets to satisfy both debt holders and equity investors?" WACC is the cornerstone discount rate used across multiple valuation methods — DCF, EVA, and NPV all depend on it. An incorrect WACC cascades errors into every downstream valuation. Skipping or miscalculating WACC renders DCF and EVA outputs unreliable.

**Situations covered**:
- Calculating the discount rate for a DCF valuation (F-039), EVA analysis (F-037), or NPV calculation (F-029)
- Benchmarking a company's return on invested capital against its cost of capital to assess value creation
- NOT suitable for companies with extremely unstable capital structures (e.g., during active restructuring) — use scenario-based discount rates instead

---

## §2 Procedure

1. Identify the company's capital structure components: long-term interest-bearing debt (D) and equity (E). Use market values, not book values. For debt: use the market value of outstanding bonds (or approximate with book value of long-term debt if bonds are not publicly traded). (Book value of debt is the fallback approximation when market prices are unavailable — it is not the preferred approach. When bonds trade publicly, always use market value.) For equity: market capitalization = current share price x shares outstanding.
2. Calculate the capital structure weights: xD = D / (D + E) and xE = E / (D + E). Verify that xD + xE = 1.0.
3. Determine the cost of debt (Rd): Use the yield to maturity (YTM) on the company's existing bonds, NOT the coupon rate. If no publicly traded bonds exist, use the interest rate on the company's most recent bank debt or estimate using credit rating and comparable bond yields.
4. Apply the tax shield to the cost of debt: After-tax cost of debt = Rd x (1 - T), where T is the marginal corporate tax rate. This reflects the tax deductibility of interest payments.
5. Determine the cost of equity (Re) using the CAPM (see F-036 for beta details): Re = Rf + Beta x MRP, where Rf = risk-free rate (10-year government bond yield, the most common choice used by ~46% of practitioners), MRP = market risk premium (historical equity return minus risk-free return; typically 5-6% for the US market), and Beta = the company's equity beta.
6. Alternative methods for cost of equity: (a) Dividend Discount Model: Re = (D1 / P0) + g, where D1 = expected next year dividend, P0 = current stock price, g = dividend growth rate. (b) Bond Yield Plus Risk Premium: Re = YTM on company's long-term debt + equity risk premium (typically 3-5%).
7. Calculate WACC: WACC = xE x Re + xD x Rd x (1 - T). This is the blended cost of capital reflecting both equity and debt financing proportions.
8. If the company also uses preferred stock, add a third component: WACC = xE x Re + xD x Rd x (1-T) + xP x Rp, where Rp = preferred dividend / preferred stock price, and xP = market value of preferred / total capital.
9. Sanity-check the result: for a typical US large-cap company, WACC ranges from 6-12%. If the result falls outside 4-15%, verify all inputs. Document all assumptions (which Rf, which MRP, which beta source).

**Output**: A WACC calculation worksheet showing: (a) each component (Re, Rd after-tax, weights), (b) the final WACC percentage (e.g., "7.2%"), (c) data sources for each input, (d) sensitivity of WACC to +/- 1% changes in cost of equity and cost of debt.

**AI Execution Notes**: Obtain market cap from Yahoo Finance (Key Statistics). Get long-term debt from the company's most recent 10-K or 10-Q balance sheet (SEC EDGAR). Risk-free rate: pull the current 10-year US Treasury yield from Treasury.gov or Yahoo Finance (^TNX). Beta: use Yahoo Finance's published beta or calculate per F-036. MRP: use Damodaran's updated equity risk premium dataset (pages.stern.nyu.edu/~adamodar/) or default to 5.5% for US markets. Tax rate: use the statutory rate (21% US federal) or effective tax rate from the company's income statement.

**Key Formulas** (quick reference):
- WACC = xE x Re + xD x Rd x (1 - T)
- Re (CAPM) = Rf + Beta x MRP
- After-tax cost of debt = Rd x (1 - T)

---

## §3 Cortex Logging

```json
{
  "text": "F-035 WACC Calculation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-035",
    "method": "wacc",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "wacc", "cost-of-capital"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When any downstream valuation (DCF, EVA, NPV) requires a discount rate
**PREREQUISITES**: F-010 (Balance Sheet Analysis — for capital structure), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-036 (Beta — for cost of equity via CAPM)
**FEEDS INTO**: F-029 (NPV — discount rate), F-037 (EVA — capital charge), F-039 (DCF — discount rate for UFCF), F-049 (ESG — baseline WACC before ESG adjustment)

**VK**: `✅ [F-035] WACC Calculation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-036 (Beta) | Requires output from | Beta is a key input to CAPM, which determines cost of equity (Re) |
| F-010 (Balance Sheet) | Requires output from | Capital structure (debt vs. equity weights) sourced from the balance sheet |
| F-029 (NPV) | Feeds into | NPV uses WACC as the discount rate for project cash flows |
| F-037 (EVA) | Feeds into | EVA uses WACC to compute the capital charge |
| F-039 (DCF) | Feeds into | DCF discounts UFCF at WACC to derive enterprise value |
| F-049 (ESG) | Feeds into | ESG integration adjusts the baseline WACC for ESG risk premia |

---
---

# F-036: Beta Calculation and Interpretation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-036

---

## §1 Problem

Beta quantifies a company's systematic risk — how much does its stock move relative to the broader market? It is the single most debated input in the CAPM and therefore in cost of equity and WACC. A wrong beta produces a wrong cost of equity, which cascades into every DCF, EVA, and NPV calculation downstream. This procedure standardizes beta estimation, adjustment, and cross-validation to minimize that risk.

**Situations covered**:
- Calculating a company-specific beta for use in CAPM to derive cost of equity (feeding into F-035 WACC)
- Estimating a project-specific beta when a company is entering a new industry with different risk characteristics (unlevering/relevering comparable betas)
- NOT suitable for companies with less than 2 years of public trading history — use industry average beta as a proxy instead

---

## §2 Procedure

1. Select the estimation period. The most popular choice is 5 years of historical data (~46% of practitioners). A shorter period (2-3 years) captures recent business changes better but may be noisy. A longer period (7-10 years) provides more data but may include outdated information if the company has changed significantly (e.g., restructuring, new business lines).
2. Select the return frequency: monthly returns are most common (giving 60 data points for a 5-year period). Weekly returns can also be used but introduce more noise.
3. Gather historical return data: (a) the company's stock returns for each period, and (b) the market index returns (typically S&P 500 for US stocks) for the same periods.
4. Calculate the beta using the regression formula: Beta = Covariance(Ri, Rm) / Variance(Rm), where Ri = stock return in period i, Rm = market return in period i. Alternatively, run a linear regression of Ri on Rm; the slope coefficient is beta.
5. Interpret the raw beta: Beta = 1.0 means the stock moves in line with the market. Beta > 1.0 means the stock amplifies market movements (more volatile/risky). Beta < 1.0 means the stock dampens market fluctuations (less volatile). Beta < 0 means the stock moves inversely to the market (rare).
6. Consider applying the Blume adjustment (mean reversion toward 1.0): Adjusted Beta = (2/3 x Raw Beta) + (1/3 x 1.0). This is what Bloomberg uses by default. It reflects the empirical tendency for betas to revert toward 1.0 over time.
7. For a project-specific beta (when evaluating a project in a different industry than the parent company): Find comparable pure-play companies in the project's industry, unlever their betas using the Hamada equation: Beta_unlevered = Beta_levered / [1 + (1-T) x (D/E)], then re-lever using the project's target capital structure: Beta_project = Beta_unlevered x [1 + (1-T) x (D/E)_project].
8. Compare the calculated beta against published betas from Yahoo Finance, Bloomberg, Value Line, and Reuters. Document any significant discrepancies and explain (different time periods, different return frequency, adjusted vs. raw).
9. Plug the final beta into the CAPM equation (F-035, step 5) to derive the cost of equity.

**Output**: A beta analysis report containing: (a) the calculated raw beta with methodology notes (period, frequency, index), (b) the adjusted beta (Blume), (c) comparison against 2-3 published beta sources, (d) interpretation of what the beta implies about the company's systematic risk, (e) the resulting cost of equity using CAPM.

**AI Execution Notes**: Download historical adjusted close prices from Yahoo Finance API (yfinance in Python) or via the Yahoo Finance website. Use the S&P 500 (^GSPC) as the market proxy for US stocks. Calculate monthly returns as (Pt - Pt-1) / Pt-1. Run regression using standard statistical methods. Cross-reference with Yahoo Finance's "Beta (5Y Monthly)" statistic displayed on each stock's summary page. For non-US companies, use the appropriate local market index (FTSE 100, DAX, Nikkei 225, etc.) and note the different MRP implications.

**Key Formulas** (quick reference):
- Beta = Covariance(Ri, Rm) / Variance(Rm)
- Blume Adjusted Beta = (2/3 x Raw Beta) + (1/3 x 1.0)
- Hamada (unlever): Beta_unlevered = Beta_levered / [1 + (1-T) x (D/E)]

---

## §3 Cortex Logging

```json
{
  "text": "F-036 Beta Calculation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-036",
    "method": "beta",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "beta", "capm", "systematic-risk"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When calculating cost of equity via CAPM for any company or project
**PREREQUISITES**: F-010 (Balance Sheet Analysis — for capital structure / D/E ratio), F-002/F-003/F-004 (Financial Ratios)
**FEEDS INTO**: F-035 (WACC — beta is a key CAPM input), and transitively into F-029 (NPV), F-037 (EVA), F-039 (DCF)

**VK**: `✅ [F-036] Beta Calculation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 (Balance Sheet) | Requires output from | D/E ratio needed for Hamada equation (unlever/relever) |
| F-035 (WACC) | Feeds into | Beta is plugged into CAPM to calculate Re, which feeds into WACC |
| F-039 (DCF) | Feeds into (transitively) | Beta affects WACC, which is the DCF discount rate |
| F-037 (EVA) | Feeds into (transitively) | Beta affects WACC, which determines the capital charge in EVA |

---
---

# F-037: EVA (Economic Value Added)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-037

---

## §1 Problem

Economic Value Added (EVA) answers: "Is this company truly creating wealth, or is it merely covering its accounting costs while failing to earn the return that capital providers require?" Unlike accounting profit (net income), EVA deducts the full cost of capital — both debt and equity. A company can report positive net income and still have negative EVA, meaning it destroys shareholder value. EVA is the sharpest tool for identifying hidden value destruction and for evaluating individual business units.

**Situations covered**:
- Assessing whether a company or business unit is genuinely creating economic value above its cost of capital
- Identifying which business segments to expand (positive EVA) vs. divest (negative EVA) in a multi-division company
- NOT suitable for early-stage companies with no established capital base or for companies undergoing major capital structure changes where WACC is unstable

---

## §2 Procedure

1. Calculate NOPAT (Net Operating Profit After Taxes): NOPAT = EBIT x (1 - Tax Rate). EBIT is earnings before interest and taxes from the income statement. Use the marginal tax rate, not the effective rate, for consistency. Alternatively: NOPAT = Net Income + Interest Expense x (1 - Tax Rate). Apply proprietary adjustments where relevant: capitalize R&D expenditures (amortize over useful life instead of expensing immediately), capitalize operating leases, adjust for goodwill amortization.
2. Determine the Capital Employed (also called Invested Capital): Capital = Total Assets - Non-Interest-Bearing Current Liabilities. Alternatively: Capital = Equity + Long-Term Debt + Short-Term Interest-Bearing Debt. Use the beginning-of-period capital (or average of beginning and ending). Adjust for capitalized R&D and operating leases to match the NOPAT adjustments.
3. Calculate WACC using procedure F-035.
4. Compute the Capital Charge: Capital Charge = Capital Employed x WACC. This represents the minimum return required by capital providers in dollar terms.
5. Calculate EVA using Definition 1 (absolute spread): EVA = NOPAT - (Capital x WACC). If EVA > 0, the company is creating economic value above the cost of capital. If EVA < 0, the company is destroying value even if it reports positive accounting profits.
6. Calculate EVA using Definition 2 (return spread): EVA = (ROIC - WACC) x Capital, where ROIC (Return on Invested Capital) = NOPAT / Capital. This formulation highlights the spread between what the company earns and what capital providers require.
7. Assess value creation levers: (a) Increase NOPAT by improving operating margins. (b) Invest more capital in activities where ROIC > WACC (positive spread activities). (c) Withdraw capital from activities where ROIC < WACC (value-destroying activities). (d) Optimize capital structure to minimize WACC.
8. Compare EVA across periods (year-over-year trend) and against industry peers. A rising EVA trend signals improving value creation. Compare EVA/Capital ratios to normalize for company size.
9. Distinguish clearly from accounting profit: a company can have positive net income but negative EVA if its earnings fail to cover the cost of capital employed. Flag this explicitly when it occurs.

**Output**: An EVA analysis report containing: (a) NOPAT calculation with adjustments listed, (b) Capital Employed figure, (c) WACC used, (d) EVA in currency terms (e.g., "EVA = $340M, indicating value creation"), (e) ROIC vs. WACC spread, (f) year-over-year EVA trend, (g) specific recommendations on which business units create/destroy value.

**AI Execution Notes**: Obtain EBIT, net income, interest expense, tax rate, total assets, and current liabilities from SEC EDGAR 10-K filings or Yahoo Finance financials tab. For R&D capitalization adjustments, pull R&D expense from the income statement and apply a 5-year straight-line amortization schedule. Use the WACC calculated in F-035. Benchmark EVA against Stern Stewart / EVA Dimensions published industry data or calculate peers' EVA for comparison using the same methodology.

**Key Formulas** (quick reference):
- EVA = NOPAT - (Capital Employed x WACC)
- EVA = (ROIC - WACC) x Capital Employed
- NOPAT = EBIT x (1 - Tax Rate)

---

## §3 Cortex Logging

```json
{
  "text": "F-037 EVA Analysis — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-037",
    "method": "eva",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "eva", "economic-value-added"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When measuring whether a company or business unit earns above its cost of capital
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-035 (WACC — required for capital charge)
**FEEDS INTO**: Level 4 (Deal Structuring — identifies value-creating vs. value-destroying segments), Level 5 (Portfolio Construction — screen for positive-EVA companies)

**VK**: `✅ [F-037] EVA Analysis | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | WACC is needed to compute the capital charge (Capital x WACC) |
| F-010 (Balance Sheet) | Requires output from | Capital Employed derived from balance sheet items |
| F-011 (Cash Flow) | Requires output from | NOPAT adjustments reference income statement and cash flow data |
| F-039 (DCF) | Cross-validates with | A company with sustained positive EVA should also show positive NPV in a DCF; if they diverge, investigate assumptions |

---
---

# F-039: DCF Valuation (Company-Level)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-039

---

## §1 Problem

Discounted Cash Flow (DCF) is the gold standard of intrinsic valuation — it answers: "What is this entire company worth based on its projected ability to generate free cash flow?" Unlike multiples-based methods, DCF does not rely on the market pricing comparable companies correctly. It is the most theoretically sound valuation method, but also the most sensitive to assumptions (growth rates, margins, discount rate, terminal value). Skipping DCF means relying entirely on relative valuation, which inherits any mispricing present in the peer group.

**Situations covered**:
- Full company valuation for M&A, investment decisions, or fairness opinions where an intrinsic (non-market-dependent) value is needed
- Valuing companies with predictable cash flows and stable business models where projections can be reasonably estimated
- NOT suitable for pre-revenue startups, companies in severe financial distress, or cyclical companies at extreme points in the cycle without normalization

---

## §2 Procedure

1. Gather the company's historical financial statements for the past 3-5 years: income statement, balance sheet, and cash flow statement. Source from SEC EDGAR (10-K filings) or Yahoo Finance. Identify key drivers: revenue growth rates, operating margins, capex as % of revenue, working capital ratios (DSO, DIO, DPO), effective tax rate, and D&A as % of PP&E.
2. Build revenue projections for 5-10 years. For established companies, use consensus analyst estimates for years 1-2 and taper growth toward the economy's long-term growth rate (2-3% nominal GDP growth) by the terminal year. For growth companies, model the revenue ramp based on market size, market share, pricing, and unit economics. Compare delivery/volume estimates against industry peers.
3. Forecast the full income statement: project cost of goods sold (COGS), operating expenses (SG&A, R&D), D&A, interest expense, and taxes. Use historical percentage ratios as a baseline and adjust for expected changes. Calculate Unlevered Free Cash Flow (UFCF): UFCF = EBIT x (1 - T) + D&A - CapEx - Change in Net Working Capital.
4. Calculate WACC using procedure F-035. This is the discount rate for the UFCF (since UFCF is available to all capital providers, both debt and equity holders).
5. Discount each year's projected UFCF to present value: PV(UFCFt) = UFCFt / (1 + WACC)^t.
6. Calculate the Terminal Value (TV) using the Gordon Growth Model (perpetuity growth method): TV = UFCFn x (1 + g) / (WACC - g), where UFCFn = last projected year's free cash flow, and g = long-term sustainable growth rate (1-3%; never assume a company can grow faster than nominal GDP indefinitely). The terminal value typically represents 60-80% of total enterprise value — flag if it exceeds 85% as this indicates heavy reliance on long-term assumptions.
7. Discount the terminal value to present: PV(TV) = TV / (1 + WACC)^n.
8. Calculate Enterprise Value (EV): EV = Sum of PV(UFCF) for years 1 through n + PV(TV).
9. Bridge from Enterprise Value to Equity Value: Equity Value = EV - Net Debt (total debt minus cash and cash equivalents) - Preferred Stock - Minority Interest + Value of Associate Companies. Implied Share Price = Equity Value / Diluted Shares Outstanding.
10. Perform sensitivity analysis: create a two-dimensional table varying WACC (e.g., 6-12%) and terminal growth rate (e.g., 1-3%). Present a range of implied share prices. Compare the implied share price to the current market price: if implied price > current price by 15%+, the stock appears undervalued; if implied price < current price by 15%+, the stock appears overvalued.

**Output**: A full DCF valuation model containing: (a) 5-10 year UFCF projections, (b) WACC derivation, (c) terminal value calculation, (d) Enterprise Value, (e) equity bridge to implied share price (e.g., "Implied share price: $185 vs. current $162, suggesting 14% upside"), (f) WACC vs. terminal growth sensitivity matrix, (g) comparison to current market price with valuation verdict.

**AI Execution Notes**: Pull historical financials from SEC EDGAR (10-K) or Yahoo Finance API. Get current share price and shares outstanding from Yahoo Finance. Revenue consensus estimates available from Yahoo Finance's Analysis tab. For the risk-free rate, use the current 10-year US Treasury yield. Use Damodaran's ERP dataset for market risk premium. Calculate beta per F-036. Net debt = total long-term debt + current portion of LTD + short-term borrowings - cash - short-term investments (from the balance sheet). For the terminal growth rate, never use more than 3% for any company — most analysts use 2-2.5% for mature companies.

**Tools prerequisites**: F-080 (TVM Functions) for PV/NPV discounting. F-070 (Model Mapping) for the sheet architecture of a DCF model. Google Sheets: NPV/PV functions identical to Excel; use GOOGLEFINANCE for live share price inputs.

**Key Formulas** (quick reference):
- UFCF = EBIT x (1 - T) + D&A - CapEx - Change in NWC
- Terminal Value = UFCFn x (1 + g) / (WACC - g)
- Enterprise Value = Sum of PV(UFCF) + PV(Terminal Value)

---

## §3 Cortex Logging

```json
{
  "text": "F-039 DCF Valuation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-039",
    "method": "dcf",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "dcf", "intrinsic-value"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When a full intrinsic valuation of a company is required, independent of market multiples
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-035 (WACC), F-036 (Beta)
**FEEDS INTO**: F-041/F-042 (cross-validation against comps), Level 4 (Deal Structuring — anchor price for negotiations), Level 5 (Portfolio Construction)

**VK**: `✅ [F-039] DCF Valuation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | WACC is the discount rate for all projected UFCF and terminal value |
| F-036 (Beta) | Requires output from (transitively) | Beta feeds WACC via CAPM cost of equity |
| F-010 (Balance Sheet) | Requires output from | Net debt, working capital, and PP&E data for equity bridge and UFCF projections |
| F-011 (Cash Flow) | Requires output from | Historical cash flows calibrate UFCF projections |
| F-041 (Trading Comps) | Cross-validates with | DCF intrinsic value should be compared against market multiples-based value; divergence flags mispricing or flawed assumptions |
| F-042 (Deal Comps) | Cross-validates with | Deal comps include control premium; DCF value should fall between trading comps and deal comps for an acquisition target |
| F-040 (Residual Income) | Cross-validates with | Alternative intrinsic method; results should be within 15-20% of each other for the same company |

---
---

# F-040: Accounting-Based Valuation (Residual Income)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-040

---

## §1 Problem

Residual Income valuation answers: "What is the company's equity worth, anchored to its book value and adjusted for its ability to earn above the cost of equity?" Unlike DCF, which derives value entirely from projected cash flows (making it highly sensitive to terminal value assumptions), residual income starts from an observable anchor — book value — and adds only the present value of "abnormal" earnings. Research shows this model outperforms finite-horizon DCF because the book value anchor reduces reliance on long-term projections.

**Situations covered**:
- Valuing financial institutions (banks, insurers) where cash flows are difficult to define but book value and earnings are well-established
- Providing a second intrinsic valuation to cross-check a DCF model, especially when terminal value dominates the DCF result (>80%)
- NOT suitable for companies with negative book value (e.g., companies that have accumulated large deficits or have engaged in aggressive buybacks) — the anchor becomes meaningless

---

## §2 Procedure

1. Determine the company's current book value of equity (stockholders' equity) from the most recent balance sheet. This is the starting anchor for the valuation.
2. Obtain or estimate the cost of equity (r) using the CAPM (see F-036 and F-035). This represents the return that equity investors require.
3. Forecast net income (or earnings) for each of the next 5-10 years, using the financial statement analysis and forecasting procedures. Base forecasts on historical ROE trends, revenue growth, and margin assumptions.
4. For each forecast year, calculate Abnormal Earnings (also called Residual Income): AE_t = Net Income_t - (r x Book Value of Equity_{t-1}). The term (r x BV_{t-1}) represents the "normal" earnings expected given the equity invested. Any earnings above this threshold represent value creation; below represents value destruction.
5. Forecast the book value of equity for each year using the clean surplus relation: BV_t = BV_{t-1} + Net Income_t - Dividends_t. If dividends are not forecasted, estimate using the historical payout ratio.
6. Discount each year's abnormal earnings to present value: PV(AE_t) = AE_t / (1 + r)^t.
7. Calculate the terminal value of abnormal earnings beyond the explicit forecast period. Options: (a) assume abnormal earnings decay to zero (most conservative — competition erodes excess returns), (b) assume abnormal earnings persist at the last forecasted level forever: TV = AE_n / r, (c) assume abnormal earnings grow at a rate g < r: TV = AE_n x (1+g) / (r - g). Discount the terminal value to present.
8. Sum all components: Intrinsic Equity Value = Current Book Value + Sum of PV(Abnormal Earnings) + PV(Terminal Value of AE).
9. Divide by diluted shares outstanding to get the implied share price.
10. Compare to the current market price. If the intrinsic value is significantly higher than the market price, the stock may be undervalued; if lower, it may be overvalued. Research shows the residual income model tends to outperform finite-horizon DCF models because the book value anchor reduces the sensitivity to terminal value assumptions.

**Output**: A residual income valuation report containing: (a) current book value of equity, (b) cost of equity used, (c) year-by-year abnormal earnings schedule, (d) terminal value of abnormal earnings, (e) total intrinsic equity value and implied share price (e.g., "Implied: $74 vs. market $68 — 9% upside"), (f) comparison of this valuation to a DCF valuation of the same company.

**AI Execution Notes**: Obtain book value of equity from the balance sheet (SEC EDGAR or Yahoo Finance). Net income from the income statement. Cost of equity from F-036/F-035. Historical ROE = Net Income / Average Stockholders' Equity (for calibrating projections). Dividends per share from Yahoo Finance dividend history. This model is particularly useful for financial institutions (banks, insurers) where cash flows are hard to define but book value and earnings are well-established. Flag if the company has negative book value (this model breaks down in that scenario).

**Tools prerequisite**: F-080 (TVM Functions) for PV discounting of abnormal earnings in steps 6-7.

**Key Formulas** (quick reference):
- Abnormal Earnings: AE_t = Net Income_t - (r x Book Value_{t-1})
- Intrinsic Equity Value = Book Value + Sum of PV(AE_t) + PV(Terminal Value of AE)
- Clean Surplus: BV_t = BV_{t-1} + Net Income_t - Dividends_t

---

## §3 Cortex Logging

```json
{
  "text": "F-040 Residual Income Valuation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-040",
    "method": "residual-income",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "residual-income", "book-value"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When valuing companies where book value is a meaningful anchor (especially financials) or as a DCF cross-check
**PREREQUISITES**: F-010 (Balance Sheet Analysis — for book value of equity), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios — especially ROE), F-035 (WACC — cost of equity component), F-036 (Beta)
**FEEDS INTO**: Level 4 (Deal Structuring), Level 5 (Portfolio Construction)

**VK**: `✅ [F-040] Residual Income Valuation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | Cost of equity (Re component of WACC) used as the required return in abnormal earnings calculation |
| F-036 (Beta) | Requires output from (transitively) | Beta feeds cost of equity via CAPM |
| F-010 (Balance Sheet) | Requires output from | Book value of equity is the valuation anchor |
| F-039 (DCF) | Cross-validates with | Both are intrinsic valuation methods; results should converge within 15-20% for the same company. If the divergence between Residual Income valuation and DCF exceeds 20%, audit all shared assumptions before reporting: (a) verify the discount rate is identical in both models, (b) confirm terminal growth rate consistency, (c) check that clean surplus accounting holds (no direct equity adjustments bypassing income statement), and (d) reconcile the terminal value methodology (perpetuity vs. fade). |
| F-041 (Trading Comps) | Cross-validates with | P/B multiple from comps should be consistent with the implied premium over book value from residual income |

---
---

# F-041: Comparable Company Analysis (Trading Comps)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-041

---

## §1 Problem

Trading comps answer: "What is the market willing to pay for similar businesses right now?" This is a relative valuation method — it derives value from how the market prices peer companies, rather than from intrinsic cash flow projections. It is fast, market-grounded, and widely used in investment banking and equity research. However, if the entire peer group is overvalued or undervalued, the comps-based valuation will inherit that mispricing. That is why it must always be triangulated with intrinsic methods like DCF.

**Situations covered**:
- Quick market-based valuation when time is limited and a DCF is not yet built
- Sanity-checking a DCF result against what the market currently pays for similar companies
- NOT suitable as the sole method when the target has no close public comparables or when the entire sector is in a bubble/distress

---

## §2 Procedure

1. Identify the target company to be valued. Determine its industry, sub-sector, business model, geographic focus, size (revenue, market cap), growth profile, and profitability.
2. Select 5-10 comparable publicly traded companies ("peer group"). Criteria for comparability: same industry/sub-sector, similar business model, similar size range (within 0.5x to 2x revenue), similar growth rates, similar geographic exposure, similar capital structure. Cast a wider net if close comps are scarce.
3. For each comparable company, gather the following data: Market Capitalization, Enterprise Value (EV = Market Cap + Total Debt - Cash), Revenue (TTM and forward), EBITDA (TTM and forward), Net Income / EPS (TTM and forward), Book Value of Equity.
4. Calculate key valuation multiples for each comp: (a) EV/Revenue, (b) EV/EBITDA, (c) P/E (Price/Earnings), (d) P/B (Price/Book Value), (e) EV/EBIT. Use both trailing twelve months (TTM) and forward (next twelve months, NTM) figures.
5. Compute the mean, median, 25th percentile, and 75th percentile for each multiple across the peer group. The median is generally preferred over the mean as it reduces the effect of outliers.
6. Identify and exclude outliers: remove companies with negative EBITDA or earnings (multiples become meaningless), companies undergoing M&A or restructuring, or companies with multiples >3 standard deviations from the mean.
7. Apply the selected multiples to the target company's financial metrics. For example: Implied EV = Target's EBITDA x Median EV/EBITDA from comps. Implied Equity Value = Implied EV - Net Debt. Implied Share Price = Implied Equity Value / Diluted Shares Outstanding.
8. Triangulate using multiple metrics (EV/Revenue, EV/EBITDA, P/E) to create a valuation range rather than a single point estimate. Present as a "football field" chart showing the range from each methodology.
9. Assess whether the target deserves a premium or discount relative to the peer median. Reasons for a premium: faster growth, higher margins, stronger brand, superior management. Reasons for a discount: lower growth, customer concentration, regulatory risk, weaker balance sheet.

**Output**: A trading comps valuation report containing: (a) peer group selection rationale, (b) summary table of all multiples for each peer, (c) median/mean multiples, (d) implied valuation range for the target (e.g., "EV/EBITDA range: $2.1B - $2.8B; P/E range: $45 - $62 per share"), (e) premium/discount assessment, (f) recommended valuation range.

**AI Execution Notes**: Identify peers using Yahoo Finance sector/industry classifications, or SEC EDGAR SIC codes. Pull market cap, enterprise value, revenue, EBITDA, and EPS from Yahoo Finance's Key Statistics and Financials pages. Forward estimates from Yahoo Finance's Analysis tab (consensus). Use Damodaran's published industry-level multiples (pages.stern.nyu.edu/~adamodar/) as a sanity check. For private companies, adjust public company multiples by a 15-30% illiquidity discount.

**Tools prerequisite**: F-081 (VLOOKUP/XLOOKUP) for retrieving peer financial data by ticker from compiled databases. Google Sheets: use GOOGLEFINANCE for live market cap, P/E, and price data; FILTER+QUERY for dynamic peer screening.

**Key Formulas** (quick reference):
- EV/EBITDA = Enterprise Value / EBITDA
- Implied EV = Target EBITDA x Median Peer EV/EBITDA
- Equity Value = Enterprise Value - Net Debt

---

## §3 Cortex Logging

```json
{
  "text": "F-041 Trading Comps — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-041",
    "method": "comps",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "trading-comps", "relative-valuation"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When a market-based valuation is needed to complement or cross-check intrinsic valuation methods
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios)
**FEEDS INTO**: F-042 (Deal Comps — combined with trading comps for full relative valuation picture), Level 4 (Deal Structuring — establishes market-based price range), Level 5 (Portfolio Construction)

**VK**: `✅ [F-041] Trading Comps | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 (Balance Sheet) | Requires output from | Enterprise Value calculation requires debt and cash data |
| F-002/F-003/F-004 (Ratios) | Requires output from | Profitability and growth ratios used for peer selection criteria |
| F-039 (DCF) | Cross-validates with | Trading comps provide market-based value; DCF provides intrinsic value. Divergence flags mispricing or flawed assumptions |
| F-042 (Deal Comps) | Cross-validates with | Deal comps should produce higher valuations (control premium embedded). If they produce lower, investigate sector distress |
| F-043 (P/Revenue) | Cross-validates with | P/Revenue is a subset of comps methodology focused on revenue multiples |
| F-044 (P/E) | Cross-validates with | P/E is a subset of comps methodology focused on earnings multiples |

---
---

# F-042: Comparable Transaction Analysis (Deal Comps)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-042

---

## §1 Problem

Deal comps answer: "What have acquirers actually paid for similar businesses in recent transactions?" Unlike trading comps (which reflect minority-stake market pricing), deal comps embed a control premium — the extra amount buyers pay for the right to control the business. This makes deal comps essential for M&A pricing, fairness opinions, and any scenario where full ownership is being acquired. Skipping deal comps in an M&A context means missing the most directly relevant pricing data.

**Situations covered**:
- Pricing an acquisition target where a control premium over trading value is expected
- Providing a fairness opinion by showing what similar deals have traded at historically
- NOT suitable when few or no comparable transactions exist in the industry (common in niche sectors), or when recent transactions occurred in materially different economic conditions

---

## §2 Procedure

1. Identify the target company's industry, size, and the purpose of the valuation (M&A pricing, fairness opinion, estate/gift tax valuation).
2. Search for comparable M&A transactions within the same or closely related industry completed within the past 3-5 years. Key databases: SEC EDGAR (merger proxy statements, S-4 filings), press releases, and financial databases. Prioritize transactions with publicly disclosed deal values.
3. For each comparable transaction, record: (a) Acquirer and target names, (b) Transaction date, (c) Deal value (Enterprise Value paid), (d) Target's Revenue (LTM at time of deal), (e) Target's EBITDA (LTM at time of deal), (f) Target's Net Income, (g) Deal type (100% acquisition, majority stake, asset purchase), (h) Payment method (cash, stock, mix), (i) Strategic vs. financial buyer.
4. Calculate transaction multiples for each deal: (a) EV/Revenue, (b) EV/EBITDA, (c) P/E (if equity value and earnings are available), (d) EV/EBIT.
5. Compute the median, mean, 25th, and 75th percentile for each multiple across all comparable transactions.
6. Adjust for time: if transactions occurred during materially different market conditions (e.g., pre/post COVID, different interest rate environments), note this and potentially weight more recent transactions more heavily.
7. Apply the median transaction multiples to the target company's current financial metrics. For example: Implied EV = Target's LTM EBITDA x Median Transaction EV/EBITDA. Bridge to equity value: Equity Value = Implied EV - Net Debt.
8. The deal comps inherently include a "control premium" (typically 20-40% above trading price) because acquirers pay a premium for control. If valuing a minority stake, discount the result accordingly. If valuing for a full acquisition, the deal comps multiple is directly applicable.
9. Compare deal comps valuations to trading comps (F-041) results. The deal comps should generally produce higher valuations due to the embedded control premium. If deal comps produce lower valuations, investigate whether the target's industry is in distress.

**Output**: A transaction comps valuation report containing: (a) table of comparable transactions with deal details and multiples, (b) summary statistics (median, range), (c) implied valuation of the target based on deal multiples (e.g., "Based on median deal EV/EBITDA of 12.5x, implied EV = $3.2B"), (d) comparison to trading comps, (e) note on embedded control premium and its implications.

**AI Execution Notes**: Search for comparable transactions using SEC EDGAR EDGAR full-text search (keywords: "merger agreement", "acquisition" + industry terms), or use press release databases. Financial data at the time of the transaction may be found in merger proxy statements (DEF 14A) on SEC EDGAR. For the target's current financials, use Yahoo Finance or the latest 10-K/10-Q. If deal details are not publicly available, note the limitation and use broader industry transaction data from Damodaran's industry multiples.

**Key Formulas** (quick reference):
- Transaction EV/EBITDA = Deal Enterprise Value / Target LTM EBITDA
- Implied EV = Target's EBITDA x Median Transaction EV/EBITDA
- Control Premium = (Deal Price - Pre-Announcement Trading Price) / Pre-Announcement Trading Price

---

## §3 Cortex Logging

```json
{
  "text": "F-042 Deal Comps — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-042",
    "method": "deal-comps",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "deal-comps", "transaction-analysis", "m-and-a"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When pricing an acquisition, preparing a fairness opinion, or establishing control-premium-inclusive valuation
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios)
**FEEDS INTO**: Level 4 (Deal Structuring — directly informs acquisition pricing), Level 5 (Portfolio Construction)

**VK**: `✅ [F-042] Deal Comps | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 (Balance Sheet) | Requires output from | Net debt needed for equity bridge from implied EV |
| F-002/F-003/F-004 (Ratios) | Requires output from | Financial metrics used to apply transaction multiples to the target |
| F-041 (Trading Comps) | Cross-validates with | Deal comps should exceed trading comps by the control premium (20-40%); if not, investigate distress |
| F-039 (DCF) | Cross-validates with | DCF provides intrinsic value; deal comps provide market-transaction-based value. Triangulating both strengthens the valuation |

---
---

# F-043: Price-to-Revenue Valuation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-043

---

## §1 Problem

Price-to-Revenue valuation answers: "What is the market paying per dollar of revenue for companies like this?" It is the valuation method of last resort for unprofitable companies — when earnings, EBITDA, and cash flows are all negative, revenue is the only positive financial metric available for multiples-based valuation. It is inherently less precise because revenue ignores profitability entirely, but it is indispensable for early-stage, high-growth companies that have not yet reached profitability.

**Situations covered**:
- Valuing pre-profit companies (negative earnings, negative EBITDA) such as early-stage SaaS, biotech with product revenue but ongoing losses, or high-growth tech
- Quick relative valuation when revenue is the most reliable and comparable metric across peers
- NOT suitable as a standalone for profitable companies — always pair with P/E (F-044) or EV/EBITDA (F-041) when earnings are available, since revenue multiples ignore cost structure entirely

---

## §2 Procedure

1. Identify the target company and confirm it is appropriate for P/Revenue valuation. This method is most useful for companies that are not yet profitable (negative earnings), high-growth companies where current profits do not reflect future potential, or early-stage companies with revenue but no positive cash flow. Examples: pre-profit tech companies, high-growth SaaS, biotech with product revenue but ongoing losses.
2. Obtain the target company's trailing twelve months (TTM) revenue and forward (NTM) revenue estimates from Yahoo Finance or SEC filings.
3. Calculate the target's current Price/Revenue ratio: P/R = Market Capitalization / TTM Revenue. Alternatively, use EV/Revenue = Enterprise Value / TTM Revenue for a capital-structure-neutral comparison.
4. Select 5-10 comparable companies (same industry, similar growth profile, similar business model). For each comparable, calculate: (a) P/Revenue (TTM), (b) P/Revenue (NTM based on consensus forward revenue), (c) EV/Revenue (TTM and NTM).
5. Calculate the median and range of P/Revenue and EV/Revenue multiples across the peer group.
6. Apply the peer median P/Revenue multiple to the target's revenue to get an implied market cap: Implied Market Cap = Target's Revenue x Median P/Revenue of peers. Divide by shares outstanding to get implied share price.
7. For a more refined analysis, use the PEG-like logic adapted for revenue: companies growing revenue faster should command higher P/Revenue multiples. Calculate the "Price/Revenue-to-Growth" ratio for each peer and apply the appropriate growth-adjusted multiple to the target.
8. Assess whether the target's growth rate, margin trajectory, and market opportunity justify a premium or discount to peer median. A company with higher revenue growth and a clear path to profitability deserves a premium; one with slowing growth or questionable unit economics deserves a discount.
9. Provide the valuation as a range based on 25th to 75th percentile of peer multiples applied to the target's revenue.

**Output**: A Price/Revenue valuation report containing: (a) target's current P/Revenue and EV/Revenue, (b) peer group multiples summary table, (c) implied share price range (e.g., "Applying 8x-12x P/Revenue to $2.1B revenue implies $67-$100 per share"), (d) comparison to current market price, (e) qualitative assessment of premium/discount factors.

**AI Execution Notes**: Pull market cap and revenue from Yahoo Finance Key Statistics. Revenue consensus estimates from Yahoo Finance Analysis tab. Peer selection via industry classification. This method is inherently less precise than DCF or P/E because revenue does not capture profitability — always present it alongside at least one other valuation method. Flag if the P/Revenue multiple exceeds 20x, as this implies extreme growth expectations.

**Key Formulas** (quick reference):
- P/Revenue = Market Capitalization / TTM Revenue
- EV/Revenue = Enterprise Value / TTM Revenue
- Implied Market Cap = Target Revenue x Median Peer P/Revenue

---

## §3 Cortex Logging

```json
{
  "text": "F-043 P/Revenue Valuation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-043",
    "method": "p-revenue",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "p-revenue", "relative-valuation"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When valuing unprofitable companies or as a supplementary multiple alongside P/E and EV/EBITDA
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios)
**FEEDS INTO**: Level 4 (Deal Structuring), Level 5 (Portfolio Construction)

**VK**: `✅ [F-043] P/Revenue Valuation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 (Balance Sheet) | Requires output from | Enterprise Value calculation requires debt and cash data |
| F-002/F-003/F-004 (Ratios) | Requires output from | Growth rates and margin trends used for peer selection and premium/discount assessment |
| F-044 (P/E) | Cross-validates with | For profitable companies, P/Revenue and P/E should tell a consistent story; if P/Revenue says cheap but P/E says expensive, margins are the issue |
| F-041 (Trading Comps) | Cross-validates with | P/Revenue is effectively one multiple within the broader comps framework |
| F-039 (DCF) | Cross-validates with | DCF intrinsic value provides a fundamentals-based anchor to compare against the market-derived P/Revenue valuation |

---
---

# F-044: P/E Ratio Valuation

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-044

---

## §1 Problem

P/E ratio valuation answers: "How much is the market willing to pay per dollar of this company's earnings, and is that premium justified by its growth?" P/E is the single most widely quoted valuation metric in financial media and equity research. The PEG ratio extends it by adjusting for growth, providing a quick screen for overvalued or undervalued stocks. However, P/E is easily distorted by one-time items, capital structure differences, and cyclicality, requiring careful normalization and cross-checking.

**Situations covered**:
- Quick screening of public equities to identify potentially over/undervalued stocks relative to peers and the broader market
- Valuing mature, profitable companies with stable earnings where P/E multiples are meaningful and comparable
- NOT suitable for companies with negative or near-zero earnings, highly cyclical companies at earnings peaks/troughs without normalization, or companies with heavily distorted EPS from stock-based compensation

---

## §2 Procedure

1. Confirm the target company has positive earnings (net income > 0). If the company is not yet profitable, use P/Revenue (F-043) or DCF (F-039) instead.
2. Obtain the target company's current P/E ratio: P/E = Share Price / Earnings Per Share (EPS). Use both trailing P/E (based on last 12 months' reported EPS) and forward P/E (based on consensus EPS estimate for the next 12 months). Source from Yahoo Finance's Key Statistics page.
3. Calculate the PEG ratio: PEG = P/E / Annual EPS Growth Rate (%). A PEG below 1.0 suggests the stock is cheap relative to its growth. A PEG above 2.0 suggests it is expensive. A PEG of ~1.0 implies fair value (the P/E multiple equals the earnings growth rate). Industry rule of thumb: a company growing earnings at 20% per year "should" trade at roughly 20x P/E.
4. Select 5-10 comparable companies in the same industry/sector. For each comparable, calculate: (a) trailing P/E, (b) forward P/E, (c) PEG ratio, (d) earnings growth rate.
5. Calculate the median and range (25th to 75th percentile) of P/E and PEG ratios across the peer group.
6. Apply the peer median P/E to the target's EPS to derive an implied share price: Implied Price = Target's Forward EPS x Median Forward P/E of peers. Calculate a range using the 25th and 75th percentile P/E multiples.
7. Assess whether the target deserves a premium or discount P/E relative to peers. Higher growth, stronger competitive moat, recurring revenue, and superior management justify a premium. Cyclicality, customer concentration, regulatory risk, and declining margins justify a discount.
8. Cross-check against the overall market P/E. The S&P 500 historically trades around 15-20x trailing earnings. If the target's P/E is significantly above the market average, ensure the growth rate justifies it.
9. For additional precision, compare EV/EBITDA multiples alongside P/E, as P/E can be distorted by capital structure differences (highly leveraged companies will have amplified EPS volatility).
10. Flag potential P/E pitfalls: (a) cyclical companies may have artificially low P/E at earnings peaks and high P/E at earnings troughs — use normalized (mid-cycle) earnings. (b) One-time charges or gains can distort EPS — use adjusted/recurring EPS. (c) Stock-based compensation may suppress reported EPS — check diluted share count.

**Output**: A P/E valuation report containing: (a) target's trailing and forward P/E, (b) target's PEG ratio, (c) peer group P/E and PEG summary table, (d) implied share price range (e.g., "At 18x-24x forward P/E on $5.20 EPS, implied range is $94-$125"), (e) overvalued/undervalued assessment relative to peers and the broader market, (f) flag any P/E distortions.

**AI Execution Notes**: Pull P/E, forward P/E, and PEG from Yahoo Finance Key Statistics. EPS from the income statement. EPS growth rate from Yahoo Finance's Analysis tab (5-year expected growth rate). Peer selection via sector/industry classification. For cyclical stocks, calculate a "Shiller P/E" (price / 10-year average inflation-adjusted earnings) for additional context. Compare against Damodaran's published industry P/E averages for sanity checking.

**Key Formulas** (quick reference):
- P/E = Share Price / Earnings Per Share (EPS)
- PEG = P/E / Annual EPS Growth Rate (%)
- Implied Price = Target Forward EPS x Median Peer Forward P/E

---

## §3 Cortex Logging

```json
{
  "text": "F-044 P/E Valuation — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-044",
    "method": "p-e",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "p-e-ratio", "relative-valuation"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When screening or valuing profitable companies using earnings-based multiples
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios)
**FEEDS INTO**: Level 4 (Deal Structuring), Level 5 (Portfolio Construction)

**VK**: `✅ [F-044] P/E Valuation | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002/F-003/F-004 (Ratios) | Requires output from | EPS growth rates, profitability ratios used for peer selection and PEG calculation |
| F-010 (Balance Sheet) | Requires output from | Capital structure awareness needed to identify P/E distortions from leverage |
| F-043 (P/Revenue) | Cross-validates with | P/Revenue and P/E should tell a consistent valuation story; divergence highlights margin differentials |
| F-041 (Trading Comps) | Cross-validates with | P/E is one multiple within the broader comps framework; EV/EBITDA from comps normalizes for capital structure |
| F-039 (DCF) | Cross-validates with | DCF provides intrinsic value independent of market multiples; significant divergence from P/E-implied value warrants investigation |

---
---

# F-049: ESG Scoring and Integration

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 3
**Complexity**: ADVANCED
**Rule**: TRAIN-F-049

---

## §1 Problem

ESG integration answers: "How do environmental, social, and governance factors affect this company's risk profile and intrinsic value?" ESG is no longer a niche concern — it directly impacts cost of capital, regulatory exposure, litigation risk, and long-term cash flow sustainability. Companies with poor ESG practices face higher risk premiums (50-200 basis points empirically), while strong ESG performers often access cheaper capital. Skipping ESG integration means ignoring a material risk/opportunity dimension that increasingly drives institutional investment decisions.

**Situations covered**:
- Adjusting a DCF valuation for ESG risk factors by modifying WACC or cash flow projections
- Screening a portfolio for ESG compliance using negative screening (exclusion) or positive screening (best-in-class selection)
- NOT suitable as a standalone valuation method — ESG adjusts or supplements existing valuations (DCF, comps) rather than replacing them

---

## §2 Procedure

1. Define the three ESG pillars for the target company: (a) Environmental: carbon emissions, energy efficiency, waste management, water usage, environmental policy, climate change exposure. (b) Social: employee safety, labor practices, human rights, community engagement, product safety, diversity and inclusion. (c) Governance: board independence, executive compensation alignment, shareholder rights, audit quality, anti-corruption policies, ownership structure.
2. Gather ESG data from multiple sources: company sustainability reports, CDP (Carbon Disclosure Project) filings, third-party ESG rating agencies (MSCI ESG Ratings, Sustainalytics, S&P Global ESG Scores, ISS ESG), and regulatory filings.
3. Score each pillar on a standardized scale (e.g., 0-100 or 1-5). Within each pillar, weight sub-factors by materiality to the industry. For example, carbon emissions weight more heavily for energy companies; labor practices weight more heavily for manufacturing/retail.
4. Apply one of the standard ESG integration strategies: (a) Negative Screening (Exclusionary): Exclude companies involved in tobacco, weapons, fossil fuels, or those with poor human rights practices. Set a revenue threshold (e.g., >5% of revenue from excluded activities = exclusion). (b) Positive Screening (Best-in-Class): Select companies with top-quartile ESG scores within their industry. (c) ESG Integration: Incorporate ESG scores directly into the financial valuation by adjusting the discount rate or cash flow projections. (d) Impact Investing: Target investments that generate measurable social/environmental outcomes alongside financial returns.
5. For ESG-integrated valuation, adjust the DCF model (F-039): (a) Discount rate adjustment: add 0.5-2.0% to WACC for companies with poor ESG scores (higher risk of regulatory fines, reputational damage, stranded assets). Reduce WACC by 0.25-0.75% for companies with superior ESG practices (lower cost of capital due to lower risk). (b) Cash flow adjustment: reduce projected cash flows by estimated costs of ESG-related risks (carbon taxes, remediation costs, litigation reserves). Increase projected cash flows for ESG benefits (energy savings, employee retention, brand premium).
6. Calculate the ESG-adjusted implied share price and compare to the non-ESG-adjusted DCF valuation. The difference represents the "ESG premium/discount" the market should apply.
7. Compare the company's ESG score against industry peers. Present a percentile ranking. Flag any material ESG controversies (e.g., ongoing environmental litigation, workplace safety violations, governance scandals) that could trigger sudden value destruction.
8. Assess the trajectory: is the company's ESG score improving or deteriorating? A company with improving ESG practices may warrant a re-rating (higher multiple); deteriorating ESG practices may signal increasing risk.
9. Document the fiduciary duty framework: note that ESG integration is increasingly accepted as consistent with fiduciary duties when ESG factors are financially material (per CFA Institute guidance and UNPRI).

**Output**: An ESG integration report containing: (a) ESG scores for each pillar (E, S, G) and composite, (b) industry percentile ranking, (c) screening results (pass/fail for negative screening, best-in-class ranking for positive screening), (d) ESG-adjusted valuation (e.g., "ESG risk discount of 1.2% on WACC reduces implied value from $85 to $78 per share"), (e) key ESG risks and opportunities identified, (f) year-over-year ESG score trend.

**AI Execution Notes**: ESG scores from MSCI (msci.com/esg-ratings), Sustainalytics (sustainalytics.com), or Yahoo Finance's Sustainability tab (provides ESG Risk Ratings from Sustainalytics). Company sustainability reports are typically found on the investor relations page. CDP data at cdp.net. For carbon emission estimates when company-specific data is unavailable, use industry average emissions per dollar of revenue from the EPA or IEA datasets. WACC adjustment magnitudes (0.5-2.0% for poor ESG) are calibrated against academic research showing ESG risk premia in the 50-200bp range across markets.

**Key Formulas** (quick reference):
- ESG-Adjusted WACC = WACC + ESG Risk Premium (0.5-2.0% for poor ESG; -0.25 to -0.75% for strong ESG) (Calibrated against Giese et al., 2019, "Foundations of ESG Investing: How ESG Affects Equity Valuation, Risk, and Performance," Journal of Portfolio Management; and MSCI ESG Research Insight, 2020.)
- ESG Premium/Discount = ESG-Adjusted Share Price - Base DCF Share Price
- Negative Screen Threshold: >5% revenue from excluded activities = exclusion

---

## §3 Cortex Logging

```json
{
  "text": "F-049 ESG Integration — valuation for {company_name}: {result_summary}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "valuation",
    "procedure": "F-049",
    "method": "esg",
    "domain": "finance",
    "level": 3,
    "company": "{company_name}",
    "valuation_result": "{value}",
    "tags": ["business-evaluation", "valuation", "esg", "sustainability", "risk-adjustment"]
  }
}
```

---

## §4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 3 (Valuation)
**WHEN**: When evaluating any investment for ESG compliance or when adjusting a valuation for ESG risk/opportunity
**PREREQUISITES**: F-010 (Balance Sheet Analysis), F-011 (Cash Flow Statement Analysis), F-002/F-003/F-004 (Financial Ratios), F-035 (WACC — baseline before ESG adjustment), F-039 (DCF — base model to be ESG-adjusted)
**FEEDS INTO**: Level 4 (Deal Structuring — ESG due diligence), Level 5 (Portfolio Construction — ESG screening and integration)

**VK**: `✅ [F-049] ESG Integration | {company_name} | Value: {result} | Level 3 complete`

---

## §5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-035 (WACC) | Requires output from | ESG adjustments are applied to the baseline WACC (add/subtract risk premium) |
| F-039 (DCF) | Requires output from | ESG-adjusted valuation modifies the base DCF model's discount rate and/or cash flows |
| F-041 (Trading Comps) | Cross-validates with | Peer ESG scores can explain why some peers trade at premium/discount multiples |
| F-010 (Balance Sheet) | Requires output from | Capital structure and asset data needed for ESG risk quantification |

---
