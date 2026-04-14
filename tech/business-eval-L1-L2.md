---
type: procedure
created: 2026-03-17
status: active
slug: business-eval-l1-l2
tags: [procedure, nexus]
---

# FORGE Procedures — Business Evaluation Training Path (Levels 1 & 2)
# Procedure Count: 25
# Generated: 2026-03-03
# Template: FORGE Standard (Training Variant)

**Note on §5 (Dependencies)**: All procedures in this file include a §5 Dependencies section beyond the FORGE Standard requirement (§1-§4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**PRISM Pipeline**: This file is part of the PRISM Business Evaluation Pipeline. Master procedure: `~/.nexus/procedures/PRISM.md`
**Tools Prerequisites**: Excel/Sheets mastery procedures in `tech/excel-sheets-mastery-batch1.md`, `batch2.md`, `batch3.md`
**Google Sheets**: All Tools-level procedures support dual Excel + Google Sheets paths. Key Sheets functions: GOOGLEFINANCE, QUERY, FILTER, ARRAYFORMULA, Apps Script.

---

## Table of Contents

### Level 1 — Read the Numbers (12 Procedures)
| # | ID | Name | Complexity |
|---|-----|------|-----------|
| 1 | F-010 | Balance Sheet Construction and Review | BASIC |
| 2 | F-011 | Cash Flow Statement Analysis | BASIC |
| 3 | F-012 | Double-Entry Bookkeeping and T-Account Analysis | BASIC |
| 4 | F-002 | Profitability Ratio Analysis | INTERMEDIATE |
| 5 | F-003 | Liquidity Ratio Analysis | INTERMEDIATE |
| 6 | F-004 | Activity Ratio Analysis | INTERMEDIATE |
| 7 | F-033 | Operating Breakeven Analysis | INTERMEDIATE |
| 8 | F-013 | Annual Report / 10-K Reading | INTERMEDIATE |
| 9 | F-017 | Inventory Valuation (FIFO vs LIFO) | BASIC |
| 10 | F-018 | Depreciation Method Analysis | BASIC |
| 11 | F-021 | EPS Analysis | INTERMEDIATE |
| 12 | F-095 | Personal Net Worth Tracking | BASIC |

### Level 2 — Analyze the Business (13 Procedures)
| # | ID | Name | Complexity |
|---|-----|------|-----------|
| 13 | F-001 | DuPont Analysis | ADVANCED |
| 14 | F-014 | Comprehensive Ratio Dashboard | ADVANCED |
| 15 | F-015 | Time Series / Trend Analysis | INTERMEDIATE |
| 16 | F-016 | Comparative / Cross-Sectional Analysis | INTERMEDIATE |
| 17 | F-019 | Leverage and Solvency Analysis | INTERMEDIATE |
| 18 | F-023 | Working Capital Management | INTERMEDIATE |
| 19 | F-034 | Financial Leverage Impact Analysis | ADVANCED |
| 20 | F-022 | Executive Compensation Analysis | INTERMEDIATE |
| 21 | F-006 | Earnings Management Detection | ADVANCED |
| 22 | F-008 | Benford's Law Fraud Screen | ADVANCED |
| 23 | F-009 | Non-Financial KPI Linking | ADVANCED |
| 24 | F-092 | Credit Analysis Framework | ADVANCED |
| 25 | F-093 | Macroeconomic Analysis | INTERMEDIATE |

---
---

# LEVEL 1 — Read the Numbers

---
---

# F-010: Balance Sheet Construction and Review

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-010

---

## S1 Problem

The balance sheet is the foundational snapshot of a company's financial position at a point in time. Without the ability to decompose, verify, and compare balance sheets, an AI evaluator cannot assess asset quality, liability exposure, or equity structure. Skipping this procedure means all downstream ratio analysis and solvency assessments are built on unverified data.

**Situations covered**:
- Initial financial data gathering for any business evaluation
- Detecting concentration risk in asset composition
- Identifying material year-over-year shifts that require deeper investigation
- Verifying data quality before running ratio analysis
- **NOT suitable for**: Companies using non-standard accounting frameworks (e.g., cash-basis accounting) or entities without audited financials where data reliability is unverifiable

---

## S2 Procedure

1. Obtain the target company's most recent balance sheet (from 10-K, annual report, or financial data API). Confirm it follows the accounting equation: Assets = Liabilities + Stockholders' Equity.
2. Decompose current assets in order of liquidity: cash and cash equivalents, short-term investments, accounts receivable, inventory, prepaid expenses. Record each line item value and its percentage of total assets (vertical/common-size analysis).
3. Decompose non-current assets: property, plant & equipment (net of accumulated depreciation), intangible assets, goodwill, long-term investments. Flag any single non-current asset exceeding 40% of total assets as a concentration risk.
4. Decompose current liabilities: accounts payable, short-term debt, accrued expenses, current portion of long-term debt. Compare total current liabilities to total current assets to get a preliminary working capital figure.
5. Decompose non-current liabilities: long-term debt, deferred tax liabilities, pension obligations. Note the debt maturity profile if disclosed in footnotes.
6. Decompose stockholders' equity: common stock at par, additional paid-in capital, retained earnings, treasury stock, accumulated other comprehensive income. Calculate the book value per share = Total Equity / Shares Outstanding.
7. Verify the balance sheet balances (Assets = L + E). If there is a discrepancy, flag data quality issues.
8. Compare the current year balance sheet to prior year. Calculate year-over-year dollar changes and percentage changes for every major line item to identify material shifts.
9. Read footnotes for off-balance-sheet items: operating leases (pre-IFRS 16/ASC 842), contingent liabilities, variable interest entities. Flag any material off-balance-sheet exposure.

**Output**: A structured balance sheet summary table with: (a) line items, (b) absolute values for 2 years, (c) YoY change %, (d) common-size percentages, (e) flagged items requiring deeper investigation.

**AI Execution Notes**: Pull data from SEC EDGAR (10-K filings), Yahoo Finance API, or Macrotrends. For private companies, request the balance sheet directly. Use vertical analysis (each item as % of total assets) automatically. Flag any line item that changed >20% YoY for follow-up.

---

## S3 Cortex Logging

```json
{
  "text": "F-010 Balance Sheet Construction and Review — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-010",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "accounting", "balance-sheet", "financial-statements"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: None — foundational procedure
**FEEDS INTO**: F-002 (Profitability Ratios), F-003 (Liquidity Ratios), F-004 (Activity Ratios), F-013 (10-K Reading), F-019 (Leverage/Solvency), F-021 (EPS), F-023 (Working Capital), F-001 (DuPont), F-014 (Dashboard)

**VK**: `[F-010] Balance Sheet Construction and Review | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002 | Feeds into | Balance sheet provides Total Assets and Equity for ROA and ROE calculations |
| F-003 | Feeds into | Current assets and current liabilities needed for liquidity ratios |
| F-004 | Feeds into | AR, Inventory, AP, Total Assets needed for activity ratios |
| F-013 | Feeds into | Balance sheet cross-referenced during 10-K reading |
| F-019 | Feeds into | Debt and equity figures needed for leverage ratios |
| F-001 | Feeds into | Average Assets and Average Equity needed for DuPont decomposition |

---
---

# F-011: Cash Flow Statement Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-011

---

## S1 Problem

Cash flow reveals whether a company's reported profits translate into actual cash generation. Without analyzing the cash flow statement, an AI evaluator could be misled by accrual-based earnings that mask cash burn, aggressive revenue recognition, or unsustainable capital spending. Skipping this procedure leaves earnings quality unverified.

**Situations covered**:
- Verifying that reported net income is backed by operating cash flow
- Calculating Free Cash Flow for valuation models
- Classifying a company's lifecycle stage by cash flow pattern
- Detecting divergence between earnings and cash (earnings quality concern)
- **NOT suitable for**: Entities that do not prepare a statement of cash flows (e.g., very small private businesses using cash-basis accounting) or when only a single period is available (trend analysis requires 2+ periods)

---

## S2 Procedure

1. Obtain the statement of cash flows for at least 2 years. Identify the method used (direct or indirect; most companies use indirect).
2. For Operating Activities (indirect method): start with net income, add back non-cash charges (depreciation & amortization), then analyze working capital adjustments. Apply the rules: asset increase = cash outflow; asset decrease = cash inflow; liability increase = cash inflow; liability decrease = cash outflow.
3. Calculate Cash Flow from Operations (CFO). Compare CFO to net income. Flag if CFO < Net Income persistently — this suggests accrual earnings exceed cash earnings (potential earnings quality concern).
4. For Investing Activities: identify capital expenditures (CapEx), acquisitions, and asset disposals. Calculate Free Cash Flow = CFO - CapEx. Flag if CapEx consistently exceeds CFO (company burning cash on investments).
5. For Financing Activities: identify debt issuances/repayments, equity issuances/buybacks, and dividend payments. Determine if the company is a net borrower or net repayer.
6. Calculate the net change in cash and verify it reconciles: Beginning Cash + Net Change = Ending Cash (must match balance sheet cash).
7. Compute the CFO-to-Net-Income ratio, CFO-to-Debt ratio, and Free Cash Flow Yield (FCF / Market Cap) to assess cash generation quality.
8. Analyze the cash flow pattern: (+Operating, -Investing, -Financing) = healthy mature company; (-Operating, +Financing) = startup or distressed; (+Operating, +Financing, -Investing) = growth phase with external funding.

**Output**: Cash flow summary with: (a) 3-section breakdown with 2-year comparison, (b) FCF calculation, (c) CFO-to-NI quality ratio, (d) cash flow pattern classification, (e) red flags.

**AI Execution Notes**: Parse cash flow data from financial APIs. The critical check is CFO vs Net Income divergence — if net income grows while CFO stagnates, flag aggressive accrual accounting. Use EBITDA minus CapEx as a secondary FCF approximation when full data is unavailable.

---

## S3 Cortex Logging

```json
{
  "text": "F-011 Cash Flow Statement Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-011",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "accounting", "cash-flow", "earnings-quality"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: None — foundational procedure (but benefits from F-010 balance sheet data for reconciliation)
**FEEDS INTO**: F-002 (Profitability — ROA uses NI), F-006 (Earnings Management — CFO vs NI), F-013 (10-K Reading), F-014 (Dashboard), F-092 (Credit Analysis — CFO/Debt)

**VK**: `[F-011] Cash Flow Statement Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Complements | Cash reconciliation requires balance sheet cash figures |
| F-006 | Feeds into | CFO vs NI divergence is the primary earnings management screen |
| F-092 | Feeds into | CFO-to-Debt ratio is a key credit analysis metric |
| F-014 | Feeds into | FCF and cash ratios feed into comprehensive dashboard |

---
---

# F-012: Double-Entry Bookkeeping and T-Account Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-012

---

## S1 Problem

Double-entry bookkeeping is the logic layer beneath all financial statements. Without understanding how transactions flow through debits and credits, an AI evaluator cannot verify whether reported entries follow proper accounting logic, explain financial impacts to users, or detect entries that violate the accounting equation. Skipping this procedure undermines the ability to audit transaction-level data.

**Situations covered**:
- Explaining how a specific transaction affects the financial statements
- Verifying that company-reported entries follow proper accounting logic
- Supporting earnings management detection (F-006) with transaction-level verification
- Educational walkthroughs when users ask "how does X affect the books?"
- **NOT suitable for**: Automated reconciliation of high-volume transaction systems (requires specialized audit software); this procedure is for understanding and verifying the logic, not for processing thousands of entries

---

## S2 Procedure

1. Identify the transaction or set of transactions to analyze. For each transaction, determine the minimum two accounts affected (double-entry principle: every debit must have an equal credit).
2. Classify each account as Asset, Liability, Equity, Revenue, or Expense. Apply the rules: Assets and Expenses increase with debits; Liabilities, Equity, and Revenue increase with credits.
3. Draw T-accounts for each affected account. Post the debit on the left side and credit on the right side. Verify the accounting equation (A = L + E) remains balanced after each entry.
4. For complex transactions (e.g., purchasing equipment with partial cash and partial loan), trace the entry through all affected accounts: debit Equipment (asset up), credit Cash (asset down), credit Notes Payable (liability up).
5. Calculate running balances for each T-account and verify total debits equal total credits across all accounts.
6. Map the T-account balances back to the three financial statements: balance sheet accounts (A, L, E) and income statement accounts (R, E). Confirm the trial balance ties out.
7. Flag any entries where the accounting equation does not balance — these indicate recording errors or potential manipulation.

**Output**: A T-account map showing (a) each transaction with debit/credit entries, (b) running balances, (c) trial balance verification, (d) mapping to financial statements. Useful when Genie needs to verify a company's transaction logic or explain bookkeeping to the user.

**AI Execution Notes**: This procedure is primarily educational/advisory. When a user asks "how does this transaction affect the financials," Genie walks through the double-entry logic. For actual business evaluation, this procedure supports F-006 (Earnings Management Detection) by verifying that reported entries follow proper accounting logic.

---

## S3 Cortex Logging

```json
{
  "text": "F-012 Double-Entry Bookkeeping and T-Account Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-012",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "accounting", "bookkeeping", "double-entry"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When verifying transaction logic or explaining accounting entries
**PREREQUISITES**: None — foundational procedure
**FEEDS INTO**: F-006 (Earnings Management Detection — verifies accounting logic), F-010 (Balance Sheet — confirms equation balance)

**VK**: `[F-012] Double-Entry Bookkeeping and T-Account Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-006 | Feeds into | Transaction-level verification supports earnings manipulation detection |
| F-010 | Complements | Accounting equation verification underlies balance sheet integrity |

---
---

# F-002: Profitability Ratio Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-002

---

## S1 Problem

Profitability ratios reveal whether a company converts revenue into profit efficiently and generates adequate returns on its assets and equity. Without profitability analysis, an AI evaluator cannot determine if a business is fundamentally profitable, identify its competitive strategy (cost leadership vs. differentiation), or benchmark performance against peers. Skipping this procedure means evaluating a business without knowing if it actually makes money.

**Situations covered**:
- Assessing whether a company's margins are expanding or contracting
- Determining if the company follows a differentiation or cost-leadership strategy
- Benchmarking profitability against competitors and industry averages
- Feeding ROA/ROE into DuPont decomposition (F-001)
- **NOT suitable for**: Comparing companies across different accounting standards (GAAP vs IFRS) without adjustments, or financial institutions where standard margin ratios have different meanings

---

## S2 Procedure

1. Gather income statement data: Revenue, COGS, Gross Profit, Operating Expenses (SGA), Operating Income (EBIT), Interest Expense, Net Income, and Preferred Dividends (if any). Gather balance sheet data: Total Assets (current + prior year for averaging), Stockholders' Equity (current + prior year).
2. Calculate Gross Profit Margin = Gross Profit / Revenue. This reveals the basic markup and pricing power. A declining GPM signals pricing pressure or rising input costs.
3. Calculate Operating Profit Margin (EBIT Margin) = EBIT / Revenue. This captures operational efficiency after SGA expenses. Compare to GPM — a large gap indicates high operating overhead.
4. Calculate Net Profit Margin = Net Income / Revenue. This is the bottom-line efficiency. Compare to EBIT margin to isolate the impact of interest and taxes.
5. Calculate Return on Assets (ROA) = (Net Income + Interest Expense) / Average Total Assets. Use EBIT / Average Assets for the pre-financing version. This measures how effectively management utilizes resources.
   Note: The textbook-correct ROA formula adjusting for financing is ROA = (Net Income + Interest Expense × (1 - Tax Rate)) / Average Total Assets. The EBIT / Average Assets variant is also widely accepted but produces different results when tax rates vary.
6. Decompose ROA into its two drivers: Profit Margin (EBIT/Sales) x Asset Turnover (Sales/Average Assets). Determine if the company follows a differentiation strategy (high margin, lower turnover — like Whole Foods) or cost leadership (low margin, high turnover — like Dollar General).
7. Calculate Return on Equity (ROE) = (Net Income - Preferred Dividends) / Average Common Stockholders' Equity. This measures the return for equity investors.
8. Compare all ratios to: (a) prior year (trend), (b) main competitor, (c) industry average. Flag any ratio that deviates >2 standard deviations from industry median.

**Output**: Profitability ratio dashboard with 6 ratios, 2-year trends, peer comparison, and a narrative summary identifying the company's strategy type (differentiation vs. cost leader) and profitability trajectory.

**AI Execution Notes**: Pull financial data from APIs (Yahoo Finance, Macrotrends, SEC EDGAR). For industry benchmarks, use Damodaran's dataset or sector ETF financials as proxy. Always use average balance sheet figures (average of beginning and ending year) when pairing with income statement items.

---

## S3 Cortex Logging

```json
{
  "text": "F-002 Profitability Ratio Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-002",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "ratio-analysis", "profitability", "margins"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: F-010 (Balance Sheet — provides Total Assets, Equity), F-011 (Cash Flow — provides NI context)
**FEEDS INTO**: F-001 (DuPont Analysis — uses ROA, ROE, margins), F-014 (Comprehensive Dashboard), F-016 (Cross-Sectional Analysis)

**VK**: `[F-002] Profitability Ratio Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Average Total Assets and Average Equity from balance sheet |
| F-001 | Feeds into | ROA decomposition and ROE are DuPont inputs |
| F-014 | Feeds into | Profitability ratios populate the comprehensive dashboard |
| F-016 | Feeds into | Peer comparison uses profitability benchmarks |

---
---

# F-003: Liquidity Ratio Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-003

---

## S1 Problem

Liquidity ratios measure whether a company can meet its short-term obligations without raising external capital. Without liquidity analysis, an AI evaluator cannot assess whether a business has the cash runway to survive, whether working capital is adequate, or how quickly the company converts assets to cash. Skipping this procedure means ignoring the single most common cause of business failure: running out of cash.

**Situations covered**:
- Assessing short-term solvency risk for any business
- Calculating cash runway for pre-revenue or loss-making companies
- Evaluating the efficiency of the cash conversion cycle
- Determining if negative working capital is a strength (like Walmart) or a danger signal
- **NOT suitable for**: Financial institutions (banks, insurance) where current ratio is meaningless due to liability structure, or companies with significant off-balance-sheet liquidity facilities

---

## S2 Procedure

1. Gather current assets breakdown: Cash, Marketable Securities, Accounts Receivable, Inventory, Prepaid Expenses. Gather current liabilities total and breakdown.
2. Calculate Current Ratio = Current Assets / Current Liabilities. A ratio >1.0 indicates short-term solvency. Flag if <1.0 but investigate context (e.g., Walmart operates below 1.0 due to efficient supply chain and favorable payment terms).
3. Calculate Quick Ratio (Acid Test) = (Cash + Short-term Investments + Accounts Receivable) / Current Liabilities. This excludes inventory and prepaid expenses — assets that cannot be quickly liquidated. Flag if Quick Ratio < 0.5.
4. Calculate Cash Ratio = (Cash + Cash Equivalents) / Current Liabilities. This is the most conservative liquidity measure — ability to pay obligations with cash on hand only.
5. Calculate Net Working Capital = Current Assets - Current Liabilities. Determine if NWC is positive (buffer exists) or negative (potential distress).
6. Calculate the Operating Cash Conversion Cycle: Days Sales Outstanding (DSO) + Days Inventory Outstanding (DIO) - Days Payable Outstanding (DPO). A shorter cycle means faster cash conversion.
7. Compare all liquidity ratios to industry benchmarks. A retailer with Current Ratio of 0.9 may be healthy, while a manufacturer with the same ratio may be distressed. Context is critical.
8. Assess cash burn rate if the company is pre-revenue or loss-making: Cash / Monthly Operating Cash Burn = Runway in months. Flag if runway < 12 months.

**Output**: Liquidity assessment with: (a) 4 liquidity ratios + 2-year trend, (b) cash conversion cycle, (c) industry comparison, (d) runway estimate if applicable, (e) risk rating (Low/Medium/High).

**AI Execution Notes**: Use the corporate-finance-course S02 framework distinguishing current ratio from quick ratio. For the cash conversion cycle, pull DSO/DIO/DPO from financial data APIs or calculate from AR/Inventory/AP turnover ratios. Flag companies with negative working capital AND negative operating cash flow as HIGH RISK.

---

## S3 Cortex Logging

```json
{
  "text": "F-003 Liquidity Ratio Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-003",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "ratio-analysis", "liquidity", "working-capital"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: F-010 (Balance Sheet — provides current assets/liabilities breakdown)
**FEEDS INTO**: F-014 (Comprehensive Dashboard), F-023 (Working Capital Management), F-092 (Credit Analysis)

**VK**: `[F-003] Liquidity Ratio Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Current assets and current liabilities from balance sheet |
| F-004 | Complements | DSO/DIO/DPO from activity ratios feed into CCC calculation |
| F-014 | Feeds into | Liquidity ratios populate the comprehensive dashboard |
| F-023 | Feeds into | Liquidity assessment is the starting point for working capital management |
| F-092 | Feeds into | Current ratio and cash ratio are key credit analysis inputs |

---
---

# F-004: Activity Ratio Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-004

---

## S1 Problem

Activity ratios measure how efficiently a company uses its assets to generate revenue. Without this analysis, an AI evaluator cannot determine if inventory is stagnating, receivables are being collected on time, or assets are being utilized productively. Skipping this procedure hides operational inefficiencies that directly erode profitability and cash flow.

**Situations covered**:
- Evaluating operational efficiency of asset utilization
- Calculating the Cash Conversion Cycle to assess cash flow timing
- Detecting deteriorating collection or inventory management trends
- Benchmarking operational efficiency against competitors
- **NOT suitable for**: Service companies with minimal inventory or receivables, where activity ratios are not meaningful performance indicators

---

## S2 Procedure

1. Gather from the income statement: Revenue (Net Sales), Cost of Goods Sold. From the balance sheet: Accounts Receivable (current + prior year), Inventory (current + prior year), Accounts Payable (current + prior year), Total Assets (current + prior year).
2. Calculate Accounts Receivable Turnover = Net Credit Sales / Average Accounts Receivable. Convert to Days Sales Outstanding (DSO) = 365 / AR Turnover. DSO >40 days for most industries suggests slow collection; benchmark is typically 30-45 days.
3. Calculate Inventory Turnover = COGS / Average Inventory. Convert to Days Inventory Outstanding (DIO) = 365 / Inventory Turnover. Higher turnover = better inventory management. Context matters: a grocery store should have DIO <30 days; a luxury goods maker may have DIO >180 days.
4. Calculate Accounts Payable Turnover = COGS / Average Accounts Payable. Convert to Days Payable Outstanding (DPO) = 365 / AP Turnover. Higher DPO means the company takes longer to pay suppliers — good for cash flow but must not violate credit terms.
5. Calculate Total Asset Turnover = Revenue / Average Total Assets. This measures overall efficiency of asset utilization. A ratio >1.0 is common in retail; <0.5 is typical for capital-intensive industries.
6. Calculate Fixed Asset Turnover = Revenue / Average Net Fixed Assets. This isolates the productivity of PP&E investments.
7. Compute the Cash Conversion Cycle (CCC) = DSO + DIO - DPO. A shorter CCC means the company converts its resource investments into cash faster. Negative CCC (like Amazon) means the company collects from customers before paying suppliers.
8. Compare each activity ratio against: (a) the company's own prior years (trend), (b) top competitor, (c) industry median. Flag deteriorating trends (rising DSO, rising DIO, shrinking DPO).

**Output**: Activity ratio table with 6 ratios + CCC, 2-year trends, peer benchmarks, and narrative identifying operational efficiency strengths/weaknesses.

**AI Execution Notes**: All activity ratios require averaging the balance sheet items (beginning + ending / 2) since income statement items cover a period. For companies without credit sales disclosure, use total revenue as the numerator for AR turnover. Flag any CCC that increased by >15 days YoY.

---

## S3 Cortex Logging

```json
{
  "text": "F-004 Activity Ratio Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-004",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "ratio-analysis", "activity", "efficiency", "cash-conversion-cycle"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: F-010 (Balance Sheet — provides AR, Inventory, AP, Total Assets), F-011 (Cash Flow — provides context for CCC interpretation)
**FEEDS INTO**: F-001 (DuPont — Asset Turnover component), F-003 (Liquidity — CCC), F-014 (Comprehensive Dashboard), F-023 (Working Capital Management)

**VK**: `[F-004] Activity Ratio Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | AR, Inventory, AP, Total Assets from balance sheet |
| F-001 | Feeds into | Asset Turnover is a DuPont component |
| F-003 | Complements | CCC components (DSO/DIO/DPO) shared with liquidity analysis |
| F-014 | Feeds into | Activity ratios populate the comprehensive dashboard |
| F-023 | Feeds into | DSO/DIO/DPO drive working capital management assessment |

---
---

# F-033: Operating Breakeven Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-033

---

## S1 Problem

Breakeven analysis determines the minimum revenue a company needs to cover its costs, revealing how much cushion exists before losses begin. Without this analysis, an AI evaluator cannot quantify downside risk, assess how sensitive profits are to revenue changes, or advise on cost structure optimization. Skipping this procedure means evaluating a business without understanding its margin of safety.

**Situations covered**:
- Determining the revenue level at which a company starts making money
- Assessing vulnerability to revenue declines (margin of safety)
- Quantifying operating leverage and its risk implications
- Scenario modeling for revenue sensitivity analysis
- **NOT suitable for**: Multi-product companies without reliable cost allocation between fixed and variable, or businesses with significant step-function costs that make linear breakeven assumptions invalid

---

## S2 Procedure

1. Classify the company's cost structure into fixed costs and variable costs. Use the contribution margin income statement format: Revenue - Variable Costs = Contribution Margin - Fixed Costs = Operating Income.
2. If the company does not disclose fixed vs. variable costs, estimate them: COGS is often mostly variable; SGA has both fixed (rent, salaries) and variable (commissions, shipping) components. Use the high-low method or regression on historical data to separate fixed from variable.
3. Calculate the Contribution Margin per Unit = Price per Unit - Variable Cost per Unit. If unit data is unavailable, use the Contribution Margin Ratio = (Revenue - Total Variable Costs) / Revenue.
4. Calculate the Operating Breakeven Point in units = Fixed Operating Costs / Contribution Margin per Unit. This excludes interest expense (financial fixed costs). For the full breakeven including financial costs: (Fixed Operating Costs + Fixed Financial Costs) / Contribution Margin per Unit.
5. Calculate the Breakeven Point in revenue dollars = Fixed Operating Costs / Contribution Margin Ratio.
6. Calculate Margin of Safety = Actual Revenue - Breakeven Revenue. Express as a ratio: Margin of Safety Ratio = (Actual Revenue - Breakeven Revenue) / Actual Revenue. A higher ratio means more cushion against downturns.
7. Calculate Degree of Operating Leverage (DOL) = Contribution Margin / Operating Income. DOL measures sensitivity of operating income to sales changes. A DOL of 3 means a 10% sales increase produces a 30% operating income increase (and vice versa for declines).
8. Assess risk: compare the Margin of Safety Ratio and DOL together. High DOL with low Margin of Safety = HIGH RISK (small sales decline wipes out profits). Low DOL with high Margin of Safety = LOW RISK.

**Output**: Breakeven analysis report with: (a) cost structure classification, (b) BEP in units and dollars, (c) margin of safety (absolute and ratio), (d) DOL, (e) risk classification, (f) scenario table showing impact of +/- 10% and 20% revenue changes on operating income.

**AI Execution Notes**: For companies that do not disclose unit economics, estimate the contribution margin ratio from the gross profit margin (assuming COGS is primarily variable). Use regression on quarterly data (revenue vs. total costs) to estimate the fixed cost base. The breakeven formula from corporate-finance-course S06: Q_BE = Fixed Operating Costs / (P - V).

**Tools prerequisite**: F-086 (Goal Seek) for finding the exact breakeven revenue or unit volume where operating income = 0. Google Sheets: no native Goal Seek — use Solver add-on or Apps Script bisection method (see F-086).

---

## S3 Cortex Logging

```json
{
  "text": "F-033 Operating Breakeven Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-033",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "cost-analysis", "breakeven", "operating-leverage"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any business's financial health
**PREREQUISITES**: F-002 (Profitability Ratios — provides margin data), F-010 (Balance Sheet — cost structure context)
**FEEDS INTO**: F-034 (Financial Leverage Impact — DOL is a key input), F-014 (Comprehensive Dashboard — risk metrics)

**VK**: `[F-033] Operating Breakeven Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002 | Requires output from | Gross margin and operating margin inform cost structure estimation |
| F-034 | Feeds into | DOL from breakeven feeds into total leverage analysis |
| F-014 | Feeds into | Margin of safety and DOL are dashboard risk metrics |

---
---

# F-013: Annual Report / 10-K Reading

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-013

---

## S1 Problem

The 10-K annual report is the single most information-dense document for evaluating a public company. Without the ability to systematically extract and synthesize information from a 10-K, an AI evaluator misses critical context: risk factors, accounting policies, auditor opinions, debt schedules, and management commentary that cannot be found in financial data APIs alone. Skipping this procedure means relying on numbers without the narrative that explains them.

**Situations covered**:
- Initial deep-dive on a public company being evaluated
- Extracting risk factors and management commentary for qualitative assessment
- Verifying accounting policies that affect reported numbers
- Identifying red flags in auditor opinions or related party transactions
- **NOT suitable for**: Non-US companies that do not file with the SEC (use local equivalents: UK Annual Report, EU ESMA filings), or private companies without public disclosure obligations

---

## S2 Procedure

1. Locate the company's most recent 10-K filing on SEC EDGAR (for US public companies) or the annual report from the investor relations page. Note the fiscal year end date.
2. Read the Business Description (Item 1): identify the company's products/services, key markets, competitive landscape, and risk factors. Extract the top 3-5 risk factors that could materially impact future performance.
3. Read Management Discussion and Analysis (MD&A, Item 7): focus on management's explanation of revenue drivers, cost trends, capital allocation priorities, and forward-looking statements. Flag any hedging language ("may," "could," "potential") around negative trends.
4. Extract the three core financial statements: Balance Sheet, Income Statement, Cash Flow Statement. Cross-reference numbers against what you computed in F-010, F-011, F-002.
5. Read the Notes to Financial Statements: focus on (a) accounting policies (revenue recognition method, depreciation method, inventory valuation method), (b) segment reporting, (c) debt schedule and maturities, (d) contingent liabilities and legal proceedings, (e) related party transactions.
6. Check the auditor's opinion (typically from a Big 4 firm). Flag if it is anything other than an unqualified ("clean") opinion. A going concern qualification is a major red flag.
7. Review executive compensation disclosures (proxy statement / DEF 14A): note total CEO compensation, the ratio of performance-based to fixed compensation, and any unusual stock option grants.
8. Review the company's segment data: identify which business segments drive the most revenue and profit. Flag segments with declining margins.

**Output**: A structured 10-K digest with: (a) business model summary, (b) top risk factors, (c) MD&A key themes, (d) accounting policy summary, (e) debt maturity profile, (f) auditor opinion status, (g) key segment breakdown, (h) red flags list.

**AI Execution Notes**: Use SEC EDGAR full-text search or the company's IR page. For non-US companies, use the equivalent annual filing (20-F for foreign filers in US, annual report for EU companies). The 10-K is the single most information-dense document for business evaluation.

---

## S3 Cortex Logging

```json
{
  "text": "F-013 Annual Report / 10-K Reading — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-013",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "financial-statements", "10-K", "annual-report", "qualitative-analysis"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any US public company (or equivalent for international)
**PREREQUISITES**: F-010 (Balance Sheet), F-011 (Cash Flow) — for cross-referencing financial statement data
**FEEDS INTO**: F-006 (Earnings Management — accounting policies and auditor opinion), F-017 (Inventory Valuation — policy from footnotes), F-018 (Depreciation — policy from footnotes), F-019 (Leverage — debt schedule from footnotes), F-022 (Executive Compensation), F-092 (Credit Analysis — covenants)

**VK**: `[F-013] Annual Report / 10-K Reading | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Balance sheet data needed for cross-referencing |
| F-011 | Requires output from | Cash flow data needed for cross-referencing |
| F-006 | Feeds into | Accounting policies and auditor opinion inform manipulation detection |
| F-017 | Feeds into | Inventory valuation method disclosed in 10-K footnotes |
| F-018 | Feeds into | Depreciation method disclosed in 10-K footnotes |
| F-019 | Feeds into | Debt schedule and maturities from 10-K footnotes |
| F-022 | Feeds into | Compensation data from proxy statement referenced in 10-K |

---
---

# F-017: Inventory Valuation (FIFO vs LIFO)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-017

---

## S1 Problem

Different inventory valuation methods (FIFO, LIFO, weighted average) produce materially different reported profits, asset values, and tax liabilities for the same physical inventory. Without the ability to adjust for inventory method differences, an AI evaluator cannot make valid cross-company comparisons, detect method-change-driven earnings manipulation, or assess the true tax impact. Skipping this procedure introduces a systematic bias in any comparative analysis.

**Situations covered**:
- Comparing companies that use different inventory valuation methods
- Adjusting LIFO-based financials to FIFO-equivalent for apples-to-apples comparison
- Assessing the tax impact of inventory valuation choices
- Detecting earnings management through inventory method changes
- **NOT suitable for**: Service businesses with no physical inventory, or companies using specialized inventory methods (e.g., specific identification for luxury goods, precious metals)

---

## S2 Procedure

1. Identify the company's disclosed inventory valuation method from the Notes to Financial Statements: FIFO (First-In, First-Out), LIFO (Last-In, First-Out), Weighted Average Cost, or Specific Identification.
2. Understand the impact on financials: In an inflationary environment, LIFO results in higher COGS (newest, more expensive items sold first) and lower reported profit, while FIFO results in lower COGS and higher reported profit. The balance sheet effect is opposite: LIFO understates inventory value; FIFO reflects more current market values.
3. If the company uses LIFO, extract the LIFO Reserve from the footnotes. Calculate FIFO Inventory = LIFO Inventory + LIFO Reserve. This adjustment enables apples-to-apples comparison with FIFO companies.
4. Recalculate adjusted COGS: FIFO COGS = LIFO COGS - Change in LIFO Reserve (year-over-year). This gives the COGS figure as if the company used FIFO.
5. Assess the tax impact: LIFO provides a tax benefit in inflationary periods (lower taxable income). Calculate the tax savings = Change in LIFO Reserve x Tax Rate.
6. Compare inventory turnover using both the reported and adjusted figures. Determine if the inventory valuation method materially affects the company's apparent efficiency.
7. Flag if the company changed its inventory method (this requires disclosure and may signal earnings management).

**Output**: Inventory valuation analysis with: (a) method identified, (b) LIFO reserve adjustment (if applicable), (c) FIFO-equivalent inventory and COGS, (d) tax impact, (e) adjusted inventory turnover, (f) cross-company comparability notes.

**AI Execution Notes**: LIFO is only permitted under US GAAP (prohibited under IFRS). When comparing US companies (potential LIFO) to international companies (FIFO/weighted average), always adjust using the LIFO reserve. Pull LIFO reserve data from 10-K footnotes. Note: LIFO conformity rule requires companies using LIFO for tax to also use it for financial reporting.

---

## S3 Cortex Logging

```json
{
  "text": "F-017 Inventory Valuation (FIFO vs LIFO) — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-017",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "accounting", "inventory-valuation", "FIFO", "LIFO"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating companies with significant inventory or comparing cross-method companies
**PREREQUISITES**: F-010 (Balance Sheet — inventory figures), F-013 (10-K — inventory method from footnotes)
**FEEDS INTO**: F-004 (Activity Ratios — adjusted inventory turnover), F-006 (Earnings Management — method changes as manipulation signal), F-016 (Cross-Sectional Analysis — comparability adjustments)

**VK**: `[F-017] Inventory Valuation (FIFO vs LIFO) | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Inventory line items from balance sheet |
| F-013 | Requires output from | Inventory valuation method from 10-K footnotes |
| F-004 | Feeds into | Adjusted inventory turnover for accurate efficiency analysis |
| F-006 | Feeds into | Method changes can signal earnings manipulation |
| F-016 | Feeds into | Cross-company comparisons require method-adjusted figures |

---
---

# F-018: Depreciation Method Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-018

---

## S1 Problem

Depreciation methods and useful life assumptions directly affect reported profits, asset values, and tax obligations. Without analyzing depreciation choices, an AI evaluator cannot detect aggressive assumptions that inflate earnings, assess the age of a company's asset base, or make valid comparisons between companies using different methods. Skipping this procedure allows earnings management through depreciation to go undetected.

**Situations covered**:
- Assessing whether depreciation assumptions are reasonable or aggressive
- Comparing companies that use different depreciation methods
- Calculating average asset age to assess capital expenditure needs
- Detecting earnings management through useful life changes
- **NOT suitable for**: Intangible assets (use amortization procedures instead), or land (which is not depreciated), or biological assets under IAS 41

---

## S2 Procedure

1. Identify the depreciation methods used by the company from the Notes to Financial Statements. Common methods: straight-line, activity-based (units of production), and accelerated (double-declining balance).
2. For straight-line: Annual Depreciation = (Cost - Salvage Value) / Useful Life. This spreads cost evenly. Verify the useful life assumptions are reasonable for the asset class (e.g., vehicles 5-8 years, buildings 20-40 years, machinery 10-15 years).
3. For activity-based: Depreciation per Unit = (Cost - Salvage Value) / Total Estimated Units. Annual Depreciation = Depreciation per Unit x Units Produced This Year. This aligns expense with actual usage.
4. For accelerated methods (double-declining balance): Year 1 Depreciation = (2 / Useful Life) x Book Value. Each subsequent year applies the same rate to the declining book value. This front-loads expenses, reducing early-year profits.
5. Assess the impact on comparability: A company using straight-line will report higher early-year profits than an identical company using accelerated depreciation. When comparing companies, note the depreciation method and adjust if needed.
6. Calculate the Average Asset Age = Accumulated Depreciation / Annual Depreciation Expense. This reveals whether the company's asset base is aging (high ratio) or recently refreshed (low ratio).
7. Calculate the Average Useful Life = Gross PP&E / Annual Depreciation Expense. Compare to industry norms. Unusually long useful lives inflate profits (lower annual depreciation charge) — a potential earnings management signal.
8. Flag if the company changed depreciation methods or revised useful life estimates (required disclosure). Any change affects comparability with prior periods.

**Output**: Depreciation analysis with: (a) method identification, (b) key assumptions (useful lives, salvage values), (c) average asset age and useful life calculations, (d) impact on reported profits vs. alternative methods, (e) flags for aggressive assumptions.

**AI Execution Notes**: Compare the company's average useful life assumptions against peers. If a company depreciates buildings over 50 years while peers use 30 years, the company reports higher profits. This is a common earnings management lever detected by comparing gross PP&E growth vs. depreciation expense growth.

---

## S3 Cortex Logging

```json
{
  "text": "F-018 Depreciation Method Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-018",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "accounting", "depreciation", "asset-management"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating companies with significant PP&E or comparing asset-heavy businesses
**PREREQUISITES**: F-010 (Balance Sheet — PP&E figures), F-013 (10-K — depreciation method from footnotes)
**FEEDS INTO**: F-006 (Earnings Management — aggressive useful life assumptions), F-016 (Cross-Sectional Analysis — method-adjusted comparisons)

**VK**: `[F-018] Depreciation Method Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | PP&E and accumulated depreciation from balance sheet |
| F-013 | Requires output from | Depreciation method and useful life from 10-K footnotes |
| F-006 | Feeds into | Unusual useful life assumptions signal earnings manipulation |
| F-016 | Feeds into | Cross-company comparisons require method awareness |

---
---

# F-021: EPS Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-021

---

## S1 Problem

Earnings Per Share is the most watched metric by equity investors and analysts. Without understanding basic vs. diluted EPS, the impact of share buybacks, and the potential for EPS manipulation, an AI evaluator cannot properly assess shareholder returns, detect cosmetic earnings growth through buybacks, or flag suspiciously consistent consensus beats. Skipping this procedure means missing the metric that drives most stock price movements.

**Situations covered**:
- Calculating and interpreting basic and diluted EPS
- Detecting buyback-driven EPS growth masking declining earnings
- Assessing dilution risk from convertible securities and stock options
- Comparing reported EPS to analyst consensus for manipulation patterns
- **NOT suitable for**: Private companies without publicly traded shares (no market price for P/E), or companies with complex capital structures requiring specialized dilution calculations beyond treasury stock method

---

## S2 Procedure

1. Determine if the company has a simple or complex capital structure. Simple = only common stock outstanding. Complex = convertible bonds, convertible preferred stock, stock options, or warrants exist.
2. Calculate Basic EPS = (Net Income - Preferred Dividends) / Weighted Average Common Shares Outstanding. For the weighted average, weight each tranche of shares by the fraction of the year they were outstanding.
3. If complex capital structure, calculate Diluted EPS using the if-converted method for convertible securities and the treasury stock method for options/warrants:
   - For convertible bonds: add back after-tax interest expense to numerator; add converted shares to denominator.
   - For convertible preferred: add back preferred dividends to numerator; add converted shares to denominator.
   - For options/warrants (treasury stock method): assume exercise, calculate proceeds, assume buyback at average market price, add only the net new shares to denominator.
4. Include only dilutive securities (those that reduce EPS). Anti-dilutive securities (that would increase EPS) are excluded from the diluted calculation.
5. Compare Basic EPS vs. Diluted EPS. A large gap indicates significant potential dilution from convertible instruments or options. Flag if diluted EPS is >10% lower than basic EPS.
6. Analyze EPS trends over 3-5 years. Separate organic EPS growth (from earnings growth) from buyback-driven EPS growth (from share count reduction). Calculate: Organic EPS Growth = Earnings Growth Rate; Buyback Effect = % Reduction in Share Count.
7. Compare reported EPS against analyst consensus estimates. A consistent pattern of "beating by one cent" may indicate earnings management.

**Output**: EPS analysis with: (a) basic and diluted EPS for 2-3 years, (b) dilution gap analysis, (c) decomposition into earnings growth vs. share count changes, (d) comparison to consensus estimates, (e) flags for potential manipulation.

**AI Execution Notes**: Pull EPS data from financial APIs. For the dilution analysis, examine the options and convertibles disclosures in the 10-K footnotes. The key insight is that share buybacks can mask declining earnings: if net income drops 5% but shares outstanding drop 8%, EPS still rises — this is not operational improvement.

---

## S3 Cortex Logging

```json
{
  "text": "F-021 EPS Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-021",
    "domain": "finance",
    "level": 1,
    "company": "{company_name}",
    "tags": ["business-evaluation", "valuation", "EPS", "dilution", "share-buyback"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When evaluating any publicly traded company
**PREREQUISITES**: F-010 (Balance Sheet — equity structure), F-002 (Profitability — Net Income), F-013 (10-K — convertible securities and options from footnotes)
**FEEDS INTO**: F-001 (DuPont — ROE context), F-006 (Earnings Management — consensus beat patterns), F-014 (Dashboard — P/E ratio requires EPS), F-022 (Executive Compensation — stock option dilution)

**VK**: `[F-021] EPS Analysis | {company_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Equity structure and shares outstanding from balance sheet |
| F-002 | Requires output from | Net income for EPS numerator |
| F-013 | Requires output from | Convertible securities and options disclosures from 10-K |
| F-006 | Feeds into | Consistent consensus beats signal earnings management |
| F-014 | Feeds into | EPS is denominator for P/E ratio in dashboard |
| F-022 | Feeds into | Stock option grants create dilution assessed here |

---
---

# F-095: Personal Net Worth Tracking

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 1
**Complexity**: BASIC
**Rule**: TRAIN-F-095

---

## S1 Problem

Personal net worth tracking applies the same balance sheet principles used for businesses to an individual's finances. Without this capability, an AI assistant cannot help users understand their own financial position, set wealth-building targets, or optimize their personal asset allocation. While not directly part of business evaluation, this procedure builds financial literacy and applies F-010 concepts at a personal level.

**Situations covered**:
- Helping a user build a personal balance sheet
- Tracking net worth growth over time
- Identifying high-interest liabilities for accelerated payoff
- Benchmarking personal wealth against age-based targets
- **NOT suitable for**: Corporate or business entity balance sheets (this is a personal finance procedure), or individuals with complex trust/estate structures requiring professional valuation

---

## S2 Procedure

1. Create a personal balance sheet. List all assets: cash and bank accounts, investment portfolios (stocks, bonds, crypto), retirement accounts (401k, IRA), real estate (market value), vehicles, valuable personal property.
2. List all liabilities: mortgage balances, auto loans, student loans, credit card balances, personal loans, any other outstanding debts.
3. Calculate Net Worth = Total Assets - Total Liabilities. This is the personal equivalent of stockholders' equity.
4. Categorize assets by liquidity: liquid (cash, publicly traded investments), semi-liquid (real estate, retirement accounts with penalties), illiquid (business ownership, collectibles). Calculate the Liquid Net Worth = Liquid Assets - Total Liabilities.
5. Set up a monthly or quarterly tracking cadence. Record net worth over time and calculate the month-over-month or quarter-over-quarter change. Decompose changes into: (a) savings rate contribution, (b) investment returns, (c) liability reduction, (d) asset appreciation/depreciation.
6. Calculate the Personal Savings Rate = (Income - Total Expenses) / Income. Track this alongside net worth growth.
7. Set benchmarks: target net worth by age (common rule of thumb: 1x annual salary by 30, 3x by 40, 6x by 50, 8x by 60). Compare current position to target.
8. Advise on optimization: identify the highest-interest liabilities for accelerated payoff (debt avalanche method), identify underperforming assets for reallocation, calculate the months of expenses covered by emergency fund (target 3-6 months).

**Output**: Personal net worth statement with: (a) asset/liability inventory, (b) net worth and liquid net worth, (c) trend over time, (d) savings rate, (e) benchmark comparison, (f) optimization recommendations.

**AI Execution Notes**: This is an advisory procedure. Genie assists the user in building a personal balance sheet template (spreadsheet or structured data). For investment portfolio valuation, use current market prices from financial APIs. For real estate, use Zillow estimates or recent comparable sales as approximations.

---

## S3 Cortex Logging

```json
{
  "text": "F-095 Personal Net Worth Tracking — executed for {user_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-095",
    "domain": "finance",
    "level": 1,
    "company": "{user_name}",
    "tags": ["business-evaluation", "personal-finance", "net-worth", "advisory"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 1
**WHEN**: When a user requests personal financial assessment
**PREREQUISITES**: None — standalone advisory procedure (applies F-010 principles)
**FEEDS INTO**: None — terminal advisory procedure

**VK**: `[F-095] Personal Net Worth Tracking | {user_name} | Level 1 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Conceptual basis | Applies balance sheet construction principles to personal finances |

---
---

# LEVEL 2 — Analyze the Business

---
---

# F-001: DuPont Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-001

---

## S1 Problem

DuPont analysis decomposes Return on Equity into its fundamental drivers: profitability, efficiency, and leverage. Without this decomposition, an AI evaluator sees only a single ROE number and cannot determine whether high returns come from operational excellence or excessive debt. Skipping this procedure means accepting ROE at face value without understanding the risk embedded in it.

**Situations covered**:
- Decomposing ROE to identify whether margin, turnover, or leverage drives returns
- Detecting artificially high ROE from excessive financial leverage
- Performing peer comparison at the component level to identify strategic positioning
- Sensitivity analysis on which lever has the most impact on ROE improvement
- **NOT suitable for**: Financial institutions where leverage ratios have regulatory meanings different from industrial companies, or companies with negative equity (denominator becomes meaningless)

---

## S2 Procedure

1. Gather the three components: Net Profit Margin = Net Income / Revenue; Asset Turnover = Revenue / Average Total Assets; Equity Multiplier = Average Total Assets / Average Stockholders' Equity.
2. Calculate the 3-component DuPont ROE = Net Profit Margin x Asset Turnover x Equity Multiplier. Verify this equals the directly calculated ROE = Net Income / Average Equity.
3. For deeper decomposition (5-component DuPont): Tax Burden = Net Income / Pre-Tax Income; Interest Burden = Pre-Tax Income / EBIT; Operating Margin = EBIT / Revenue; Asset Turnover = Revenue / Average Assets; Equity Multiplier = Average Assets / Average Equity. ROE = Tax Burden x Interest Burden x Operating Margin x Asset Turnover x Equity Multiplier.
4. Analyze which component drives ROE: (a) High margin, low turnover = differentiation strategy; (b) Low margin, high turnover = cost leadership; (c) High equity multiplier = leveraged returns (risky).
5. Perform trend analysis: calculate each DuPont component for 3-5 years. Identify which component is improving or deteriorating. A rising ROE driven solely by increasing leverage (equity multiplier) is a warning sign.
6. Perform peer comparison: calculate DuPont components for the top 2-3 competitors. Identify where the target company has an advantage or disadvantage.
7. Calculate the impact of each component: if Profit Margin improves by 1 percentage point while other components stay constant, how much does ROE change? This sensitivity analysis reveals leverage points for improvement.

**Output**: DuPont decomposition table (3-component and optionally 5-component) with: (a) 3-5 year trends, (b) peer comparison, (c) driver identification (margin vs. efficiency vs. leverage), (d) sensitivity analysis, (e) strategic classification.

**AI Execution Notes**: The DuPont framework is the core analytical tool for understanding ROE. Always start with the 3-component version. Use the 5-component version when a company's interest expense or tax rate is unusual. The key insight: ROE can be "artificially" high from leverage — always check the equity multiplier.

---

## S3 Cortex Logging

```json
{
  "text": "F-001 DuPont Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-001",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "ratio-analysis", "dupont", "ROE-decomposition"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When evaluating any business's financial health at the analysis level
**PREREQUISITES**: F-002 (Profitability Ratios — provides margins and ROE), F-004 (Activity Ratios — provides Asset Turnover), F-010 (Balance Sheet — provides equity and assets)
**FEEDS INTO**: F-014 (Comprehensive Dashboard — DuPont components), F-019 (Leverage Analysis — equity multiplier context), F-034 (Financial Leverage Impact)

**VK**: `[F-001] DuPont Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002 | Requires output from | Net Profit Margin and ROE from profitability ratios |
| F-004 | Requires output from | Asset Turnover from activity ratios |
| F-010 | Requires output from | Average Total Assets and Average Equity from balance sheet |
| F-014 | Feeds into | DuPont components are key dashboard metrics |
| F-019 | Feeds into | Equity multiplier provides leverage context |
| F-034 | Feeds into | DuPont leverage component informs leverage impact analysis |

---
---

# F-014: Comprehensive Ratio Dashboard

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-014

---

## S1 Problem

Individual ratio analyses provide fragmented views of financial health. Without a unified dashboard that aggregates profitability, liquidity, activity, solvency, and valuation metrics with industry benchmarks and color-coded status indicators, an AI evaluator cannot deliver a holistic assessment or quickly identify the most critical strengths and weaknesses. Skipping this procedure means presenting scattered metrics instead of an integrated financial health scorecard.

**Situations covered**:
- Delivering a complete financial health assessment in a single view
- Comparing all ratio categories against industry benchmarks simultaneously
- Identifying the top 3 strengths and top 3 weaknesses across all dimensions
- Generating a visual radar chart for quick pattern recognition
- **NOT suitable for**: Cross-industry comparisons without sector-specific ratio benchmarks, or startups with less than 2 years of financial history where trends are unreliable

---

## S2 Procedure

1. Compile all ratios from F-002 (Profitability), F-003 (Liquidity), F-004 (Activity), plus Solvency ratios (F-019) into a single unified dashboard.
2. Profitability ratios (6): Gross Margin, Operating Margin, Net Margin, ROA, ROE, ROIC.
3. Liquidity ratios (4): Current Ratio, Quick Ratio, Cash Ratio, CCC (Cash Conversion Cycle).
4. Activity ratios (5): AR Turnover (DSO), Inventory Turnover (DIO), AP Turnover (DPO), Asset Turnover, Fixed Asset Turnover.
5. Solvency ratios (4): Debt-to-Equity, Debt-to-Assets, Interest Coverage (EBIT/Interest Expense), Long-term Debt-to-Capitalization.
6. Market/Valuation ratios (4): P/E, P/B, EV/EBITDA, Dividend Yield.
7. For each ratio, include: (a) current year value, (b) prior year value, (c) 3-year average, (d) industry median, (e) status indicator (green/yellow/red vs. industry benchmark).
8. Generate a radar/spider chart to visually display the company's ratio profile relative to industry benchmarks across all four dimensions (profitability, liquidity, activity, solvency).
9. Write a narrative summary: identify the top 3 strengths and top 3 weaknesses revealed by the dashboard. Prioritize issues by materiality.

**Output**: A complete ratio dashboard (23+ ratios) with multi-year trends, industry benchmarks, color-coded status, radar chart, and narrative summary of key findings.

**AI Execution Notes**: Pull all data from a unified financial data source. Use Damodaran's industry average database for benchmarks. The dashboard should be generated as a structured table that can be displayed or exported. This procedure aggregates F-002, F-003, F-004, F-019 into one deliverable.

**Tools prerequisites**: F-083 (Dashboard Construction) for layout, KPI cards, and visual design. F-084 (Portfolio Dashboard) for live market data integration via GOOGLEFINANCE. F-081 (VLOOKUP/XLOOKUP) for ratio data retrieval from financial databases. Google Sheets: use SPARKLINE for inline trend indicators, GOOGLEFINANCE for live valuation ratios.

---

## S3 Cortex Logging

```json
{
  "text": "F-014 Comprehensive Ratio Dashboard — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-014",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "ratio-analysis", "dashboard", "comprehensive"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When delivering a complete financial assessment for any business
**PREREQUISITES**: F-002 (Profitability Ratios), F-003 (Liquidity Ratios), F-004 (Activity Ratios), F-019 (Solvency Ratios), F-021 (EPS — for P/E)
**FEEDS INTO**: Terminal aggregation procedure — feeds into final business evaluation report

**VK**: `[F-014] Comprehensive Ratio Dashboard | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002 | Requires output from | Profitability ratios (6 metrics) |
| F-003 | Requires output from | Liquidity ratios (4 metrics) |
| F-004 | Requires output from | Activity ratios (5 metrics) |
| F-019 | Requires output from | Solvency ratios (4 metrics) |
| F-021 | Requires output from | EPS for P/E ratio calculation |
| F-001 | Requires output from | DuPont components for ROE decomposition display |

---
---

# F-015: Time Series / Trend Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-015

---

## S1 Problem

A single year's financial data is a snapshot; trend analysis reveals the trajectory. Without time series analysis, an AI evaluator cannot distinguish improving from deteriorating businesses, identify inflection points, detect seasonality, or project future performance from historical patterns. Skipping this procedure means evaluating a business based on where it is today rather than where it is heading.

**Situations covered**:
- Determining whether a company's financial health is improving or deteriorating
- Calculating Compound Annual Growth Rates for key metrics
- Identifying seasonal patterns in revenue and earnings
- Projecting baseline financial performance 1-2 years forward
- **NOT suitable for**: Companies with fewer than 3 years of comparable data, recently merged entities where historical comparisons span different business compositions, or industries undergoing structural disruption where past trends are not predictive

---

## S2 Procedure

1. Collect at least 3-5 years of financial data for the target company: revenue, COGS, gross profit, operating expenses, net income, total assets, equity, key ratios.
2. Perform horizontal analysis: calculate year-over-year dollar changes and percentage changes for each major line item. Identify items with persistent growth or decline trends.
3. Perform index analysis: set a base year (= 100) and express all subsequent years as an index. This reveals cumulative growth patterns. Example: if Year 1 revenue = 100, Year 5 = 145, revenue grew 45% over the period.
4. Calculate the Compound Annual Growth Rate (CAGR) for key metrics: CAGR = (Ending Value / Beginning Value)^(1/n) - 1. Compare revenue CAGR vs. net income CAGR — if NI grows faster than revenue, margins are expanding.
5. Chart key trends: plot revenue and COGS on the same axis to visualize margin expansion or compression. Plot operating income and capex to identify investment cycles. Plot debt levels and interest coverage to assess financial health trajectory.
6. Apply simple time series modeling: use a 12-month lagged auto-regression on revenue to identify seasonality patterns. If R-squared is >0.80 with a 12-month lag, the business has strong seasonal patterns.
7. Identify inflection points: periods where growth accelerated, decelerated, or reversed. Correlate these with known events (acquisitions, market downturns, product launches, management changes).
8. Project 1-2 years forward using the trend: apply the CAGR or regression model for a baseline forecast. Flag this as an extrapolation, not a prediction.

**Output**: Trend analysis report with: (a) horizontal analysis table, (b) index analysis, (c) CAGR calculations for key metrics, (d) trend charts, (e) seasonality assessment, (f) inflection point analysis, (g) baseline projection.

**AI Execution Notes**: Use 5 years minimum for meaningful trend analysis. Pull data from Macrotrends or similar historical databases. The time series auto-regression (from complete-financial-analyst S40) can be run in Excel with the Data Analysis Toolpak or programmatically with standard regression libraries.

---

## S3 Cortex Logging

```json
{
  "text": "F-015 Time Series / Trend Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-015",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "trend-analysis", "time-series", "CAGR", "forecasting"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When performing multi-year financial analysis on any business
**PREREQUISITES**: F-010 (Balance Sheet — multi-year data), F-011 (Cash Flow — multi-year data), F-002 (Profitability — ratio trends)
**FEEDS INTO**: F-016 (Cross-Sectional Analysis — trend context for peer comparison), F-092 (Credit Analysis — trend-based risk assessment)

**VK**: `[F-015] Time Series / Trend Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Multi-year balance sheet data |
| F-011 | Requires output from | Multi-year cash flow data |
| F-002 | Requires output from | Multi-year profitability ratio trends |
| F-016 | Feeds into | Trend context enriches cross-sectional peer comparison |
| F-092 | Feeds into | Historical trajectory informs credit risk assessment |

---
---

# F-016: Comparative / Cross-Sectional Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-016

---

## S1 Problem

A company's financial metrics are meaningful only in context. Without cross-sectional comparison against peers, an AI evaluator cannot determine whether a 15% operating margin is excellent or mediocre for the industry, whether asset turnover is above or below par, or how the company ranks overall. Skipping this procedure means evaluating a business in a vacuum.

**Situations covered**:
- Ranking a company against 3-5 industry peers across all financial dimensions
- Identifying competitive advantages and disadvantages through benchmarking
- Normalizing financials using common-size statements for valid cross-company comparison
- Computing composite scores for relative positioning
- **NOT suitable for**: Companies in niche industries with fewer than 3 viable peers, or comparisons between companies using different accounting standards without reconciliation

---

## S2 Procedure

1. Identify the peer group: 3-5 companies in the same industry, of similar size (revenue or market cap). Use SIC/NAICS codes or sector classification to select peers.
2. Compile common-size (vertical) financial statements for all peers: express each income statement item as a percentage of revenue; express each balance sheet item as a percentage of total assets. This normalizes for size differences.
3. Create a comparative table for key metrics: Gross Margin, Operating Margin, Net Margin, ROA, ROE, Current Ratio, Debt-to-Equity, Asset Turnover, Revenue Growth, and any sector-specific KPIs.
4. Rank the target company against peers for each metric. Identify areas where the target ranks in the top quartile (strengths) and bottom quartile (weaknesses).
5. Perform a benchmarking analysis at the business unit level (if segment data available): identify which of the company's divisions perform best/worst relative to pure-play competitors in those segments.
6. Compare gross profit margins across country units or segments (internal comparative analysis): identify which units are the most and least profitable. Flag outliers — a division with 15% GPM when peers show 30%+ needs investigation.
7. Calculate a composite score: assign equal weight to profitability, liquidity, efficiency, and solvency rankings. Compute a weighted average rank for each company. This produces a simple relative positioning metric.
8. Write a narrative: "Company X outperforms peers in operational efficiency (ranked #1 in asset turnover and #2 in inventory turnover) but lags in profitability (#4 in net margin) likely due to aggressive pricing strategy."

**Output**: Cross-sectional analysis report with: (a) common-size statements, (b) peer comparison table with rankings, (c) composite score, (d) strengths/weaknesses narrative, (e) visualization (bar charts or heatmap of ratio rankings).

**AI Execution Notes**: Use financial databases that provide peer comparison data. When exact peers are unavailable, use the industry median from Damodaran's database. The SUMIFS-based Excel approach from complete-financial-analyst S40 works for internal segmented comparisons. Always disclose the peer group used.

**Tools prerequisites**: F-089 (SUMIF P&L Database) for SUMIFS-based segmented aggregation. F-082 (Pivot Tables) for multi-dimensional peer comparison. Google Sheets: use QUERY for multi-criteria aggregation instead of SUMIFS on large datasets.

---

## S3 Cortex Logging

```json
{
  "text": "F-016 Comparative / Cross-Sectional Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-016",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "peer-comparison", "cross-sectional", "benchmarking"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When comparing a target company against industry peers
**PREREQUISITES**: F-002 (Profitability Ratios), F-003 (Liquidity Ratios), F-004 (Activity Ratios), F-015 (Trend Analysis — for trend context)
**FEEDS INTO**: F-014 (Dashboard — peer benchmarks), F-092 (Credit Analysis — relative positioning)

**VK**: `[F-016] Comparative / Cross-Sectional Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-002 | Requires output from | Profitability ratios for peer ranking |
| F-003 | Requires output from | Liquidity ratios for peer ranking |
| F-004 | Requires output from | Activity ratios for peer ranking |
| F-015 | Requires output from | Trend context enriches cross-sectional comparison |
| F-017 | Requires output from | Inventory method adjustments for valid comparison |
| F-014 | Feeds into | Peer benchmarks populate the dashboard |
| F-092 | Feeds into | Relative positioning informs credit analysis |

---
---

# F-019: Leverage and Solvency Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-019

---

## S1 Problem

Leverage and solvency ratios reveal whether a company can meet its long-term obligations and how much financial risk its capital structure introduces. Without this analysis, an AI evaluator cannot assess bankruptcy risk, determine if debt levels are sustainable, or evaluate the risk-return trade-off of leverage. Skipping this procedure means ignoring the financial structure that can amplify both gains and catastrophic losses.

**Situations covered**:
- Assessing long-term solvency and bankruptcy risk
- Evaluating debt covenant compliance and proximity to breach
- Analyzing the leverage amplification effect on ROE
- Assessing refinancing risk from debt maturity concentration
- **NOT suitable for**: Banks and financial institutions where leverage has regulatory definitions (use Basel III capital ratios instead), or companies in active restructuring where debt terms are being renegotiated

---

## S2 Procedure

1. Gather data: Total Debt (short-term + long-term), Total Assets, Total Equity, EBIT, Interest Expense, EBITDA, and Cash and Equivalents.
2. Calculate Debt-to-Equity Ratio = Total Debt / Total Equity. A ratio >2.0 indicates high leverage. Flag industries with typically low leverage (tech/SaaS) if D/E exceeds 1.0.
3. Calculate Debt-to-Assets Ratio = Total Debt / Total Assets. This shows what percentage of assets is financed by debt. A ratio >0.7 (70%) is generally considered high.
4. Calculate Interest Coverage Ratio = EBIT / Interest Expense. A ratio <2.0 means the company barely covers interest payments from operations. Flag if <1.5 — imminent debt service risk.
5. Calculate Net Debt = Total Debt - Cash and Equivalents. Then Net Debt-to-EBITDA = Net Debt / EBITDA. This is the standard metric for assessing leverage relative to cash earnings. Most industries consider >4.0x as highly leveraged.
6. Analyze the financial leverage effect: compare ROE with and without leverage. Use the DuPont Equity Multiplier from F-001. A levered company can have a higher ROE than an unlevered one with the same operating performance, but the amplification works in both directions — losses are also magnified.
7. Quantify the impact: if a company has $1M assets, 50% debt at 8% interest, and 40% tax rate, compare ROE scenarios at -10%, base, and +10% operating income (as shown in the corporate-finance-course S06 Sigma example). This reveals how leverage amplifies both upside and downside.
8. Assess debt covenants: check if the company discloses financial covenants (minimum interest coverage, maximum D/E). Calculate how close the company is to breaching covenants. Flag if within 10% of a covenant threshold.
9. Check the debt maturity profile: identify when major debt tranches mature. A company with >30% of debt maturing in the next 12 months faces refinancing risk.

**Output**: Solvency analysis with: (a) 5 leverage ratios + 2-year trends, (b) leverage amplification scenario table, (c) covenant proximity analysis, (d) debt maturity timeline, (e) risk classification (Low/Medium/High leverage risk).

**AI Execution Notes**: Debt data is often buried in 10-K footnotes. For the scenario analysis, model 3 cases: base, +10% EBIT, -10% EBIT, showing the impact on NI and ROE under current capital structure vs. an all-equity hypothetical. The corporate-finance-course S06 example of Sigma (100% equity vs. 50/50 debt-equity) is the reference framework.

---

## S3 Cortex Logging

```json
{
  "text": "F-019 Leverage and Solvency Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-019",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "solvency", "leverage", "debt-analysis", "covenants"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When evaluating any business's long-term financial health
**PREREQUISITES**: F-010 (Balance Sheet — debt and equity figures), F-013 (10-K — debt schedule and covenants from footnotes), F-001 (DuPont — equity multiplier context)
**FEEDS INTO**: F-014 (Comprehensive Dashboard — solvency ratios), F-034 (Financial Leverage Impact — DFL inputs), F-092 (Credit Analysis — leverage assessment)

**VK**: `[F-019] Leverage and Solvency Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Total debt, total assets, total equity from balance sheet |
| F-013 | Requires output from | Debt schedule, maturities, and covenants from 10-K |
| F-001 | Requires output from | Equity multiplier from DuPont decomposition |
| F-014 | Feeds into | Solvency ratios populate the comprehensive dashboard |
| F-034 | Feeds into | Leverage ratios feed into financial leverage impact analysis |
| F-092 | Feeds into | Leverage assessment is core to credit analysis |

---
---

# F-023: Working Capital Management

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-023

---

## S1 Problem

Working capital management determines how efficiently a company manages the cash tied up in day-to-day operations. Without this analysis, an AI evaluator cannot identify cash trapped in slow-moving inventory, deteriorating receivable quality, or strained supplier relationships. Skipping this procedure means missing the operational levers that directly impact cash flow and short-term financial health.

**Situations covered**:
- Diagnosing inefficient cash conversion in operations
- Identifying overtrading (growth outpacing working capital capacity)
- Assessing receivable quality, inventory obsolescence risk, and payable management
- Recommending a working capital optimization roadmap with cash impact estimates
- **NOT suitable for**: Negative working capital businesses that operate by design (e.g., subscription SaaS with deferred revenue, large retailers with supplier financing), where negative WC is a strength not a weakness

---

## S2 Procedure

1. Calculate Working Capital = Current Assets - Current Liabilities. Calculate the Working Capital Ratio = Current Assets / Current Liabilities.
2. Decompose working capital into its three operational components: Trade Receivables (DSO), Inventory (DIO), and Trade Payables (DPO). Calculate each using the formulas from F-004.
3. Calculate the Cash Conversion Cycle (CCC) = DSO + DIO - DPO. Benchmark against industry median. A negative CCC (like Amazon) is a competitive advantage — the company gets paid before paying suppliers.
4. Assess Trade Receivables Management: identify the proportion of receivables >90 days overdue (if disclosed). Check the allowance for doubtful accounts as a percentage of gross receivables — a rising percentage signals deteriorating collection quality. Look for factoring arrangements (selling receivables to collect sooner).
5. Assess Inventory Management: calculate Economic Order Quantity (EOQ) = sqrt(2 x Annual Demand x Order Cost / Holding Cost per Unit) if data available. Check inventory for obsolescence risk: if DIO is increasing while revenue is flat or declining, excess inventory may need to be written down.
6. Assess Trade Payables Management: determine if the company is paying suppliers on time, early, or late. Consistently stretching payables beyond terms may damage supplier relationships. Paying too early foregoes cash float.
7. Identify symptoms of inefficient working capital: (a) overtrading (revenue growing faster than working capital can support — leads to cash crunches), (b) excessive investment (too much cash tied up in inventory or receivables).
8. Recommend a working capital optimization roadmap: (a) assess current position using metrics, (b) build dashboards to track DSO/DIO/DPO monthly, (c) create an action plan for the weakest component, (d) forecast the cash impact of proposed improvements.

**Output**: Working capital management report with: (a) CCC analysis and trend, (b) component-by-component assessment (AR, Inventory, AP), (c) symptoms of inefficiency identification, (d) optimization recommendations with estimated cash impact, (e) dashboard KPIs to monitor.

**AI Execution Notes**: Use the complete-financial-analyst S38 working capital optimization roadmap as the framework. The top-performing companies (per the 2015 study cited in the course) use half the working capital of average firms — this is the benchmark aspiration. For EOQ calculations, estimate demand from COGS and order costs from industry norms.

**Tools prerequisite**: F-083 (Dashboard Construction) for building DSO/DIO/DPO monitoring dashboards referenced in step 8. Google Sheets: use SPARKLINE for inline CCC trend visualization.

---

## S3 Cortex Logging

```json
{
  "text": "F-023 Working Capital Management — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-023",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "working-capital", "cash-conversion-cycle", "operational-efficiency"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When evaluating operational cash efficiency for any business
**PREREQUISITES**: F-003 (Liquidity Ratios — working capital baseline), F-004 (Activity Ratios — DSO/DIO/DPO), F-010 (Balance Sheet — current assets/liabilities)
**FEEDS INTO**: F-014 (Dashboard — CCC and working capital metrics), F-092 (Credit Analysis — cash flow adequacy)

**VK**: `[F-023] Working Capital Management | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-003 | Requires output from | Working capital and liquidity baseline |
| F-004 | Requires output from | DSO, DIO, DPO from activity ratio analysis |
| F-010 | Requires output from | Current assets and current liabilities detail |
| F-014 | Feeds into | CCC and working capital metrics for dashboard |
| F-092 | Feeds into | Cash adequacy informs credit analysis |

---
---

# F-034: Financial Leverage Impact Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-034

---

## S1 Problem

Financial leverage amplifies both returns and losses through the entire chain from revenue to EPS. Without modeling this amplification, an AI evaluator cannot quantify how sensitive the bottom line is to revenue changes, compare leveraged vs. unleveraged capital structures, or identify the breakeven EBIT where leverage begins to destroy value. Skipping this procedure means accepting leverage without quantifying its risk.

**Situations covered**:
- Quantifying the combined operating and financial leverage effect on EPS
- Modeling revenue-to-EPS sensitivity under current capital structure
- Comparing leveraged vs. all-equity capital structure outcomes
- Identifying the breakeven EBIT where leverage switches from beneficial to harmful
- **NOT suitable for**: Companies with zero debt (DFL is undefined when financial costs are zero), or companies in financial distress where leverage impact is non-linear

---

## S2 Procedure

1. Define the three types of leverage: Operating Leverage (fixed operating costs), Financial Leverage (fixed financial costs / interest), and Total Leverage (combined effect).
2. Calculate Degree of Operating Leverage (DOL) = % Change in EBIT / % Change in Revenue. Alternatively, DOL = Contribution Margin / EBIT. A DOL of 3 means a 10% revenue change creates a 30% EBIT change.
3. Calculate Degree of Financial Leverage (DFL) = % Change in EPS / % Change in EBIT. Alternatively, DFL = EBIT / (EBIT - Interest Expense). A DFL of 2 means a 10% EBIT change creates a 20% EPS change.
4. Calculate Degree of Total Leverage (DTL) = DOL x DFL = % Change in EPS / % Change in Revenue. This shows the total magnifying effect from revenue to the bottom line.
5. Build a scenario table: model revenue changes of -20%, -10%, base, +10%, +20%. For each scenario, compute EBIT (using DOL), then Net Income and EPS (using DFL). Display the total impact chain from revenue to EPS.
6. Compare two capital structures: (a) the company's current leveraged structure, (b) a hypothetical all-equity structure. Show how ROE and EPS differ under each scenario. Use the corporate-finance-course S06 framework: with 50% debt at 8% and 40% tax rate, compare NI and ROE under optimistic and pessimistic scenarios.
7. Calculate the breakeven EBIT point: the level of EBIT where the leveraged and unleveraged capital structures produce the same EPS. Below this EBIT, the unleveraged structure is preferred; above it, the leveraged structure amplifies returns.
8. Assess the risk-reward trade-off: high leverage increases expected ROE but also increases the probability of financial distress. Quantify this by calculating the probability that EBIT falls below interest expense (based on historical EBIT volatility).

**Output**: Financial leverage impact report with: (a) DOL/DFL/DTL calculations, (b) scenario sensitivity table (revenue to EPS chain), (c) leveraged vs. unleveraged comparison, (d) breakeven EBIT analysis, (e) risk-reward assessment.

**AI Execution Notes**: The DOL and DFL calculations require either two periods of data (to compute % changes) or the component formulas (CM/EBIT and EBIT/EBT respectively). If the company doesn't disclose fixed vs. variable costs, estimate DOL from the operating cost structure. The Sigma example from corporate-finance-course S06 is the canonical case study for this procedure.

**Tools prerequisite**: F-086 (Goal Seek) for finding breakeven EBIT in step 7. Google Sheets: use Solver add-on or Apps Script bisection (see F-086).

---

## S3 Cortex Logging

```json
{
  "text": "F-034 Financial Leverage Impact Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-034",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "leverage", "capital-structure", "risk-analysis", "scenario-modeling"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When evaluating leverage risk and capital structure impact
**PREREQUISITES**: F-033 (Breakeven — provides DOL), F-019 (Leverage/Solvency — provides debt and interest data), F-002 (Profitability — margin data for scenarios)
**FEEDS INTO**: F-014 (Dashboard — leverage risk metrics), F-092 (Credit Analysis — leverage risk assessment)

**VK**: `[F-034] Financial Leverage Impact Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-033 | Requires output from | DOL and contribution margin from breakeven analysis |
| F-019 | Requires output from | Debt levels and interest expense for DFL calculation |
| F-002 | Requires output from | Margin data for scenario modeling |
| F-014 | Feeds into | DOL/DFL/DTL and risk metrics for dashboard |
| F-092 | Feeds into | Leverage risk assessment informs credit analysis |

---
---

# F-022: Executive Compensation Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-022

---

## S1 Problem

Executive compensation structure reveals whether management incentives are aligned with shareholder interests. Without this analysis, an AI evaluator cannot detect perverse incentive structures that encourage short-term earnings manipulation, assess whether CEO pay is justified by performance, or identify governance red flags. Skipping this procedure means evaluating a business without understanding the motivations of the people running it.

**Situations covered**:
- Assessing alignment between executive pay and shareholder returns
- Detecting compensation structures that incentivize earnings manipulation
- Evaluating governance quality through compensation committee independence
- Identifying excessive pay or suspicious stock option timing
- **NOT suitable for**: Private companies with no proxy statement disclosure, or non-US companies where compensation disclosure requirements differ materially from SEC rules

---

## S2 Procedure

1. Obtain the company's proxy statement (DEF 14A for US companies) from SEC EDGAR. Identify the Named Executive Officers (NEOs): typically CEO, CFO, and the next 3 highest-paid executives.
2. Decompose each executive's total compensation into components: (a) base salary, (b) cash bonus/annual incentive, (c) stock options, (d) restricted stock/RSU grants, (e) long-term incentive plans, (f) pension/deferred compensation, (g) perquisites and other compensation.
3. Calculate the performance-based pay ratio = (bonus + stock/option grants + LTI) / total compensation. A higher ratio suggests better alignment between pay and performance. Flag if >60% of CEO pay is fixed (poor alignment).
4. Identify the performance metrics used for variable compensation: EPS, revenue growth, ROE, stock price targets, TSR (Total Shareholder Return), or non-financial KPIs. Assess whether these metrics can be manipulated through accounting choices.
5. Check for the "cobra effect" (perverse incentives): does the compensation structure incentivize short-term earnings manipulation? For example, if bonuses are tied to annual EPS targets, managers have incentive to manage earnings just above the threshold.
6. Calculate the CEO Pay Ratio (required disclosure since 2018 in the US): CEO Total Comp / Median Employee Comp. Compare to industry peers.
7. Assess the Compensation Committee: is it composed entirely of independent directors? Are there any conflicts of interest? Review the committee's discussion of how they set compensation levels.
8. Analyze stock option grants: check the grant timing relative to stock price movements (backdating concern). Check if options were granted before positive announcements or after negative ones.

**Output**: Executive compensation analysis with: (a) compensation breakdown table for top 5 NEOs, (b) performance-based pay ratio, (c) performance metric assessment, (d) CEO pay ratio, (e) alignment score (how well comp structure aligns executive and shareholder interests), (f) red flags.

**AI Execution Notes**: The proxy statement (DEF 14A) is the primary data source. Focus on the Compensation Discussion and Analysis (CD&A) section. The key concern is principal-agent alignment: managers (agents) should be compensated in ways that maximize shareholder (principal) value. Flag any compensation structure where managers can earn large bonuses even when the stock underperforms.

---

## S3 Cortex Logging

```json
{
  "text": "F-022 Executive Compensation Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-022",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "governance", "executive-compensation", "principal-agent"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When evaluating management quality and governance of any public company
**PREREQUISITES**: F-013 (10-K — proxy statement reference), F-021 (EPS — to assess EPS-linked compensation)
**FEEDS INTO**: F-006 (Earnings Management — compensation motives), F-009 (Non-Financial KPIs — KPI integration into compensation)

**VK**: `[F-022] Executive Compensation Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-013 | Requires output from | Proxy statement referenced in 10-K reading |
| F-021 | Requires output from | EPS data to assess EPS-linked bonus structures |
| F-006 | Feeds into | Compensation motives inform earnings management detection |
| F-009 | Feeds into | Non-financial KPIs should be linked to compensation structure |

---
---

# F-006: Earnings Management Detection

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-006

---

## S1 Problem

Earnings management distorts reported financial performance, making companies appear more profitable, stable, or growth-oriented than they actually are. Without the ability to detect both legal (within GAAP) and fraudulent (fabricated) manipulation, an AI evaluator will be systematically misled by managed numbers. Skipping this procedure means trusting reported earnings at face value, which is the single most dangerous assumption in business evaluation.

**Situations covered**:
- Screening for accruals-based earnings manipulation (NI vs. CFO divergence)
- Detecting revenue-side red flags (channel stuffing, premature recognition)
- Detecting expense-side red flags (capitalizing expenses, reserve manipulation)
- Distinguishing earnings management from outright fraud
- Identifying real activities manipulation (R&D cuts, discretionary spending reduction)
- **NOT suitable for**: Companies with fewer than 5 years of continuous data under the same accounting standards, or industries with inherently volatile accruals (e.g., insurance, construction) where the Jones Model produces high false-positive rates

---

## S2 Procedure

1. Assess motive: check if management has incentive to manipulate earnings. Common motives: meeting analyst consensus (by 1-2 cents), debt covenant compliance, bonus thresholds, IPO window dressing, smoothing earnings volatility.
2. Assess means — Revenue side red flags: (a) Recognize revenue before cash collection (bill-and-hold, channel stuffing). Flag if AR grows significantly faster than revenue. (b) Recognize revenue after cash collection (deferred revenue manipulation). Check for unusual changes in deferred revenue.
3. Assess means — Expense side red flags: (a) Capitalizing vs. expensing (e.g., capitalizing operating expenses as assets to boost current-period income). Flag if capitalized costs grow faster than revenue. (b) Reserve account manipulation (over-reserving in good years, releasing reserves in bad years to smooth earnings). Check allowance for doubtful accounts and restructuring reserves.
4. Check accruals quality: Calculate Total Accruals = Net Income - Cash Flow from Operations. If accruals are persistently positive and growing, the company is reporting earnings not backed by cash — a classic earnings management signal.
5. Apply the Discretionary Accruals Model (Jones Model or Modified Jones Model): estimate expected accruals based on revenue growth and PP&E level. Deviations from expected accruals are "discretionary" and may indicate manipulation. Formula: Total Accruals = alpha + beta1(Change in Revenue) + beta2(PP&E) + epsilon. Large residuals (epsilon) flag potential manipulation.
   Note: In the standard Jones (1991) specification, all variables in the regression (total accruals, Δrevenue, PPE) must be scaled by lagged total assets to control for firm size. Omitting this scaling produces biased coefficient estimates.
6. Check for Discretionary Expenditure cuts: did the company reduce R&D, advertising, or maintenance spending to boost short-term earnings? Calculate R&D-to-Revenue ratio — a sudden drop may indicate real activities manipulation.
7. Look for big bath accounting: a new CEO taking massive write-downs in year 1 (lowering the base) to show dramatic "improvement" in subsequent years.
8. Distinguish between earnings management (legal, within GAAP) and fraud (fabricating transactions, changing ledger numbers). Both are concerning, but fraud involves criminal liability.

**Output**: Earnings management detection report with: (a) motive assessment, (b) revenue and expense red flag checklist, (c) accruals quality analysis (NI vs CFO), (d) discretionary accruals estimate, (e) discretionary expenditure trend, (f) overall risk rating (Low/Medium/High manipulation concern).

**AI Execution Notes**: The accruals check (NI vs CFO) is the simplest and most powerful screen — perform it first. Use the accounting-analytics Module 2 framework for the detailed red flags. For the Jones Model, you need industry-level regression data or can use simplified ratios. The financial-analysis Module 3 covers accounting scandals (Enron, WorldCom) as case studies for fraud patterns.

---

## S3 Cortex Logging

```json
{
  "text": "F-006 Earnings Management Detection — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-006",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "fraud-detection", "earnings-management", "accruals-analysis"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When assessing earnings quality for any business evaluation
**PREREQUISITES**: F-011 (Cash Flow — CFO for accruals check), F-012 (Double-Entry — transaction logic verification), F-013 (10-K — accounting policies and auditor opinion), F-022 (Executive Compensation — motive assessment)
**FEEDS INTO**: F-008 (Benford's Law — combined fraud screening), F-092 (Credit Analysis — earnings quality assessment)

**VK**: `[F-006] Earnings Management Detection | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-011 | Requires output from | CFO needed for NI-vs-CFO accruals check |
| F-012 | Requires output from | Double-entry logic verification supports manipulation detection |
| F-013 | Requires output from | Accounting policies and auditor opinion from 10-K |
| F-022 | Requires output from | Compensation structure reveals manipulation motives |
| F-008 | Feeds into | Benford's Law provides complementary fraud screening |
| F-092 | Feeds into | Earnings quality is critical input for credit analysis |

---
---

# F-008: Benford's Law Fraud Screen

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-008

---

## S1 Problem

Fabricated financial data rarely follows the natural digit distribution found in legitimate datasets. Without Benford's Law screening, an AI evaluator lacks a statistical tool to flag potentially fabricated numbers. While not proof of fraud, significant deviations from Benford's distribution elevate concern, especially when combined with earnings management red flags. Skipping this procedure removes a powerful quantitative fraud detection layer.

**Situations covered**:
- Statistical screening of financial data for potential fabrication
- Identifying digit anomalies that suggest manufactured numbers
- Complementing qualitative earnings management detection (F-006)
- Second-digit analysis to detect rounding manipulation
- **NOT suitable for**: Datasets with fewer than 200 data points (insufficient statistical power for chi-squared test), assigned/sequential numbers (e.g., invoice numbers), or data with natural constraints on leading digits (e.g., prices clustered around $9.99)

---

## S2 Procedure

1. Collect a large dataset of numeric values from the company's financial statements: all line items from the balance sheet, income statement, and cash flow statement across multiple periods. Include segment data and footnote figures if available. Minimum 200+ data points (Nigrini, 2012 recommends a minimum of 200 for reliable first-digit analysis; 50-100 is insufficient for statistical significance in the chi-squared test).
2. Extract the first digit of each number. Ignore negative signs and leading zeros. Count the frequency of each first digit (1 through 9).
3. Calculate the expected frequency using Benford's Law: P(d) = log10(1 + 1/d). Expected frequencies: 1=30.1%, 2=17.6%, 3=12.5%, 4=9.7%, 5=7.9%, 6=6.7%, 7=5.8%, 8=5.1%, 9=4.6%.
4. Compare actual first-digit frequencies to Benford's expected frequencies. Calculate the chi-squared statistic: chi2 = sum of [(Observed - Expected)^2 / Expected] for each digit.
5. Apply the significance test: with 8 degrees of freedom, chi2 > 15.51 (p < 0.05) indicates a statistically significant deviation from Benford's Law. This does NOT prove fraud but flags the data for deeper investigation.
6. Extend to second-digit analysis: extract the second digit and compare to Benford's second-digit distribution. Second-digit analysis can catch rounding manipulation (e.g., a company consistently rounding expenses down).
7. Investigate specific deviations: if digit "5" appears far more frequently than expected, look for rounded or estimated numbers. If "1" appears far less than expected, numbers may have been fabricated (humans tend to underweight "1" when inventing numbers).
8. Report findings with appropriate caveats: Benford's Law works best on naturally occurring, multi-order-of-magnitude datasets. It is less reliable for constrained datasets (e.g., prices in a narrow range) or very small samples.

**Output**: Benford's Law screening report with: (a) first-digit frequency table (actual vs. expected), (b) chi-squared test result, (c) visualization (bar chart comparing actual vs. Benford distributions), (d) second-digit analysis if warranted, (e) specific digit anomalies identified, (f) recommendation for further investigation or all-clear.

**AI Execution Notes**: Automate the digit extraction and frequency counting programmatically. Benford's Law is a screening tool, not proof of fraud. Use it alongside F-006 (Earnings Management Detection) — if both Benford's and accruals analysis flag the same company, the concern is materially elevated. Source: accounting-analytics Module 3 (Professor Bushee).

---

## S3 Cortex Logging

```json
{
  "text": "F-008 Benford's Law Fraud Screen — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-008",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "fraud-detection", "benfords-law", "statistical-analysis"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When screening financial data for potential manipulation or fraud
**PREREQUISITES**: F-010 (Balance Sheet — data points), F-011 (Cash Flow — data points), F-006 (Earnings Management — qualitative flags to combine with)
**FEEDS INTO**: F-092 (Credit Analysis — fraud risk assessment)

**VK**: `[F-008] Benford's Law Fraud Screen | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Balance sheet line items as data points |
| F-011 | Requires output from | Cash flow line items as data points |
| F-006 | Complements | Qualitative earnings management flags combined with quantitative Benford screen |
| F-092 | Feeds into | Fraud risk assessment informs credit analysis |

---
---

# F-009: Non-Financial KPI Linking

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-009

---

## S1 Problem

Financial metrics are lagging indicators that report what already happened. Non-financial KPIs (customer satisfaction, employee engagement, innovation metrics) are leading indicators that predict future financial performance. Without the ability to link non-financial KPIs to financial outcomes, an AI evaluator relies solely on backward-looking data and cannot assess whether a company's future trajectory will differ from its past. Skipping this procedure means evaluating businesses with only half the picture.

**Situations covered**:
- Mapping a company's strategy to measurable non-financial KPIs
- Testing the statistical linkage between KPIs and financial outcomes
- Identifying leading indicators that predict future revenue or margin changes
- Recommending KPI integration into management evaluation and forecasting
- **NOT suitable for**: Early-stage startups where KPI baselines do not exist yet, or companies that do not disclose operational metrics (some industries have minimal non-financial reporting)

---

## S2 Procedure

1. Identify the company's stated strategy (from 10-K, annual report, or investor presentations). Map the strategy to a causal business model: Strategy -> Actions -> KPIs -> Financial Outcomes.
2. Identify 5-10 non-financial KPIs that the company tracks or should track. Common categories: (a) Customer: NPS, customer satisfaction score, churn rate, customer acquisition cost; (b) Employee: engagement score, turnover rate, training hours; (c) Operational: defect rate, on-time delivery, capacity utilization; (d) Innovation: R&D pipeline, patent applications, new product revenue %.
3. For each non-financial KPI, hypothesize the causal link to financial performance: "If employee engagement increases, then customer satisfaction increases, then retention improves, then revenue per customer grows." This is the causal chain.
4. Test the linkage statistically: correlate each non-financial KPI with subsequent financial performance (with appropriate lag). A customer satisfaction score in Q1 should correlate with revenue growth in Q2-Q4, not the same quarter.
5. Construct reliable and valid measures: ensure each KPI actually captures what it claims to measure. An employee satisfaction survey with 10% response rate is not valid. A customer NPS based on 1,000+ responses has statistical validity.
6. Set targets using analytics: do NOT assume "more is always better." For example, customer satisfaction above 95% may have diminishing returns while consuming disproportionate resources. Find the optimal level through regression analysis of KPI vs. financial outcome.
7. Incorporate KPI results into financial forecasting: if the model shows employee engagement has a 0.3 correlation with next-quarter revenue, build this into the revenue forecast model.
8. Assess organizational issues: are managers evaluated on these KPIs? If not, the KPIs are likely to be ignored. Recommend embedding KPIs into performance evaluation and compensation structure (linking back to F-022).

**Output**: Non-financial KPI analysis with: (a) causal business model map, (b) KPI inventory with definitions and data sources, (c) statistical linkage results (correlation/regression), (d) optimal target recommendations, (e) integration plan (how to embed KPIs into forecasting and management evaluation).

**AI Execution Notes**: This procedure is based on the Balanced Scorecard / Strategy Map framework from accounting-analytics Module 4. The key analytical step is the lagged correlation between non-financial KPIs and financial outcomes. Genie can advise on which KPIs to track but executing the correlation analysis requires historical data from the company. Use publicly available non-financial data (Glassdoor ratings, app store reviews, web traffic) as proxies.

---

## S3 Cortex Logging

```json
{
  "text": "F-009 Non-Financial KPI Linking — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-009",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "non-financial-KPI", "balanced-scorecard", "leading-indicators"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When performing comprehensive business evaluation beyond financial metrics
**PREREQUISITES**: F-013 (10-K — company strategy and segment data), F-022 (Executive Compensation — KPI linkage to pay), F-002 (Profitability — financial outcomes to correlate against)
**FEEDS INTO**: F-092 (Credit Analysis — qualitative assessment enrichment)

**VK**: `[F-009] Non-Financial KPI Linking | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-013 | Requires output from | Company strategy and segment data from 10-K |
| F-022 | Requires output from | Compensation structure to assess KPI integration |
| F-002 | Requires output from | Financial outcome data to test KPI correlations |
| F-092 | Feeds into | Non-financial assessment enriches credit analysis |

---
---

# F-092: Credit Analysis Framework

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: ADVANCED
**Rule**: TRAIN-F-092

---

## S1 Problem

Credit analysis determines whether a company can reliably service its debt under both normal and stressed conditions. Without this capability, an AI evaluator cannot assess default risk, evaluate covenant compliance, or advise on the creditworthiness of a potential investment or partner. Skipping this procedure means evaluating a business without understanding whether it can survive a downturn.

**Situations covered**:
- Assessing borrower creditworthiness for lending or investment decisions
- Evaluating debt covenant compliance and headroom
- Stress-testing debt service capacity under downside scenarios
- Mapping credit ratings to default probabilities
- **NOT suitable for**: Sovereign credit analysis (requires different framework: fiscal policy, monetary policy, political risk), or project finance where cash flows are ring-fenced from the corporate entity

---

## S2 Procedure

1. Assess the borrower profile: industry, business model, competitive position, management quality. Identify whether the business operates in a cyclical or defensive sector.
2. Analyze quality of earnings: distinguish sustainable (recurring) from transitory (one-time) income. Calculate the ratio of sustainable earnings to total earnings. A company with >80% sustainable earnings is a stronger credit.
3. Evaluate financial ratios critical for credit: Interest Coverage (EBIT/Interest Expense), Debt/EBITDA, Current Ratio, CFO/Total Debt, Fixed Charge Coverage = (EBIT + Lease Payments) / (Interest + Lease Payments + Principal Repayments).
4. Assess collateral: what assets secure the debt? Calculate the Loan-to-Value (LTV) ratio for secured loans. Assess the liquidation value of assets (forced sale typically yields 50-70% of book value for equipment, less for intangibles).
5. Review credit covenants: identify financial covenants (minimum interest coverage, maximum D/E, minimum net worth) and incurrence covenants (limits on additional borrowing, asset sales, dividends). Calculate covenant headroom = (Current Ratio Value / Covenant Threshold) - 1. Flag if headroom < 10%.
6. Check the company's credit rating (if rated by Moody's, S&P, or Fitch). Map the rating to default probability: AAA = ~0.01% annual default; BBB = ~0.2%; BB = ~1%; B = ~5%; CCC = ~15%+.
7. Build a downside scenario: model a 20-30% revenue decline and assess whether the company can still service its debt. Calculate the breakeven revenue level for debt service (minimum revenue needed to cover interest + mandatory principal payments).
8. Assess financial flexibility: does the company have undrawn credit lines? Are there assets that can be liquidated? Can it raise equity if needed? A company with strong financial flexibility can weather temporary downturns.

**Output**: Credit analysis report with: (a) borrower profile assessment, (b) earnings quality analysis, (c) credit-specific ratio table, (d) collateral assessment, (e) covenant headroom analysis, (f) credit rating comparison, (g) downside scenario stress test, (h) overall credit recommendation (Approve/Conditional/Decline with terms).

**AI Execution Notes**: Use the financial-statement-analysis S04 framework for the credit analysis workflow: site visit, financial review, quality of earnings, balance sheet strength, trends, forecasting, flexibility assessment. For credit ratings, use S&P's scale: AAA to D. The key question is: "Can this company repay its obligations under normal AND stressed conditions?"

**Tools prerequisites**: F-068 (Loan Amortization) for debt service schedule construction in step 7. F-086 (Goal Seek) for finding breakeven revenue for debt service. F-080 (TVM Functions) for PV/FV calculations on debt instruments.

---

## S3 Cortex Logging

```json
{
  "text": "F-092 Credit Analysis Framework — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-092",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "credit-analysis", "debt-analysis", "stress-testing", "default-risk"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When assessing creditworthiness or debt sustainability for any business
**PREREQUISITES**: F-010 (Balance Sheet — asset and liability data), F-011 (Cash Flow — CFO/Debt ratios), F-019 (Leverage — solvency ratios), F-006 (Earnings Management — earnings quality), F-015 (Trend Analysis — historical trajectory)
**FEEDS INTO**: Terminal analysis procedure — feeds into final business evaluation and investment decision

**VK**: `[F-092] Credit Analysis Framework | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-010 | Requires output from | Asset and liability data for collateral and covenant analysis |
| F-011 | Requires output from | CFO for cash-based credit ratios |
| F-019 | Requires output from | Leverage and solvency ratios for credit assessment |
| F-006 | Requires output from | Earnings quality assessment for sustainable income analysis |
| F-015 | Requires output from | Historical trends for trajectory-based risk assessment |
| F-023 | Requires output from | Working capital adequacy for cash flow analysis |
| F-034 | Requires output from | Leverage impact for downside scenario modeling |

---
---

# F-093: Macroeconomic Analysis

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Business Evaluation Training Path
**Level**: 2
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-093

---

## S1 Problem

Every business operates within a macroeconomic environment that can amplify or undermine its performance regardless of operational excellence. Without macroeconomic analysis, an AI evaluator cannot contextualize financial results within GDP cycles, interest rate regimes, currency movements, or commodity price shifts. Skipping this procedure means evaluating a business as if it exists in a vacuum, disconnected from the economic forces that drive revenue, costs, and valuations.

**Situations covered**:
- Contextualizing a company's performance within the economic cycle
- Assessing the impact of monetary policy changes on leveraged companies
- Evaluating currency and commodity exposure for international businesses
- Identifying macro tailwinds and headwinds specific to the target company
- **NOT suitable for**: Short-term trading decisions (macro indicators lag market movements), or single-company valuation without integrating company-specific factors from L1-L3 procedures

---

## S2 Procedure

1. Gather key macroeconomic indicators for the company's primary operating countries: GDP growth rate, inflation rate (CPI), unemployment rate, central bank interest rate (Fed Funds, ECB rate), currency exchange rates.
2. Assess the GDP trajectory: is the economy expanding (GDP growth >2%), stagnating (0-2%), or contracting (<0%)? A contracting economy will pressure cyclical businesses (consumer discretionary, industrials) more than defensive ones (utilities, healthcare).
3. Analyze monetary policy: is the central bank tightening (raising rates, reducing QE) or easing (cutting rates, expanding QE)? Rising rates increase borrowing costs for leveraged companies and reduce present value of future cash flows (hurting growth stock valuations).
4. Assess currency impact: if the target company earns significant revenue internationally, analyze the impact of currency movements. A strengthening dollar reduces the value of foreign earnings when translated back. Calculate the revenue exposure by currency (if disclosed in segments).
5. Monitor commodity prices relevant to the company: oil (impacts transportation, manufacturing), metals (impacts industrials), agricultural commodities (impacts food/beverage). A company's input costs are directly affected by commodity prices.
6. Evaluate fiscal policy: are governments stimulating (increased spending, tax cuts) or consolidating (austerity, tax increases)? Government contracts, infrastructure spending, and regulatory changes directly impact certain sectors.
7. Assess trade policy: tariffs, trade agreements, and supply chain disruptions. Identify if the company's supply chain is exposed to geopolitical risk (e.g., China dependency, sanctions exposure).
8. Synthesize the macro picture: classify the environment as favorable, neutral, or unfavorable for the target company's specific business model. Identify the 2-3 macro factors with the highest impact on the company.

**Output**: Macroeconomic context report with: (a) dashboard of key macro indicators, (b) GDP/monetary policy/fiscal policy assessment, (c) currency and commodity exposure analysis, (d) trade/geopolitical risk assessment, (e) macro environment classification (favorable/neutral/unfavorable), (f) top macro risks and tailwinds for the specific company.

**AI Execution Notes**: Use IMF World Economic Outlook data, FRED (Federal Reserve Economic Data), World Bank databases, and central bank publications. For commodity data, use Alpha Vantage or STOOQ. The complete-financial-analyst-training S03-S06 covers IMF/GDP, World Bank/inflation, interest rates/monetary policy, and fiscal policy/trade. Genie should monitor these macro indicators continuously and flag significant changes that impact portfolio companies.

---

## S3 Cortex Logging

```json
{
  "text": "F-093 Macroeconomic Analysis — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-093",
    "domain": "finance",
    "level": 2,
    "company": "{company_name}",
    "tags": ["business-evaluation", "macroeconomics", "GDP", "monetary-policy", "geopolitical-risk"]
  }
}
```

---

## S4 Enforcement

**WHERE**: Business Evaluation Pipeline — Level 2
**WHEN**: When contextualizing any business evaluation within the economic environment
**PREREQUISITES**: F-013 (10-K — company's market and geographic exposure), F-019 (Leverage — interest rate sensitivity)
**FEEDS INTO**: F-092 (Credit Analysis — macro risk in downside scenarios), F-014 (Dashboard — macro context for ratio interpretation)

**VK**: `[F-093] Macroeconomic Analysis | {company_name} | Level 2 complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-013 | Requires output from | Geographic and market exposure from 10-K |
| F-019 | Requires output from | Leverage data to assess interest rate sensitivity |
| F-092 | Feeds into | Macro risks feed into credit analysis downside scenarios |
| F-014 | Feeds into | Macro context for interpreting ratio benchmarks |

---

# END OF FORGE PROCEDURES — BUSINESS EVALUATION L1 & L2
