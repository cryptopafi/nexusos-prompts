---
type: procedure
created: 2026-03-17
status: active
slug: excel-sheets-mastery-batch1
tags: [procedure, nexus]
---

# FORGE Procedures — Excel & Sheets Mastery (Batch 1)
# Procedure Count: 4
# Generated: 2026-03-03
# Template: FORGE Standard (Training Variant)
# Domain: Finance — Excel & Sheets Mastery (Tools Level)

**Note on S5 (Dependencies)**: All procedures in this file include an S5 Dependencies section beyond the FORGE Standard requirement (S1-S4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**Note on Dual Implementation**: Each procedure provides both Excel/VBA and Google Sheets/Apps Script implementation paths. Python/openpyxl equivalents are preserved from the raw source for CLI-based execution.

---

## Table of Contents

### Tools Level — Excel & Sheets Mastery (4 Procedures)
| # | ID | Name | Complexity |
|---|-----|------|-----------|
| 1 | F-066 | FMCG Report with SAP Data (P&L Database Case Study) | INTERMEDIATE |
| 2 | F-068 | Loan Amortization Schedule Construction | BASIC |
| 3 | F-070 | Financial Model Mapping and Sheet Architecture | INTERMEDIATE |
| 4 | F-080 | Excel Financial Functions Mastery (TVM) | BASIC |

---
---

# F-066: FMCG Report with SAP Data (P&L Database Case Study)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-066

---

## S1 Problem

Transforming raw SAP transactional data into a structured P&L management report is the core skill for any financial analyst working in FMCG or manufacturing companies. Without this procedure, an AI evaluator cannot produce the variance analysis, product group breakdowns, or executive-ready reporting that operations teams rely on for monthly business reviews. Raw SAP exports are flat transaction tables — they carry zero analytical value until mapped, aggregated, and formatted into P&L hierarchies.

**Situations covered**:
- Monthly management reporting from SAP transaction extracts
- Variance analysis (Actual vs Budget vs Prior Year) for P&L line items
- Product group and regional profitability breakdowns in FMCG context
- YTD and full-year forecast blending actuals with remaining budget
- Building Chart of Accounts mapping layers for any ERP-sourced data
- **NOT suitable for**: Non-P&L reporting (balance sheet or cash flow from SAP), companies without structured account code taxonomies, or SAP BPC/BW consolidated reporting (which has its own reporting layer)

---

## S2 Procedure

**Before starting**: Save a backup copy of the raw SAP extract before any data manipulation.

1. **Define the SAP data structure.** Create or receive a raw transaction dataset with mandatory columns: `Document_Date` (YYYY-MM-DD), `Posting_Period` (1-12), `Fiscal_Year`, `Account_Code` (e.g., 400000-series for revenue, 500000-series for COGS, 600000-series for OpEx), `Account_Description`, `Cost_Center`, `Product_Group` (FMCG categories: beverages, personal care, household, food), `Region` (e.g., EMEA, APAC, Americas), `Amount_Actual`, `Amount_Budget`, `Amount_Prior_Year`. Validate that no rows have null Account_Code or Amount_Actual.

   **Excel**: Import CSV via Data > From Text/CSV. Use Power Query to validate: Add Column > Custom Column > `if [Account_Code] = null then "INVALID" else "OK"`. Filter out invalid rows. **Safety**: Move filtered invalid rows to a 'Data_Issues' tab for manual review before deletion. Never discard data without inspection.

   **Python**: `df = pd.read_csv('sap_extract.csv', parse_dates=['Document_Date'])` then `assert df['Account_Code'].notna().all()`.

   ### Google Sheets Implementation
   Import CSV via File > Import > Upload. Validate with an auxiliary column: `=IF(ISBLANK(A2), "INVALID", "OK")`. Use `=COUNTBLANK(A2:A)` to count missing Account_Code values. For large datasets (>50K rows), consider using BigQuery connected sheets (`Data > Data connectors > BigQuery`) as Sheets has a 10M cell limit. **Note**: BigQuery connected sheets requires Google Workspace Enterprise or Standard. For basic accounts, split large CSVs into multiple sheets (<50K rows each).

2. **Build the Chart of Accounts mapping table.** Create a mapping sheet/dictionary that classifies every Account_Code into P&L line items. Structure: `Account_Code` -> `P&L_Line` -> `P&L_Category` -> `P&L_Section`. Example mappings: 400100 -> "Domestic Sales" -> "Revenue" -> "Net Revenue"; 500100 -> "Raw Materials" -> "COGS" -> "Cost of Goods Sold"; 600100 -> "Salaries" -> "Operating Expenses" -> "SG&A". Validate that every account code in the raw data has a mapping.

   **Excel**: Validation formula: `=COUNTIF(RawData[Account_Code], MappingTable[Account_Code])` must match for all codes. Use conditional formatting to highlight unmatched codes.

   **Python**: `mapping = pd.read_csv('chart_of_accounts.csv'); unmatched = df[~df['Account_Code'].isin(mapping['Account_Code'])]; assert len(unmatched) == 0, f"Unmatched codes: {unmatched['Account_Code'].unique()}"`.

   ### Google Sheets Implementation
   Create the mapping on a separate sheet named "COA_Mapping". Use `=COUNTIF(RawData!A:A, COA_Mapping!A2)` to validate each code exists. For unmatched detection: `=FILTER(RawData!A:A, ISNA(MATCH(RawData!A:A, COA_Mapping!A:A, 0)))` returns all unmatched codes. Apply conditional formatting: Format > Conditional formatting > Custom formula `=ISNA(MATCH(A2, COA_Mapping!$A:$A, 0))` with red fill.

3. **Aggregate actuals by P&L line using SUMIFS.** For each P&L line item and each reporting period, calculate the actual amount.

   **Excel**: For a specific line in a specific month: `=SUMIFS(RawData[Amount_Actual], RawData[Account_Code], ">="&AccountRangeStart, RawData[Account_Code], "<="&AccountRangeEnd, RawData[Posting_Period], TargetMonth, RawData[Fiscal_Year], TargetYear)`. For single account: `=SUMIF(RawData[Account_Code], "400100", RawData[Amount_Actual])`. For product group subtotals: `=SUMIFS(RawData[Amount_Actual], RawData[Product_Group], "Beverages", RawData[Posting_Period], 6, RawData[Fiscal_Year], 2025)`.

   **Python**: `pivot_actual = df.groupby(['Fiscal_Year', 'Posting_Period', 'Account_Code'])['Amount_Actual'].sum().reset_index()` then merge with mapping.

   ### Google Sheets Implementation
   SUMIFS syntax is identical in Google Sheets: `=SUMIFS(RawData!J:J, RawData!D:D, ">="&A2, RawData!D:D, "<="&B2, RawData!B:B, E2, RawData!C:C, F2)`. For large datasets, prefer `=QUERY(RawData!A:K, "SELECT D, SUM(J) WHERE C = 2025 AND B = 6 GROUP BY D LABEL SUM(J) 'Actual'")` which is more performant than SUMIFS on 50K+ rows. Google Sheets QUERY function has no Excel equivalent and is significantly faster for multi-criteria aggregation.

4. **Aggregate budget and prior year columns identically.** Repeat the SUMIFS logic for `Amount_Budget` and `Amount_Prior_Year`. Build three parallel columns: Actual, Budget, Prior Year.

   **Excel**: `=SUMIFS(RawData[Amount_Budget], RawData[Account_Code], AccountCode, RawData[Posting_Period], Month, RawData[Fiscal_Year], Year)`.

   **Python**: `pivot_all = df.groupby(['Fiscal_Year', 'Posting_Period', 'Account_Code']).agg({'Amount_Actual': 'sum', 'Amount_Budget': 'sum', 'Amount_Prior_Year': 'sum'}).reset_index()`.

   ### Google Sheets Implementation
   Same SUMIFS syntax. For efficiency, use a single QUERY with multiple aggregations: `=QUERY(RawData!A:K, "SELECT D, SUM(J), SUM(K), SUM(L) WHERE C = 2025 AND B = 6 GROUP BY D LABEL SUM(J) 'Actual', SUM(K) 'Budget', SUM(L) 'Prior Year'")`.

5. **Construct the P&L hierarchy.** Arrange the aggregated data into standard P&L format with calculated subtotals: Net Revenue - COGS = **Gross Profit**. Gross Profit - SG&A - R&D - Other OpEx = **EBIT**. EBIT - Interest Expense = **EBT**. EBT - Tax = **Net Income**. Calculate each subtotal for Actual, Budget, and Prior Year.

   **Excel**: Gross Profit formula: `=SUM(RevenueRange) - SUM(COGSRange)`.

   **Python**: Build a dictionary of P&L sections, sum the mapped accounts per section, compute subtotals programmatically.

   ### Google Sheets Implementation
   Identical SUM formulas. For dynamic subtotals that auto-adjust as lines are added, use named ranges (Data > Named ranges) or `=SUMPRODUCT((COA_Mapping!D:D="Revenue")*Actuals!B:B)` to sum all lines mapped to "Revenue" section dynamically.

6. **Calculate variance analysis columns.** For each P&L line, compute four variance metrics:
   - Actual vs Budget (absolute): `=Actual - Budget`
   - Actual vs Budget (%): `=IF(Budget<>0, (Actual-Budget)/ABS(Budget), "N/A")`
   - Actual vs Prior Year (absolute): `=Actual - Prior_Year`
   - Actual vs Prior Year (%): `=IF(Prior_Year<>0, (Actual-Prior_Year)/ABS(Prior_Year), "N/A")`

   Flag any variance exceeding +/-10% in red (conditional formatting). For revenue lines, positive variance is favorable; for cost lines, negative variance (lower costs) is favorable — apply sign convention accordingly.

   ### Google Sheets Implementation
   Formulas are identical. Conditional formatting: Format > Conditional formatting > Custom formula `=AND(ABS(E2)>0.1, E2<0)` with red fill for unfavorable revenue variances. For cost lines, reverse the logic: `=AND(ABS(E2)>0.1, E2>0)`. Use a helper column with "Revenue"/"Cost" classification to drive the sign convention programmatically.

7. **Build product group and regional breakdowns.** Create sub-reports sliced by Product_Group and Region using additional SUMIFS dimensions.

   **Excel**: `=SUMIFS(RawData[Amount_Actual], RawData[Product_Group], "Beverages", RawData[Account_Code], ">="&400000, RawData[Account_Code], "<="&499999, RawData[Posting_Period], Month)`. Produce a matrix: rows = P&L lines, columns = Product Groups (Beverages, Personal Care, Household, Food). Repeat for Region breakdown.

   **Python**: `product_pivot = df.merge(mapping, on='Account_Code').groupby(['P&L_Section', 'Product_Group'])['Amount_Actual'].sum().unstack(fill_value=0)`.

   ### Google Sheets Implementation
   SUMIFS works identically. For a cleaner approach, use a Pivot Table: Select data range > Insert > Pivot table. Set Rows = P&L_Section, Columns = Product_Group, Values = SUM of Amount_Actual. Google Sheets Pivot Tables are interactive and auto-refresh. Alternatively, `=QUERY(RawData!A:K, "SELECT H, SUM(J) WHERE D >= 400000 AND D <= 499999 AND B = 6 GROUP BY H PIVOT G")` produces the full product-group-by-region matrix in one formula.

8. **Generate YTD and full-year views.** For YTD: sum all periods from 1 through the current reporting month. For full-year forecast: YTD Actual + Remaining months from Budget.

   **Excel**: YTD: `=SUMPRODUCT((RawData[Account_Code]=AccountCode)*(RawData[Posting_Period]<=CurrentMonth)*(RawData[Fiscal_Year]=Year)*RawData[Amount_Actual])`. Full-year forecast: `=YTD_Actual + SUMIFS(RawData[Amount_Budget], RawData[Posting_Period], ">"&CurrentMonth, RawData[Account_Code], AccountCode)`.

   **Python**: `ytd = pivot_all[pivot_all['Posting_Period'] <= current_month].groupby('Account_Code').sum()`.

   ### Google Sheets Implementation
   SUMPRODUCT syntax is identical. For QUERY-based approach: `=QUERY(RawData!A:K, "SELECT D, SUM(J) WHERE B <= "&CurrentMonth&" AND C = 2025 GROUP BY D")`. Full-year forecast: `=YTD_Cell + QUERY(RawData!A:K, "SELECT SUM(K) WHERE B > "&CurrentMonth&" AND D = '"&AccountCode&"'")`.

9. **Format the management report.** Apply standard FMCG management reporting format:
   - Header: Company name, report title ("P&L Report -- Month Ending [Date]"), currency, reporting entity
   - Columns: Current Month (Actual | Budget | Var%) | YTD (Actual | Budget | Var% | PY | PY Var%)
   - Row grouping with bold subtotals at Gross Profit, EBIT, EBT, Net Income
   - Thousands or millions formatting with one decimal
   - Conditional formatting: green for favorable variances > 5%, red for unfavorable variances > 5%

   **Python/openpyxl**: Apply `Font(bold=True)` to subtotal rows, `PatternFill` for variance highlighting, `numbers.FORMAT_NUMBER_COMMA_SEPARATED1` for amounts.

   ### Google Sheets Implementation
   Apply formatting via Format > Number > Custom number format: `#,##0.0,` for thousands. Bold subtotals manually or via Apps Script for automation:
   ```javascript
   function formatReport() {
     var sheet = SpreadsheetApp.getActiveSheet();
     var subtotalRows = [10, 15, 20, 25]; // Row numbers for GP, EBIT, EBT, NI
     subtotalRows.forEach(function(row) {
       sheet.getRange(row, 1, 1, sheet.getLastColumn())
            .setFontWeight('bold')
            .setBorder(true, null, true, null, null, null);
     });
   }
   ```
   Conditional formatting: Format > Conditional formatting > apply to variance columns with custom formula rules.

**Output**: A complete FMCG P&L management report containing: (a) monthly and YTD income statements with Actual/Budget/Prior Year columns, (b) variance analysis with absolute and percentage differences, (c) product group breakdown matrix, (d) regional breakdown matrix, (e) full-year forecast blending actuals and budget, (f) formatted for executive presentation with conditional highlighting.

---

## S3 Cortex Logging

```json
{
  "text": "F-066 FMCG Report with SAP Data — executed for {company_name}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-066",
    "domain": "finance",
    "level": "tools",
    "complexity": "INTERMEDIATE",
    "company": "{company_name}",
    "implementation": "{excel|sheets|python}",
    "tags": ["business-evaluation", "excel-mastery", "fmcg", "sap-data", "pnl-reporting", "variance-analysis", "management-reporting"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Path — Tools Level

### WHEN
When executing financial reporting from transactional ERP data

### HOW (violation detection)
- Report produced without Chart of Accounts mapping validation (Step 2) -> violation (unmatched codes corrupt P&L)
- Variance percentages calculated without ABS(denominator) guard -> violation (division errors)
- P&L subtotals hardcoded instead of formula-linked -> violation (breaks on data refresh)
- Runner: manual review of output workbook structure, automated Cortex check for procedure completion tag

### CONNECT
- **Upstream**: Basic spreadsheet skills (cell references, SUM, SUMIF). Understanding of P&L structure (what Revenue, COGS, EBIT mean).
- **Downstream**: F-014 (Comprehensive Ratio Dashboard — margin and profitability KPIs from the P&L), F-082 (Pivot Table Analysis — alternative aggregation approach), F-083/F-084 (Dashboard procedures — visualization of P&L output).

### VERIFY
- [ ] Procedura a fost executată complet (toate steps)?
- [ ] Output spec deliverables verificate (P&L report, variance analysis, product/region breakdowns)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] Chart of Accounts mapping validated — zero unmatched account codes?
- [ ] Variance columns include ABS(denominator) guard against division errors?

**VK**: `[F-066] FMCG Report with SAP Data | {company_name} | {excel|sheets|python} | Tools complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-014 | Feeds into | P&L output provides margin and profitability KPIs for the Comprehensive Ratio Dashboard |
| F-005 | Related | Revenue forecasting logic is referenced when building full-year forecast (Step 8) |
| F-081 | Related | VLOOKUP/INDEX-MATCH patterns from F-081 are used in Chart of Accounts mapping |
| F-082 | Alternative | Pivot Table approach provides an alternative to SUMIFS-based aggregation |
| F-083 | Feeds into | Dashboard layout patterns consume the structured P&L output |
| F-070 | Related | Model mapping conventions (color coding, sheet architecture) apply when structuring the workbook |

---
---

# F-068: Loan Amortization Schedule Construction

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: BASIC
**Rule**: TRAIN-F-068

---

## S1 Problem

Constructing a loan amortization schedule is a fundamental skill for any financial analyst dealing with debt instruments, real estate finance, or corporate borrowing. Without this procedure, an AI evaluator cannot decompose periodic debt service payments into their interest and principal components, model balloon payments, evaluate refinancing scenarios, or project total interest costs. Every debt schedule in a 3-statement model (F-065) and every credit analysis (F-092) requires amortization logic.

**Situations covered**:
- Building period-by-period loan repayment schedules (monthly, quarterly, annual)
- Decomposing fixed payments into interest and principal portions
- Modeling balloon payment structures common in commercial real estate
- Evaluating impact of extra payments on loan duration and total interest
- Variable rate loan scenarios with periodic rate resets
- Producing summary statistics for loan comparison (total interest paid, payoff timeline)
- **NOT suitable for**: Zero-coupon or interest-only instruments (no principal amortization), revolving credit facilities (variable draw/repay), or mortgage-backed securities (require prepayment modeling beyond standard amortization)

---

## S2 Procedure

1. **Define loan parameters.** Collect the following inputs: Principal (PV) = loan amount (e.g., $500,000), Annual Interest Rate (r) = stated rate (e.g., 5.0%), Loan Term in Years (n) = e.g., 30, Payment Frequency = monthly (most common), annually, quarterly. Calculate derived values: periodic rate = annual_rate / periods_per_year (for monthly: r/12), total number of payments = years * periods_per_year (for 30-year monthly: 360).

   **Excel**: Set up an input panel with blue font (hard-coded inputs per F-070 color convention). Cells: B2 = Principal, B3 = Annual Rate, B4 = Years, B5 = Periods/Year. B6 = `=B3/B5` (periodic rate), B7 = `=B4*B5` (total periods).

   **Python**: `principal = 500000; annual_rate = 0.05; years = 30; freq = 12; periodic_rate = annual_rate / freq; total_periods = years * freq`.

   ### Google Sheets Implementation
   Identical setup to Excel. Use Data > Data validation > Dropdown for Payment Frequency (Monthly/Quarterly/Annual) in cell B5, then `=IF(B5="Monthly",12,IF(B5="Quarterly",4,1))` to derive periods_per_year. Color convention: Format > Text color > blue (#0000FF) for all hard-coded inputs.

2. **Calculate the fixed periodic payment using PMT.** The PMT function returns the constant payment amount that will fully amortize the loan.

   **Excel**: `=PMT(rate/12, nper*12, -pv)` — note the negative PV sign convention (loan received is positive inflow, so PV is entered as negative to get a positive payment). Example: `=PMT(0.05/12, 30*12, -500000)` = $2,684.11 per month.

   **Python**: `import numpy_financial as npf; payment = npf.pmt(0.05/12, 360, -500000)`.

   Manual formula: `PMT = (r * PV) / (1 - (1 + r)^(-n))` where r = periodic rate and n = total periods.

   ### Google Sheets Implementation
   PMT function is identical: `=PMT(B6, B7, -B2)` where B6 = periodic rate, B7 = total periods, B2 = principal. Sign convention is the same as Excel. Google Sheets also supports the optional `type` argument (0 = end of period, 1 = beginning of period) and `fv` argument, identical to Excel.

   **Precision note**: Apply `=ROUND(formula, 2)` to all monetary calculations to prevent cumulative floating-point errors over 360 periods.

3. **Build the amortization table structure.** Create columns: Period (1 to total_periods), Beginning Balance, Payment (fixed from Step 2), Interest Portion, Principal Portion, Ending Balance. Row 1: Beginning Balance = Principal. For each subsequent row: Beginning Balance = previous row's Ending Balance.

   **Excel**: Column A = period number (1 to 360), Column B = Beginning Balance (B2 = Principal, B3 = F2 from prior row), Column C = Payment (absolute reference to PMT cell), Column D = Interest, Column E = Principal, Column F = Ending Balance.

   ### Google Sheets Implementation
   Identical column structure. Use `=SEQUENCE(B7)` in Column A to auto-generate period numbers (Sheets-native function, no Excel equivalent). For the Beginning Balance column, B2 = Principal reference, B3 = `=F2` (previous row's ending balance). Lock the Payment reference with `=$C$1` (absolute reference, same as Excel).

4. **Calculate interest and principal portions for each period.** For period t: Interest = Beginning_Balance_t * periodic_rate. Principal = Payment - Interest. Ending Balance = Beginning Balance - Principal.

   **Excel per-period functions**: `=IPMT(rate/12, period, nper*12, -pv)` returns the interest portion; `=PPMT(rate/12, period, nper*12, -pv)` returns the principal portion. Manual: `Interest_t = Beginning_Balance_t * (rate/12)`. `Principal_t = Payment - Interest_t`. `Ending_Balance_t = Beginning_Balance_t - Principal_t`. Verify: `IPMT + PPMT = PMT` for every period.

   **Python**: `for t in range(1, n+1): interest = balance * r_monthly; principal = payment - interest; balance -= principal`.

   ### Google Sheets Implementation
   IPMT and PPMT are available in Google Sheets with identical syntax: `=IPMT(rate, period, nper, pv)` and `=PPMT(rate, period, nper, pv)`. For the manual approach (recommended for transparency): D2 = `=B2*$B$6` (interest), E2 = `=$C$1-D2` (principal), F2 = `=B2-E2` (ending balance). These formulas are identical to Excel.

5. **Validate the schedule totals.** Sum all Interest portions = Total Interest Paid over the loan life. Sum all Principal portions must equal the original Principal (PV). Total_Payments = PMT * total_periods. Total_Interest = Total_Payments - Principal.

   **Excel**: `=SUM(Interest_Column)` should equal `=PMT(rate/12, nper*12, -pv)*nper*12 - pv`. Ending Balance after the last period must be $0.00 (or within $0.01 due to rounding).

   ### Google Sheets Implementation
   Identical SUM validation formulas. Add a check cell: `=IF(ABS(F361)<0.01, "BALANCED", "ERROR: "&TEXT(F361,"$#,##0.00"))` where F361 is the final Ending Balance. Use conditional formatting to highlight green (balanced) or red (error).

6. **Add cumulative columns and visualizations.** Add columns: Cumulative Interest Paid = running sum of interest, Cumulative Principal Paid = running sum of principal, Remaining Balance (same as Ending Balance). Create visualizations showing payment composition over time.

   **Excel**: Cumulative Interest: `=SUM($D$2:D2)` (expanding range). Create a stacked area chart: Interest vs. Principal composition per payment. Add a line chart of Remaining Balance declining over time.

   **Python**: `plt.stackplot(periods, interest_series, principal_series, labels=['Interest', 'Principal'])`.

   ### Google Sheets Implementation
   Cumulative formulas identical: `=SUM($D$2:D2)`. For charts: Insert > Chart > select "Stacked area chart", data range = Period + Interest + Principal columns. For the balance decline: Insert > Chart > "Line chart" with Period on X-axis and Remaining Balance on Y-axis. Google Sheets charts are interactive (hover for values) and auto-update. For advanced visualization, use the `=SPARKLINE()` function inline: `=SPARKLINE(F2:F361, {"charttype","line"})` shows a mini balance decline chart within a single cell.

7. **Handle special scenarios.** Three common variants:
   - **(a) Balloon payment**: Amortize over a longer period (e.g., 30 years) but with a balloon at year 10. Calculate PMT based on 30-year amortization, then at period 120, the Ending Balance becomes the balloon payment due.
   - **(b) Extra payments**: Add an "Extra Payment" column. Subtract from Ending Balance, which shortens the loan term and reduces total interest.
   - **(c) Variable rate**: Replace the fixed rate with a rate lookup per period (e.g., rate resets every 12 months). Recalculate PMT at each reset using the remaining balance and remaining periods. **WARNING — Negative Amortization**: If the periodic rate increases such that Interest_t > Payment, the balance grows instead of shrinking (negative amortization). Add a check: `=IF(Interest > Payment, "NEG AMORT", "")` and recalculate PMT at each rate reset using remaining balance and remaining periods.

   ### Google Sheets Implementation
   **(a) Balloon**: Add conditional formula in Ending Balance column: `=IF(A2=120, B2-E2, B2-E2-0)` or simply flag the balloon amount with `=INDEX(F:F, 120)`. **(b) Extra payments**: Add column G for extra payment amount. Modify Ending Balance: `=B2-E2-G2`. Use Data validation > Checkbox in column G header to toggle extra payments on/off. **(c) Variable rate**: Create a "Rate Schedule" sheet with Period and Rate columns. Use `=VLOOKUP(A2, RateSchedule!A:B, 2, TRUE)` to look up the applicable rate per period (TRUE for approximate match gets the most recent rate reset). Alternatively, `=XLOOKUP(A2, RateSchedule!A:A, RateSchedule!B:B, , -1)` for exact-or-previous matching.

8. **Produce summary statistics.** Report: Total Amount Paid, Total Interest Paid, Interest as % of Principal, Monthly Payment Amount, Effective Annual Rate (if compounding differs from annual), payoff date. For extra payment scenarios: Months Saved, Interest Saved. Format as a summary box at the top of the schedule.

   **Excel**: `="Total Interest: "&TEXT(SUM(InterestColumn),"$#,##0.00")`.

   **Python**: f-string formatting with locale for currency.

   ### Google Sheets Implementation
   Identical TEXT formatting: `="Total Interest: "&TEXT(SUM(D:D),"$#,##0.00")`. For a dashboard-style summary, use `={"Metric","Value"; "Total Paid",TEXT(SUM(C:C),"$#,##0"); "Total Interest",TEXT(SUM(D:D),"$#,##0"); "Interest %",TEXT(SUM(D:D)/B2,"0.0%"); "Monthly PMT",TEXT(C2,"$#,##0.00")}` which creates an instant multi-row summary array. Google Finance integration: `=GOOGLEFINANCE("CURRENCY:USDEUR")` can convert the summary to another currency if needed.

**Output**: A complete loan amortization schedule containing: (a) period-by-period breakdown of payment, interest, principal, and balance for the full loan term, (b) cumulative interest and principal tracking, (c) stacked area chart of payment composition, (d) balance decline chart, (e) balloon payment and variable rate variants, (f) summary statistics panel.

---

## S3 Cortex Logging

```json
{
  "text": "F-068 Loan Amortization Schedule — constructed for {loan_type} ${principal} at {rate}% for {years} years",
  "collection": "business_evaluation",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-068",
    "domain": "finance",
    "level": "tools",
    "complexity": "BASIC",
    "loan_principal": "{principal}",
    "annual_rate": "{rate}",
    "term_years": "{years}",
    "implementation": "{excel|sheets|python}",
    "tags": ["business-evaluation", "excel-mastery", "loan-amortization", "tvm", "debt-modeling", "pmt"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Path — Tools Level

### WHEN
When constructing any debt repayment schedule or modeling loan instruments

### HOW (violation detection)
- Final Ending Balance is not within $0.01 of zero -> violation (schedule arithmetic error)
- IPMT + PPMT does not equal PMT for any period -> violation (formula inconsistency)
- Sign convention error (positive payment but negative interest) -> violation (TVM convention not applied)
- Summary Total Interest + Principal does not equal Total Payments -> violation (aggregation error)
- Runner: automated validation of final balance cell, manual review of chart output

### CONNECT
- **Upstream**: None — self-contained foundational procedure. Understanding of basic interest concepts (principal, rate, time) assumed.
- **Downstream**: F-080 (Excel Financial Functions Mastery — PMT/IPMT/PPMT practical application), F-065 (3-Statement Model — debt schedule component), F-092 (Credit Analysis — debt service coverage calculations).

### VERIFY
- [ ] Procedura a fost executată complet (toate steps)?
- [ ] Output spec deliverables verificate (amortization table, charts, summary stats)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] Final Ending Balance is within $0.01 of zero?
- [ ] IPMT + PPMT = PMT verified for all periods?

**VK**: `[F-068] Loan Amortization Schedule | ${principal} | {rate}% | {years}yr | {excel|sheets|python} | Tools complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-080 | Feeds into | Amortization is a practical application of PMT/IPMT/PPMT covered in TVM mastery |
| F-065 | Feeds into | Debt schedule component of 3-Statement Model requires amortization logic |
| F-092 | Feeds into | Credit Analysis uses amortization output for debt service coverage ratio (DSCR) |
| F-029/F-030 | Related | NPV/IRR calculations reference TVM concepts shared with loan amortization |
| F-070 | Related | Model architecture conventions (color coding, input isolation) apply to the amortization workbook |

---
---

# F-070: Financial Model Mapping and Sheet Architecture

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-070

---

## S1 Problem

Financial models without a standardized architecture become unmaintainable spaghetti — formulas reference arbitrary cells, inputs are scattered across sheets, circular references break silently, and new users cannot navigate the workbook. This procedure establishes the industry-standard sheet taxonomy, color coding convention, scenario toggling, cross-sheet referencing, and integrity checking framework used by investment banks, Big 4 firms, and corporate FP&A teams. Without this meta-procedure, every model built in F-065 through F-089 lacks structural integrity. This is the architecture blueprint that must be executed before populating any financial model.

**Situations covered**:
- Designing workbook structure for any financial model (DCF, LBO, 3-statement, merger)
- Implementing color coding convention (blue inputs, black formulas, green cross-links)
- Building CHOOSE-based scenario toggles for base/bull/bear analysis
- Replacing VLOOKUP with INDEX-MATCH for robust cross-sheet references
- Creating dynamic named ranges with OFFSET for expandable models
- Resolving circular references in integrated financial statements
- Building model integrity check dashboards
- **NOT suitable for**: Single-sheet ad-hoc calculations, non-financial workbooks, or models with fewer than 3 interconnected sheets (overhead of full architecture not justified)

### Quick Reference (3 Non-Negotiable Conventions)
1. **Color coding**: Blue font = hardcoded input | Black = formula | Green = cross-sheet link
2. **Input isolation**: ALL inputs on the Inputs sheet — no hardcoded numbers in formula sheets
3. **Integrity checks**: Every financial statement must have balance/reconciliation checks (BS balance, CFS cash match, RE reconciliation)

---

## S2 Procedure

1. **Define the model sheet taxonomy.** Establish the standard sheet structure for any financial model:
   - **(a) Cover** — model name, version, date, author, disclaimer
   - **(b) Inputs/Assumptions** — all hard-coded inputs isolated here (blue font convention), grouped by category (revenue drivers, cost assumptions, capital structure, tax, working capital days)
   - **(c) Historical Data** — 3-5 years of actual financial statements, untouched source data
   - **(d) Calculations/Engine** — all intermediate computations, schedules (revenue build, D&A, working capital, debt)
   - **(e) Income Statement** — projected IS pulling from calculations
   - **(f) Balance Sheet** — projected BS pulling from calculations
   - **(g) Cash Flow Statement** — projected CFS derived from IS and BS changes
   - **(h) Valuation** — DCF, multiples, or other valuation output
   - **(i) Sensitivity** — data tables and scenario analysis (per F-069)
   - **(j) Output/Dashboard** — summary charts, KPIs, executive view

   ### Google Sheets Implementation
   Google Sheets supports multiple sheets within a workbook identically to Excel. Create sheets using the "+" tab at the bottom. Key difference: Google Sheets does not support VBA macros — use Apps Script (Tools > Script editor) for any automation. Sheet protection: Right-click tab > Protect sheet > set permissions per user (useful for locking Historical Data and Calculations from accidental edits). For large models, consider sheet grouping by color-coding the tabs: Right-click tab > Change color (blue for inputs, grey for calculations, green for output statements).

2. **Implement the color coding convention.** Apply universally across all sheets:
   - Blue font (#0000FF) = hard-coded input (typed by the user)
   - Black font (#000000) = formula (calculated)
   - Green font (#008000) = cross-sheet link (pulls from another sheet)
   - Red font (#FF0000) = error or check value

   **Python/openpyxl**: `blue_font = Font(color="0000FF"); black_font = Font(color="000000"); green_font = Font(color="008000")`. Apply programmatically: any cell containing a formula referencing another sheet gets green font; any cell with no formula and a numeric value gets blue font.

   ### Google Sheets Implementation
   Apply manually via Format > Text color, or automate with Apps Script:
   ```javascript
   function applyColorConvention() {
     var sheet = SpreadsheetApp.getActiveSheet();
     var range = sheet.getDataRange();
     var formulas = range.getFormulas();
     var values = range.getValues();
     for (var i = 0; i < formulas.length; i++) {
       for (var j = 0; j < formulas[i].length; j++) {
         var cell = sheet.getRange(i + 1, j + 1);
         if (formulas[i][j] && formulas[i][j].indexOf('!') > -1) {
           cell.setFontColor('#008000'); // Green: cross-sheet reference
         } else if (formulas[i][j]) {
           cell.setFontColor('#000000'); // Black: formula
         } else if (values[i][j] !== '' && !isNaN(values[i][j])) {
           cell.setFontColor('#0000FF'); // Blue: hard-coded input
         }
       }
     }
   }
   ```
   Note: This Apps Script processes the entire sheet and should be run once after model setup, not on every edit (performance). For ongoing enforcement, use onEdit trigger with targeted cell checks.

3. **Build the CHOOSE function for scenario selection.** On the Inputs sheet, create a scenario selector cell (e.g., B2) with Data Validation dropdown: 1 = Base, 2 = Bull, 3 = Bear. For each assumption, define three values and use CHOOSE to select the active one.

   **Excel**: `=CHOOSE($B$2, BaseValue, BullValue, BearValue)`. Example: Revenue Growth = `=CHOOSE(Inputs!$B$2, 5%, 8%, 2%)`. This single cell toggle switches the entire model between scenarios.

   **Python**: `scenarios = {'Base': {'rev_growth': 0.05}, 'Bull': {'rev_growth': 0.08}, 'Bear': {'rev_growth': 0.02}}; active = scenarios[selected_scenario]`.

   ### Google Sheets Implementation
   CHOOSE function is identical: `=CHOOSE(Inputs!$B$2, 0.05, 0.08, 0.02)`. For the dropdown, use Data > Data validation > Dropdown (from a range or list of items): create a named list "1 - Base, 2 - Bull, 3 - Bear". Alternative Sheets-native approach: use `=SWITCH(Inputs!$B$2, "Base", 0.05, "Bull", 0.08, "Bear", 0.02)` which allows text labels instead of numeric indices (more readable, Sheets-native — SWITCH also available in Excel 365+). For more than 3 scenarios, use `=IFS()` or `=VLOOKUP(Inputs!$B$2, ScenarioTable, 2, FALSE)`.

4. **Implement INDEX-MATCH for cross-sheet references.** Replace all VLOOKUP-based cross-sheet links with INDEX-MATCH for robustness (column insertion won't break references).

   **Excel**: `=INDEX(HistoricalData!$C$5:$G$50, MATCH("Revenue", HistoricalData!$B$5:$B$50, 0), MATCH(2024, HistoricalData!$C$4:$G$4, 0))`. For linking calculated values to output sheets: `=INDEX(Calculations!OutputRange, MATCH(LineItemLabel, Calculations!LabelRange, 0), MATCH(YearHeader, Calculations!YearRange, 0))`.

   **Python/openpyxl**: Implemented as data lookups during model generation rather than live formulas.

   ### Google Sheets Implementation
   INDEX-MATCH syntax is identical to Excel. Additionally, Google Sheets supports XLOOKUP natively: `=XLOOKUP("Revenue", HistoricalData!B:B, HistoricalData!C:G)` which returns the entire row for "Revenue". XLOOKUP is cleaner for single-dimension lookups. For two-dimensional lookups (specific year + specific line item), INDEX-MATCH-MATCH remains the preferred pattern. Note: `=FILTER()` is a powerful Sheets-native alternative: `=FILTER(HistoricalData!C5:G50, HistoricalData!B5:B50="Revenue")` returns the entire Revenue row.

5. **Build OFFSET-based dynamic named ranges.** For any range that grows (e.g., years added to the projection), use OFFSET to create auto-expanding named ranges.

   **Excel**: `=OFFSET(Calculations!$C$5, 0, 0, COUNTA(Calculations!$B$5:$B$100), COUNTA(Calculations!$C$4:$Z$4))`. Name this range "ProjectionData" via Name Manager. Use in all charts and summary tables. **Caveat**: COUNTA is unreliable if the label column contains blank rows within data. For sparse data, use `MATCH(REPT("z",255), range)` or convert the range to a structured Table reference which auto-expands.

   **Python**: Track range boundaries dynamically using `ws.max_row` and `ws.max_column` from openpyxl.

   ### Google Sheets Implementation
   Google Sheets does NOT support OFFSET in named ranges (named ranges must reference static cell ranges). Workarounds:
   - **Option 1**: Use `=INDIRECT("Calculations!C5:G"&COUNTA(Calculations!B:B)+4)` within formulas (INDIRECT is volatile, avoid in large models).
   - **Option 2**: Use open-ended ranges: name "ProjectionData" as `Calculations!C5:Z100` (oversize the range — empty cells are ignored by most functions).
   - **Option 3 (recommended)**: Use `=ARRAYFORMULA()` with dynamic array functions — Google Sheets handles spill ranges natively, so formulas auto-expand without named ranges.
   - For charts: Google Sheets charts automatically detect data range expansion if the chart source references an entire column (e.g., `C:C` instead of `C5:C50`).

6. **Implement circular reference prevention.** Identify the common circularity: Interest Expense (IS) depends on Debt Balance (BS), which depends on Cash Flow (CFS), which depends on Interest Expense.

   Resolution strategies:
   - **(a) Preferred**: Calculate Interest Expense using beginning-of-period debt balance (no circularity). Formula: `Interest_t = Debt_Balance_{t-1} * Interest_Rate`.
   - **(b) Alternative**: Use iterative calculation (Excel: File > Options > Formulas > Enable iterative calculation, max iterations = 100, max change = 0.001).
   - **(c) Circuit breaker**: `=IF(CircuitBreaker=1, IterativeCalc, StaticCalc)` where CircuitBreaker is a toggle.
   Document which method is used on the Cover sheet.

   ### Google Sheets Implementation
   Google Sheets supports iterative calculation: File > Settings > Calculation > Iterative calculation ON (set max iterations = 100, max change = 0.001). This is functionally identical to Excel's iterative mode. **However**, iterative calculation in Sheets applies globally to the entire spreadsheet (not per-formula), so enabling it can mask unintended circular references. **Recommendation**: Use method (a) — beginning-of-period debt — as the default to avoid iterative calculation entirely. If iterative mode is required, add a cell on the Cover sheet documenting: 'Iterative Calculation: ENABLED | Method: Beginning-of-period debt (preferred)' as a manual documentation note. Google Sheets does not expose iterative calculation status to formulas — use this documentation cell as a team reminder.

7. **Create the model map (visual documentation).** Build a schematic showing data flow between sheets: Inputs -> Calculations -> IS/BS/CFS -> Valuation -> Output. For each sheet, list: purpose, key inputs (from which sheet), key outputs (to which sheet), number of rows/columns.

   **Excel**: Format as a flowchart or table on the Cover sheet.

   **Python**: Generate a mermaid diagram: `graph LR; Inputs --> Calculations; Calculations --> IS; Calculations --> BS; IS --> CFS; BS --> CFS; CFS --> BS; IS --> Valuation`.

   ### Google Sheets Implementation
   Use Insert > Drawing to create a flowchart directly in the Cover sheet. Alternatively, use Insert > Chart > Organizational chart type (limited but native). For a more professional model map, create a Google Drawing (drive.google.com/drawings) and embed it via Insert > Drawing > From Drive. Apps Script can auto-generate a text-based model map:
   ```javascript
   function generateModelMap() {
     var ss = SpreadsheetApp.getActiveSpreadsheet();
     var sheets = ss.getSheets();
     var map = "MODEL MAP\n=========\n";
     sheets.forEach(function(sheet) {
       map += sheet.getName() + " | Rows: " + sheet.getLastRow() +
              " | Cols: " + sheet.getLastColumn() + "\n";
     });
     ss.getSheetByName('Cover').getRange('A20').setValue(map);
   }
   ```

8. **Add integrity checks and error trapping.** On a dedicated "Checks" sheet or at the bottom of each statement sheet, add:
   - **(a) BS Balance Check**: `=IF(ABS(Total_Assets - Total_Liabilities_Equity) < 0.01, "OK", "ERROR: BS UNBALANCED")`
   - **(b) CFS Cash Check**: `=IF(ABS(CFS_Ending_Cash - BS_Cash) < 0.01, "OK", "ERROR: CASH MISMATCH")`
   - **(c) Retained Earnings Check**: `=IF(ABS(RE_End - (RE_Begin + NI - Dividends)) < 0.01, "OK", "ERROR: RE MISMATCH")`
   - **(d) Aggregate**: `=IF(COUNTIF(CheckRange, "ERROR*") > 0, "MODEL HAS ERRORS", "ALL CHECKS PASS")`. Color-code green/red.

   ### Google Sheets Implementation
   Formulas are identical. For color coding, use conditional formatting: Format > Conditional formatting > Text contains "ERROR" -> Red background, Text is exactly "OK" -> Green background. Add a dashboard check using `=ARRAYFORMULA({"Check","Status"; "BS Balance",IF(ABS(IS!B50-BS!B50)<0.01,"OK","ERROR"); "Cash Match",IF(ABS(CFS!B30-BS!B5)<0.01,"OK","ERROR"); "RE Check",IF(ABS(BS!B40-(BS!B39+IS!B45-BS!B41))<0.01,"OK","ERROR")})` which creates an instant check dashboard as a single array formula. For email alerts on errors, add an Apps Script trigger:
   ```javascript
   function checkModelIntegrity() {
     var status = SpreadsheetApp.getActiveSpreadsheet()
       .getSheetByName('Checks').getRange('B1').getValue();
     if (status.indexOf('ERROR') > -1) {
       MailApp.sendEmail('analyst@company.com', 'Model Error Alert', status);
     }
   }
   ```

9. **Generate the model template as a workbook file.** Create the workbook with all sheets pre-structured: headers, row labels, formula placeholders, color coding applied, named ranges defined, data validation dropdowns set, conditional formatting for checks. The template should be company-agnostic — populate with sample data to demonstrate structure, but design so that swapping historical data and assumptions produces a new model.

   **Python/openpyxl**: Create workbook with all sheets, apply formatting, define named ranges, set data validation, set print areas. Save as `.xlsx`.

   ### Google Sheets Implementation
   Create a template spreadsheet and use File > Make a copy for each new model. For automated template generation via Apps Script:
   ```javascript
   function createModelTemplate() {
     var ss = SpreadsheetApp.create('Financial Model Template');
     var sheets = ['Cover','Inputs','Historical','Calculations',
                   'Income Statement','Balance Sheet','Cash Flow',
                   'Valuation','Sensitivity','Dashboard','Checks'];
     sheets.forEach(function(name, i) {
       if (i === 0) { ss.getSheets()[0].setName(name); }
       else { ss.insertSheet(name); }
     });
     // Apply tab colors
     ss.getSheetByName('Inputs').setTabColor('#0000FF');
     ss.getSheetByName('Calculations').setTabColor('#808080');
     ss.getSheetByName('Dashboard').setTabColor('#008000');
     ss.getSheetByName('Checks').setTabColor('#FF0000');
     return ss.getUrl();
   }
   ```
   For distribution, publish as a Google Sheets template via File > Template gallery (requires Google Workspace admin).

**Output**: A financial model architecture specification and template containing: (a) 8-10 sheet workbook with standardized naming and ordering, (b) color-coded input/formula/link convention applied throughout, (c) CHOOSE-based scenario toggle, (d) INDEX-MATCH cross-references replacing VLOOKUP, (e) OFFSET/dynamic named ranges for charts, (f) circular reference resolution documented, (g) model map/flowchart, (h) integrity check dashboard, (i) ready-to-populate template file.

---

## S3 Cortex Logging

```json
{
  "text": "F-070 Financial Model Mapping and Sheet Architecture — template generated for {model_type}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-070",
    "domain": "finance",
    "level": "tools",
    "complexity": "INTERMEDIATE",
    "model_type": "{dcf|lbo|3-statement|merger|custom}",
    "sheet_count": "{number}",
    "implementation": "{excel|sheets|python}",
    "tags": ["business-evaluation", "excel-mastery", "financial-modeling", "model-architecture", "sheet-design", "color-convention", "integrity-checks"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Path — Tools Level (meta-procedure: must execute BEFORE any financial model construction)

### WHEN
When building any multi-sheet financial model workbook

### HOW (violation detection)
- Financial model created without Inputs sheet isolating all hard-coded values -> violation (inputs scattered = unmaintainable)
- Blue font convention not applied to input cells -> violation (users cannot distinguish inputs from formulas)
- VLOOKUP used instead of INDEX-MATCH for cross-sheet references -> violation (fragile references)
- No integrity check sheet or checks not covering BS balance + cash match + RE check -> violation (silent model errors)
- Circular reference present without documented resolution method on Cover sheet -> violation (unpredictable behavior)
- Runner: model template audit (check sheet count, naming, color convention, checks sheet presence)

### CONNECT
- **Upstream**: Basic spreadsheet navigation. Understanding of financial statement structure (IS, BS, CFS) from L1 procedures (F-010, F-011).
- **Downstream**: F-065 (3-Statement Model — populates the architecture with content), F-069 (Sensitivity Analysis — uses the CHOOSE scenario mechanism), F-066 (FMCG Report — applies color coding and structure conventions), all model-building procedures in L3-L5.

### VERIFY
- [ ] Procedura a fost executată complet (toate steps)?
- [ ] Output spec deliverables verificate (workbook template with all sheets, color coding, checks)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] All 3 integrity checks present (BS balance, CFS cash match, RE reconciliation)?
- [ ] Color coding convention applied consistently (blue=input, black=formula, green=cross-link)?

**VK**: `[F-070] Financial Model Architecture | {model_type} | {sheet_count} sheets | {excel|sheets|python} | Tools complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-065 | Feeds into | 3-Statement Model is built on top of the architecture defined here |
| F-069 | Feeds into | Sensitivity Analysis uses the CHOOSE scenario mechanism from Step 3 |
| F-066 | Related | FMCG Report applies the color coding and structure conventions |
| F-081 | Related | Lookup function patterns (INDEX-MATCH) are the cross-sheet reference standard from Step 4 |
| F-089 | Feeds into | Integrated financial model depends on this architecture |
| F-083/F-084 | Related | Dashboard procedures consume the Output/Dashboard sheet structure |

---
---

# F-080: Excel Financial Functions Mastery (TVM)

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: BASIC
**Rule**: TRAIN-F-080

---

## S1 Problem

The Time Value of Money (TVM) is the single most fundamental concept in finance — every valuation, every investment decision, every loan calculation rests on the principle that a dollar today is worth more than a dollar tomorrow. Without mastery of PV, FV, PMT, NPV, IRR, XNPV, and XIRR functions, an AI evaluator cannot perform DCF valuations (F-039), evaluate capital projects (F-029/F-030), construct amortization schedules (F-068), or analyze investment returns. This procedure builds the complete TVM function toolkit with sign convention discipline, edge case awareness, and practical calculator construction.

**Situations covered**:
- Present value calculations for annuities, lump sums, and bonds
- Future value projections for savings, investments, and compounding
- Periodic payment calculation for loans and savings plans
- Solving for interest rate or number of periods
- Net Present Value and Internal Rate of Return for project evaluation
- XNPV and XIRR for irregular (non-periodic) cash flow dates
- Building a comprehensive TVM calculator workbook
- **NOT suitable for**: Option pricing (requires Black-Scholes, not basic TVM), real options analysis, or stochastic interest rate modeling (requires Monte Carlo simulation)

---

## S2 Procedure

1. **Establish sign convention and TVM fundamentals.** All Excel financial functions use the sign convention: negative values = cash outflows (money you pay), positive values = cash inflows (money you receive). Document a reference table showing the convention for each scenario:
   - Loan received: PV = +100,000 (inflow to you), payments = negative (outflow from you)
   - Investment made: PV = -50,000 (outflow from you), future value = positive (inflow to you)
   - Misunderstanding sign convention is the #1 source of errors in TVM calculations.

   ### Google Sheets Implementation
   Sign convention is identical in Google Sheets. Create a reference table on a "Conventions" sheet using `={"Scenario","PV","PMT","FV"; "Loan Received","+","-","0"; "Investment Made","-","0","+"; "Savings Plan","0","-","+"; "Bond Purchase","-","+","+"}` as an array formula to produce an instant reference grid.

2. **Present Value (PV) calculations.** Formula: `=PV(rate, nper, pmt, [fv], [type])`.

   **Excel use cases**:
   - (a) PV of receiving $1,000/year for 10 years at 8%: `=PV(0.08, 10, -1000)` = $6,710.08
   - (b) Bond pricing: $50 semiannual coupon, 10 years, $1,000 face, 6% yield: `=PV(0.06/2, 10*2, -50, -1000)` = $1,000.00 (par)
   - (c) PV of $500,000 in 20 years at 7%: `=PV(0.07, 20, 0, -500000)` = $129,221.83

   **Python**: `npf.pv(rate=0.08, nper=10, pmt=-1000)`.

   ### Google Sheets Implementation
   PV function syntax and behavior are identical: `=PV(0.08, 10, -1000)`. Google Sheets also supports the optional `type` argument (0 = end-of-period ordinary annuity, 1 = beginning-of-period annuity due). No differences from Excel. For real-time bond pricing with market rates, combine with `=GOOGLEFINANCE("CURRENCY:USDEUR")` for cross-currency bond analysis or use the Sheets add-on "Bond Valuation Calculator" for more complex structures.

3. **Future Value (FV) calculations.** Formula: `=FV(rate, nper, pmt, [pv], [type])`.

   **Excel use cases**:
   - (a) Invest $5,000/year for 30 years at 10%: `=FV(0.10, 30, -5000)` = $822,470.11
   - (b) Deposit $100,000 today, compounding at 6% for 15 years: `=FV(0.06, 15, 0, -100000)` = $239,655.82
   - (c) Save $500/month at 8% annual for 25 years: `=FV(0.08/12, 25*12, -500)` = $475,513.31

   **Python**: `npf.fv(rate=0.08/12, nper=300, pmt=-500, pv=0)`.

   ### Google Sheets Implementation
   FV function is identical: `=FV(0.10, 30, -5000)`. For retirement planning calculators in Sheets, add a slider using Data validation > Slider (available in modern Sheets) for the annual contribution amount, with `=FV(B2/12, B3*12, -B4)` dynamically updating. Google Finance integration: `=FV(GOOGLEFINANCE("TNX")/100/12, 25*12, -500)` uses the live 10-year Treasury yield as the discount rate (TNX = CBOE 10-Year Treasury Note Yield Index).

4. **Payment (PMT) and period/rate solving.** Three complementary functions:

   **PMT**: `=PMT(rate, nper, pv, [fv], [type])`. See F-068 for detailed amortization use.

   **NPER** (number of periods): `=NPER(rate, pmt, pv, [fv], [type])`. Example: How long to pay off $200,000 at 4.5% with $1,500/month? `=NPER(0.045/12, -1500, 200000)` = 188.7 months (15.7 years).

   **RATE** (interest rate per period): `=RATE(nper, pmt, pv, [fv], [type])`. Example: $20,000 car loan, $400/month, 5 years: `=RATE(60, -400, 20000)*12` = 7.42% annual rate. **IMPORTANT**: RATE returns the per-period rate. For monthly payments, multiply by 12 to get the annual rate. Forgetting this conversion is one of the most common TVM errors.

   **Python**: `npf.nper(0.045/12, -1500, 200000)` and `npf.rate(60, -400, 20000)*12`.

   ### Google Sheets Implementation
   PMT, NPER, and RATE are all identical in Google Sheets. No syntax differences. For an interactive loan comparison tool, use `=ARRAYFORMULA(PMT(B2:B5/12, C2:C5*12, -D2:D5))` to calculate payments for multiple loan options simultaneously (ARRAYFORMULA spills results, unlike Excel which requires Ctrl+Shift+Enter for legacy array entry or dynamic arrays in 365+). RATE note: if RATE fails to converge (returns #NUM!), wrap with `=IFERROR(RATE(...), "No solution")` — identical behavior to Excel.

5. **Net Present Value (NPV) and Internal Rate of Return (IRR).** CRITICAL: Excel's NPV function assumes cash flows start at period 1, not period 0.

   **NPV**: `=NPV(rate, value1, value2, ...) + initial_investment`. For an investment of -$100,000 today with $30,000/year for 5 years: `=-100000 + NPV(0.10, 30000, 30000, 30000, 30000, 30000)` = $13,723.60.

   **IRR**: `=IRR(values, [guess])` where values include period 0. Example: `=IRR({-100000, 30000, 30000, 30000, 30000, 30000})` = 15.24%.

   **WARNING — Multiple IRRs**: For non-conventional cash flows (multiple sign changes), IRR may have multiple mathematical solutions. Always verify: compute `=NPV(IRR_result, cashflows)` — it must approximately equal the initial investment. Cross-reference F-030 for MIRR approach.

   **Python**: `npf.npv(0.10, [30000]*5) - 100000` and `npf.irr([-100000] + [30000]*5)`.

   ### Google Sheets Implementation
   NPV and IRR are identical in Google Sheets, including the period-0 trap (NPV starts at period 1). The workaround is the same: `=initial_investment + NPV(rate, period1_cf, period2_cf, ...)`. For array input to IRR: `=IRR(A1:A6)` where A1 contains -100000 and A2:A6 contain 30000. Google Sheets also supports `=IRR({-100000, 30000, 30000, 30000, 30000, 30000})` with curly braces for inline arrays (identical to Excel). Note: for multiple IRR scenarios (non-conventional cash flows), both Excel and Sheets may converge to different roots depending on the `guess` parameter — document this edge case.

6. **Extended NPV and IRR for irregular dates (XNPV, XIRR).** When cash flows do not occur at regular intervals, standard NPV/IRR cannot be used.

   **XNPV**: `=XNPV(rate, values, dates)` discounts each cash flow by the exact number of days. Example: initial outflow on 2025-01-15, inflows on 2025-07-22, 2026-03-10, 2027-11-01. `=XNPV(0.10, {-100000, 40000, 45000, 50000}, {"2025-01-15", "2025-07-22", "2026-03-10", "2027-11-01"})`.

   **XIRR**: `=XIRR(values, dates, [guess])` — solves for the rate that makes XNPV = 0.

   **Python**: Use `scipy.optimize.brentq` with a custom XNPV function, or `pyxirr` library: `from pyxirr import xirr; xirr(dates, values)`.

   ### Google Sheets Implementation
   XNPV and XIRR are available in Google Sheets with identical syntax: `=XNPV(0.10, B2:B5, A2:A5)` and `=XIRR(B2:B5, A2:A5)`. Date handling note: Google Sheets uses the same serial date system as Excel (1900 date system). Dates can be entered as strings ("2025-01-15") or as DATE function output (`=DATE(2025,1,15)`). **Key Sheets advantage**: `=GOOGLEFINANCE("GOOG", "price", DATE(2025,1,15), DATE(2027,11,1), "DAILY")` can pull actual stock prices for the exact dates in the XIRR calculation, enabling real-time investment return analysis with actual market data.

   **Sanity check**: After computing XIRR, verify with `=XNPV(XIRR_result, values, dates)` — must be approximately zero. If XIRR returns #NUM!, try different guess values (0.1, 0.5, -0.1).

7. **Build a comprehensive TVM calculator workbook.** Create a single-sheet calculator with sections for each function:
   - (a) PV Calculator — inputs: rate, nper, pmt, fv; output: PV
   - (b) FV Calculator — inputs: rate, nper, pmt, pv; output: FV
   - (c) PMT Calculator — inputs: rate, nper, pv, fv; output: PMT
   - (d) NPER Calculator — inputs: rate, pmt, pv, fv; output: NPER
   - (e) RATE Calculator — inputs: nper, pmt, pv, fv; output: RATE
   - (f) NPV/IRR Calculator — inputs: discount rate, cash flow array; outputs: NPV and IRR
   - (g) XNPV/XIRR Calculator — inputs: rate, cash flows with dates; outputs: XNPV and XIRR

   Add data validation to prevent common errors (e.g., rate > 100%, nper < 0).

   **Python/openpyxl**: Use `DataValidation` objects with min/max constraints.

   ### Google Sheets Implementation
   Build as a single Google Sheet with named sections. Use Data > Data validation for input constraints:
   - Rate: Data validation > Number > Between 0 and 1 (for decimal) or 0 and 100 (for percentage format)
   - NPER: Data validation > Number > Greater than 0
   - For the NPV/IRR calculator with variable-length cash flow arrays, use `=COUNTA(B10:B50)` to detect how many cash flows have been entered, then `=NPV(B8, INDIRECT("B10:B"&(9+COUNTA(B10:B50))))` for dynamic range NPV calculation.
   - Add checkboxes (Insert > Checkbox) for the `type` parameter (ordinary annuity vs. annuity due) — checkbox returns TRUE/FALSE which maps to 1/0 for the type argument.
   - Google Sheets template gallery: publish the TVM calculator as a template for reuse (File > Template gallery).

8. **Document common pitfalls and edge cases.** Create a reference section covering:
   - (a) NPV period-0 trap (Excel/Sheets NPV starts at period 1)
   - (b) Sign convention errors (forgetting to negate PV or PMT)
   - (c) Rate frequency mismatch (using annual rate with monthly nper without dividing by 12)
   - (d) Begin/end mode (type=0 for end-of-period, type=1 for beginning-of-period — annuity due vs. ordinary annuity)
   - (e) IRR convergence failure (provide a guess closer to expected rate)
   - (f) Multiple IRR problem (non-conventional cash flows — see F-030 step 2)

   ### Google Sheets Implementation
   All pitfalls are identical between Excel and Google Sheets. Create the reference as a formatted table using `=ARRAYFORMULA()` or a simple markdown-style layout. One Sheets-specific addition: Google Sheets has stricter type coercion — `=PV("0.08", 10, -1000)` may fail in Sheets if rate is passed as text (Excel silently coerces). Always ensure numeric inputs are formatted as Number, not Text.

**Output**: A TVM reference workbook and calculator containing: (a) individual calculators for PV, FV, PMT, NPER, RATE, NPV, IRR, XNPV, XIRR, (b) worked examples for each function with real financial scenarios, (c) sign convention reference table, (d) common pitfalls documentation, (e) Python equivalents for all functions using numpy-financial.

---

## S3 Cortex Logging

```json
{
  "text": "F-080 Excel Financial Functions Mastery (TVM) — calculator built, {function_count} functions covered",
  "collection": "business_evaluation",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-080",
    "domain": "finance",
    "level": "tools",
    "complexity": "BASIC",
    "functions_covered": ["PV", "FV", "PMT", "NPER", "RATE", "NPV", "IRR", "XNPV", "XIRR"],
    "implementation": "{excel|sheets|python}",
    "tags": ["business-evaluation", "excel-mastery", "tvm", "financial-functions", "present-value", "future-value", "npv", "irr"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Path — Tools Level (foundational — should be executed early in the Tools level sequence)

### WHEN
When performing any time value of money calculation or building financial function-based models

### HOW (violation detection)
- TVM calculation performed without sign convention verification -> violation (most common source of errors)
- NPV calculated without separating period-0 cash flow from the NPV function -> violation (period-0 trap)
- Annual rate used with monthly periods without dividing by 12 -> violation (rate/period mismatch)
- IRR returned without checking for multiple IRR possibility on non-conventional cash flows -> violation (may return wrong root)
- Calculator workbook missing data validation on inputs -> violation (garbage-in risk)
- Runner: manual review of calculator output against known reference values, automated check of sign convention table presence

### CONNECT
- **Upstream**: None — foundational procedure. Basic arithmetic and understanding of interest/compounding assumed.
- **Downstream**: F-068 (Loan Amortization — PMT/IPMT/PPMT practical application), F-029/F-030 (NPV/IRR for capital project evaluation), F-039 (DCF Valuation — NPV of free cash flows), F-065 (3-Statement Model — debt and investment calculations).

### VERIFY
- [ ] Procedura a fost executată complet (toate steps)?
- [ ] Output spec deliverables verificate (TVM calculator workbook with all 9 functions)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] Sign convention reference table present and correct?
- [ ] NPV period-0 trap documented and handled in calculator?

**VK**: `[F-080] TVM Functions Mastery | {function_count} functions | {excel|sheets|python} | Tools complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-068 | Feeds into | Loan Amortization is a direct practical application of PMT/IPMT/PPMT |
| F-029 | Feeds into | NPV for capital budgeting decisions uses the NPV function from this procedure |
| F-030 | Feeds into | IRR for project evaluation uses the IRR/XIRR functions from this procedure |
| F-039 | Feeds into | DCF Valuation applies NPV to projected free cash flows |
| F-065 | Feeds into | 3-Statement Model uses PMT for debt schedules and NPV/IRR for valuation checks |
| F-070 | Related | Model architecture conventions apply when building the TVM calculator workbook |

---
