---
type: procedure
created: 2026-03-17
status: active
slug: excel-sheets-mastery-batch2
tags: [procedure, nexus]
---

# FORGE Procedures — Excel & Google Sheets Mastery (Batch 2: F-081 to F-084)
# Procedure Count: 4
# Generated: 2026-03-03
# Template: FORGE Standard (Training Variant)
# Domain: Finance — Tools (Excel & Google Sheets)

**Note on §5 (Dependencies)**: All procedures in this file include a §5 Dependencies section beyond the FORGE Standard requirement (§1-§4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**Note on Google Sheets Implementation**: Each procedure includes a `### Google Sheets Implementation` subsection within §2 that provides the Google Sheets equivalent for every Excel-specific operation, including native Sheets formulas, Apps Script alternatives for VBA macros, and GOOGLEFINANCE functions where applicable.

---

## Table of Contents

### Tools Level — Excel & Sheets Mastery
| # | ID | Name | Complexity | Excel | Sheets |
|---|-----|------|-----------|-------|--------|
| 1 | F-081 | VLOOKUP/XLOOKUP/INDEX-MATCH for Financial Data | BASIC | Full | Full |
| 2 | F-082 | Pivot Table Analysis for Financial Reporting | BASIC | Full | Full (UI differences) |
| 3 | F-083 | Professional Dashboard Construction | INTERMEDIATE | Full | Partial (no Form Controls) |
| 4 | F-084 | Portfolio Dashboard with Live Market Data | INTERMEDIATE | Full | Full (GOOGLEFINANCE) |

---
---

# F-081: VLOOKUP/XLOOKUP/INDEX-MATCH for Financial Data

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: BASIC
**Rule**: TRAIN-F-081

---

## S1 Problem

Lookup functions are the connective tissue of every financial model. Without reliable data retrieval from reference tables, an analyst cannot pull metrics from financial databases, link chart-of-accounts codes to names, or cross-reference multi-year company data. Skipping this procedure means every downstream model (F-070 cross-sheet references, F-066 COA mapping, F-084 portfolio lookups) relies on fragile, manually-linked data that breaks when columns shift or new rows are added.

**Situations covered**:
- Retrieving a specific financial metric (revenue, EBITDA, P/E ratio) from a large dataset by ticker or account code
- Building left-lookups and two-dimensional lookups that VLOOKUP cannot handle
- Cross-referencing company data across multiple sheets or years in a financial model
- Error-handling for missing tickers or accounts that would otherwise break downstream calculations
- Building a reusable lookup reference workbook for practice and team onboarding
- **NOT suitable for**: Fuzzy matching on company names (use text similarity algorithms instead), real-time streaming data retrieval (use API connections), or lookups across separate workbook files without proper external references established

---

## S2 Procedure

1. **VLOOKUP for basic financial data retrieval.** Syntax: `=VLOOKUP(lookup_value, table_array, col_index_num, [range_lookup])`. Always use `FALSE` (exact match) for financial data — approximate match is almost never appropriate for account codes, ticker symbols, or dates. Example: retrieve a company's revenue from a financial database table: `=VLOOKUP("AAPL", FinancialData!A:F, 3, FALSE)` returns column 3 (Revenue) for ticker AAPL. Document three limitations: (a) lookup value must be in the leftmost column, (b) col_index_num breaks if columns are inserted or deleted, (c) returns only the first match. Python equivalent: `df.loc[df['Ticker'] == 'AAPL', 'Revenue'].iloc[0]`.

2. **XLOOKUP as the modern replacement.** Syntax: `=XLOOKUP(lookup_value, lookup_array, return_array, [if_not_found], [match_mode], [search_mode])`. Advantages over VLOOKUP: lookup column need not be leftmost, native error handling via `if_not_found`, supports left-lookups and entire-row returns. Example: `=XLOOKUP("AAPL", FinancialData!C:C, FinancialData!A:A, "Not found", 0)` — looks up AAPL in column C and returns from column A (left-lookup impossible with VLOOKUP). Match modes: 0 = exact, -1 = exact or next smaller, 1 = exact or next larger, 2 = wildcard. Financial use case for nearest-date matching: `=XLOOKUP(TODAY(), DateColumn, PriceColumn, , -1)` returns the most recent available price on or before today.

3. **INDEX-MATCH for robust two-way lookups.** Syntax: `=INDEX(return_range, MATCH(lookup_value, lookup_range, 0))`. This is the professional standard in financial modeling (cross-referenced in F-070). Example: `=INDEX(Financials!$D$5:$D$100, MATCH("EBITDA", Financials!$A$5:$A$100, 0))`. Key advantage: column insertions and deletions do not break the formula because it references specific named ranges, not column offset numbers. For two-dimensional lookup (row and column intersection): `=INDEX(DataRange, MATCH(RowLookup, RowHeaders, 0), MATCH(ColLookup, ColHeaders, 0))`. Example: retrieve the 2024 EBITDA from a company-year matrix: `=INDEX($C$5:$G$20, MATCH("EBITDA", $B$5:$B$20, 0), MATCH(2024, $C$4:$G$4, 0))`.

4. **INDEX-MATCH-MATCH for cross-tab financial data.** Full two-dimensional lookup: `=INDEX(DataTable, MATCH(RowCriteria, RowLabels, 0), MATCH(ColCriteria, ColLabels, 0))`. Financial use case: company comparison matrix with companies as columns, metrics as rows. `=INDEX(CompMatrix!$B$2:$F$20, MATCH("P/E Ratio", CompMatrix!$A$2:$A$20, 0), MATCH("MSFT", CompMatrix!$B$1:$F$1, 0))` retrieves the P/E ratio for Microsoft from a comparable companies table. Python equivalent: `comp_df.loc['P/E Ratio', 'MSFT']`.

5. **Error handling with IFERROR and IFNA.** Wrap all lookup formulas to handle missing data gracefully: `=IFERROR(INDEX(..., MATCH(...)), "N/A")` or `=IFERROR(VLOOKUP(...), 0)`. For XLOOKUP, use the built-in `if_not_found` parameter: `=XLOOKUP(value, range, return, "Not found")`. Financial context: when pulling ratios for a company that does not exist in the dataset, display "N/A" rather than #N/A error (which breaks downstream SUM/AVERAGE calculations). For numeric fallback: `=IFERROR(VLOOKUP(...), 0)` — but document that 0 is a placeholder, not actual data, to avoid misinterpretation.

6. **Practical financial data integration patterns.** Build three common lookup scenarios: (a) **Chart of Accounts lookup**: given an account code, retrieve account name, category, and P&L section using INDEX-MATCH against the COA mapping table (as used in F-066). (b) **Market data lookup**: given a ticker, retrieve current price, market cap, sector from a market data sheet: `=XLOOKUP(A2, MarketData!$A:$A, MarketData!$B:$F)` returns an entire row of data. (c) **Historical financial statement lookup**: given a company, year, and line item, retrieve the value from a 3D database using concatenated helper columns: `=INDEX(FinDB, MATCH(Company&Year&LineItem, CompanyCol&YearCol&ItemCol, 0))`, or use SUMPRODUCT: `=SUMPRODUCT((CompanyCol=Company)*(YearCol=Year)*(ItemCol=LineItem)*ValueCol)`.

7. **Build a lookup reference workbook.** Create a workbook with: (a) Sample financial database (50+ companies, 5 years, 15 metrics each). (b) VLOOKUP examples sheet — 5 worked examples with explanatory notes. (c) XLOOKUP examples sheet — 5 worked examples showcasing advantages over VLOOKUP. (d) INDEX-MATCH examples sheet — 5 worked examples including 2D lookups. (e) Performance comparison note: INDEX-MATCH is faster on large datasets (100K+ rows) than VLOOKUP because MATCH searches a single column vs. VLOOKUP loading the entire table_array. (f) Decision tree: When to use which function — VLOOKUP for quick ad-hoc, XLOOKUP for modern Excel/Sheets, INDEX-MATCH for financial models and shared workbooks.

### Google Sheets Implementation

All three lookup families work in Google Sheets with minimal differences:

- **VLOOKUP**: Identical syntax and behavior. `=VLOOKUP("AAPL", FinancialData!A:F, 3, FALSE)` works unchanged. Sheets also supports `is_sorted` parameter (equivalent to `range_lookup`).
- **XLOOKUP**: Fully supported in Google Sheets (added 2022). Syntax is identical: `=XLOOKUP("AAPL", C:C, A:A, "Not found", 0)`. All match modes (0, -1, 1, 2) and search modes work the same.
- **INDEX-MATCH**: Identical syntax. `=INDEX(D5:D100, MATCH("EBITDA", A5:A100, 0))` works unchanged. Two-dimensional INDEX-MATCH-MATCH also identical.
- **IFERROR / IFNA**: Identical in Sheets.
- **SUMPRODUCT**: Identical in Sheets for multi-criteria lookups.
- **Sheets-specific alternatives**: Google Sheets also offers `=FILTER(return_range, criteria_range=value)` which can replace simple lookups and return multiple rows: `=FILTER(FinancialData!B:F, FinancialData!A:A="AAPL")` returns all columns for AAPL. QUERY function can also serve as a powerful lookup: `=QUERY(FinancialData!A:F, "SELECT C WHERE A='AAPL'")`.
- **Performance note**: For very large datasets (100K+ rows), Sheets can be slower than Excel. Consider using Apps Script for batch lookups: `SpreadsheetApp.getActiveSheet().getRange().getValues()` with JavaScript array methods.

**Output**: A lookup function reference workbook containing: (a) worked examples of VLOOKUP, XLOOKUP, INDEX-MATCH, and INDEX-MATCH-MATCH applied to financial data, (b) error-handling patterns, (c) three practical financial integration scenarios (COA lookup, market data, historical financials), (d) function selection decision tree, (e) sample financial database for practice. Deliverable in both Excel (.xlsx) and Google Sheets format.

---

## S3 Cortex Logging

```json
{
  "text": "F-081 VLOOKUP/XLOOKUP/INDEX-MATCH for Financial Data — executed for {context}",
  "collection": "procedures",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-081",
    "rule_id": "TRAIN-F-081",
    "domain": "finance",
    "level": "tools",
    "complexity": "BASIC",
    "tags": ["excel", "google-sheets", "lookup-functions", "vlookup", "xlookup", "index-match", "financial-modeling"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Pipeline — Tools Level

### WHEN
When building any financial model that requires data retrieval from reference tables, or when onboarding to Excel/Sheets fundamentals

### HOW (violation detection)
- Lookup formula uses `TRUE` or omits `range_lookup` for financial data (approximate match on account codes/tickers = violation)
- VLOOKUP used in a shared financial model without documented reason (INDEX-MATCH is the professional standard per Step 3)
- Lookup formula lacks IFERROR/IFNA wrapper in a model where missing data would break downstream calculations
- Lookup reference workbook (Step 7) missing any of the four required sections (VLOOKUP, XLOOKUP, INDEX-MATCH examples, decision tree)

### CONNECT
- **Upstream**: None — foundational tools procedure (no prerequisites)
- **Downstream**: F-066 (COA lookup patterns), F-070 (cross-sheet INDEX-MATCH), F-082 (pivot source preparation), F-083 (KPI card formulas), F-084 (portfolio data retrieval), F-002/F-003/F-004 (ratio analysis lookups)

### VERIFY
- [ ] Procedura a fost executată complet (toate steps 1-7)?
- [ ] Output spec deliverables verificate (reference workbook with VLOOKUP/XLOOKUP/INDEX-MATCH examples, error handling, 3 financial scenarios, decision tree)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] All three lookup families (VLOOKUP, XLOOKUP, INDEX-MATCH) demonstrated with financial data examples?
- [ ] Google Sheets implementation tested alongside Excel version?

**VK**: `[F-081] VLOOKUP/XLOOKUP/INDEX-MATCH | {context} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-066 | Feeds into | COA lookup pattern from Step 6a is used directly in the FMCG SAP report |
| F-070 | Feeds into | INDEX-MATCH is the standard cross-sheet reference method in 3-Statement models |
| F-082 | Feeds into | Clean lookup-prepared data is prerequisite for meaningful pivot analysis |
| F-083 | Feeds into | KPI cards in Step 2 of F-083 use INDEX-MATCH to pull dynamic values |
| F-084 | Feeds into | Portfolio data retrieval in F-084 relies on lookup patterns for ticker-based access |
| F-002/F-003/F-004 | Feeds into | Ratio analysis procedures use lookups to pull specific line items from financial statements |

---
---

# F-082: Pivot Table Analysis for Financial Reporting

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: BASIC
**Rule**: TRAIN-F-082

---

## S1 Problem

Financial data in raw transaction format (ledgers, sales records, expense logs) is unusable for executive reporting without aggregation and dimensional analysis. Without pivot table skills, an analyst manually creates subtotals — a slow, error-prone process that cannot handle ad-hoc questions like "What is revenue by region and quarter?" Skipping this procedure means all multi-dimensional financial summaries (P&L by department, revenue by product-region, expense concentration analysis) require manual formula construction, making reporting cycles longer and error rates higher.

**Situations covered**:
- Summarizing transaction-level data into monthly, quarterly, or annual financial reports
- Building revenue-by-product, expense-by-department, or P&L-by-region analyses
- Identifying customer or product concentration risk via Pareto (80/20) analysis
- Creating interactive financial reports with slicers and date grouping
- Producing multi-pivot dashboard sheets for management review
- **NOT suitable for**: Real-time streaming data analysis (use database queries), datasets exceeding 1M rows in Excel (use Power Pivot or database tools), or statistical modeling requiring regression or hypothesis testing (use Python/R)

---

## S2 Procedure

1. **Prepare the source data for pivot analysis.** Ensure the dataset meets pivot table requirements: (a) tabular format — one row per transaction or observation, no merged cells, no blank rows within data. (b) Consistent headers in row 1 — no duplicates, no special characters. (c) No subtotal rows embedded in the data (pivot tables calculate their own). (d) Consistent data types per column (do not mix text and numbers in the same column). Common financial datasets: transaction ledgers, sales records, expense reports, trade logs. In Python: `df = pd.read_csv('transactions.csv'); df.columns = df.columns.str.strip(); df = df.dropna(subset=['Amount'])`.

2. **Design the pivot table field layout for financial reporting.** Map business questions to pivot configurations: (a) **Revenue by Product and Region**: Rows = Product_Group, Columns = Region, Values = SUM of Revenue. (b) **Monthly P&L Summary**: Rows = Account_Category (Revenue, COGS, OpEx), Columns = Month, Values = SUM of Amount. (c) **Expense Analysis by Department**: Rows = Department, Columns = Expense_Type, Values = SUM of Amount, Filter = Fiscal_Year. (d) **Customer Concentration**: Rows = Customer_Name, Values = SUM of Revenue, sort descending, add running percentage for Pareto (80/20) analysis. In pandas: `pd.pivot_table(df, values='Revenue', index='Product_Group', columns='Region', aggfunc='sum', fill_value=0)`.

3. **Implement calculated fields and custom aggregations.** Excel: PivotTable Analyze > Fields, Items & Sets > Calculated Field. Define formula using existing pivot fields. Examples: (a) Gross Margin % = (Revenue - COGS) / Revenue. (b) Variance = Actual - Budget. (c) Average Transaction Size = Revenue / Transaction_Count. In pandas: add calculated columns before pivot: `df['Gross_Margin'] = (df['Revenue'] - df['COGS']) / df['Revenue']`, then include in pivot_table values.

4. **Apply date grouping for time-series financial analysis.** Excel: right-click any date field in the pivot > Group > select Months, Quarters, Years. This transforms daily transaction data into periodic summaries without modifying the source. Grouping configurations: (a) Monthly trend: group by Year and Month. (b) Quarterly reporting: group by Year and Quarter. (c) Year-over-year comparison: Year in Columns, Month in Rows. In pandas: `df['Quarter'] = df['Date'].dt.to_period('Q'); pd.pivot_table(df, values='Amount', index='Account', columns='Quarter', aggfunc='sum')`.

5. **Add slicers and filters for interactive analysis.** Excel: PivotTable Analyze > Insert Slicer. Add slicers for: Fiscal_Year, Region, Product_Group, Department. Connect multiple slicers to the same pivot table for drill-down. If multiple pivot tables share the same data source, connect slicers to all via Report Connections. Timeline slicer: insert for date fields to enable drag-to-filter by period. In Python: simulate interactivity by parameterizing the pivot call: `def financial_pivot(df, year=None, region=None): filtered = df.copy(); if year: filtered = filtered[filtered['Year']==year]; return pd.pivot_table(filtered, ...)`.

6. **Create pivot charts for visual financial reporting.** From the pivot table: Insert > PivotChart. Chart type recommendations: (a) Revenue trend: line chart (months on x-axis). (b) Revenue by region: stacked bar chart. (c) Expense breakdown: pie or donut chart. (d) Actual vs Budget: clustered column chart. (e) Pareto / customer concentration: bar + cumulative line combo chart. Formatting: add data labels for key values, use company color palette, title format = "Description — Period" (e.g., "Revenue by Region — Q3 2025"). In Python: `pivot.plot(kind='bar', stacked=True, figsize=(12,6), title='Revenue by Region')`.

7. **Implement refresh and data source management.** Ensure pivot tables update when source data changes: (a) Manual refresh: right-click > Refresh. (b) Auto-refresh on file open: PivotTable Options > Data > "Refresh data when opening the file". (c) Change data source: PivotTable Analyze > Change Data Source when rows are added. Best practice: define source data as an Excel Table (Ctrl+T) so the pivot automatically includes new rows. In Python: simply re-run the pivot_table function on the updated DataFrame.

8. **Build a multi-pivot financial reporting dashboard.** Combine 3-4 pivot tables on a single Dashboard sheet: (a) Top-left: Monthly Revenue Trend (line pivot chart). (b) Top-right: Expense Category Breakdown (donut pivot chart). (c) Bottom-left: Regional P&L Summary (pivot table with conditional formatting). (d) Bottom-right: Top 10 Customers/Products (bar chart). Connect all to shared slicers for Year and Region. In openpyxl: create charts programmatically using `openpyxl.chart` classes, position with `ws.add_chart(chart, "A1")`. This feeds into F-083 (Professional Dashboard Construction) for the non-pivot dashboard elements.

### Google Sheets Implementation

Google Sheets pivot tables work fundamentally differently from Excel — they use a side-panel editor rather than drag-and-drop field areas. Key differences:

- **Creating a pivot table**: Select data range > Insert > Pivot table > choose "New sheet" or "Existing sheet". Sheets opens the **Pivot table editor** panel on the right side (not the field list dialog Excel uses).
- **Field layout**: In the editor panel, click "Add" next to Rows, Columns, Values, or Filters to assign fields. Drag-and-drop reordering is supported within the panel. Each value field has a dropdown for SUM, COUNT, AVERAGE, etc.
- **Calculated fields**: Sheets supports calculated fields via the pivot table editor panel > Values > Add > Calculated Field. Syntax uses field names directly: `='Revenue' - 'COGS'`. However, Sheets calculated fields are more limited than Excel — complex formulas may require a helper column in the source data instead.
- **Date grouping**: Right-click a date row/column in the pivot > Create pivot date group > select Year, Quarter, Month, Day. The UI flow is similar to Excel but the menu path differs.
- **Slicers**: Insert > Slicer. Works similarly to Excel slicers — select the data range and field, then the slicer appears as a floating control. Multiple slicers can filter the same pivot. **No Timeline slicer** — use a regular slicer on a date-grouped field instead.
- **Pivot charts**: Select the pivot table > Insert > Chart. Sheets auto-detects the pivot data. Chart editor panel opens on the right for customization. Chart types available: line, bar, column, pie, combo, etc. Charts update automatically when the pivot refreshes.
- **Refresh**: Sheets pivots refresh automatically when source data changes (no manual refresh needed). If the source is in a different sheet, updates propagate immediately.
- **QUERY function alternative**: For simple pivots, Sheets offers `=QUERY(DataRange, "SELECT B, SUM(D) GROUP BY B LABEL SUM(D) 'Total Revenue'")` which produces pivot-like output directly in cells without a formal pivot table. This is a Sheets-only capability with no direct Excel equivalent.
- **Apps Script for automation**: For programmatic pivot creation, use `SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Data').getRange('A1:F1000').createPivotTable(destinationRange)` with `addPivotValue()`, `addRowGroup()`, `addColumnGroup()` methods.

**Output**: A pivot table analysis workbook containing: (a) clean source dataset formatted as an Excel Table / Sheets named range, (b) 4+ pivot tables answering key financial questions (revenue, expenses, P&L, concentration), (c) calculated fields for margins and variances, (d) date grouping at monthly/quarterly/annual levels, (e) slicers for interactive filtering, (f) pivot charts for each analysis, (g) combined dashboard sheet with connected slicers. Deliverable in both Excel and Google Sheets format.

---

## S3 Cortex Logging

```json
{
  "text": "F-082 Pivot Table Analysis for Financial Reporting — executed for {context}",
  "collection": "procedures",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-082",
    "rule_id": "TRAIN-F-082",
    "domain": "finance",
    "level": "tools",
    "complexity": "BASIC",
    "tags": ["excel", "google-sheets", "pivot-tables", "financial-reporting", "data-analysis", "slicers"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Pipeline — Tools Level

### WHEN
When building any multi-dimensional financial summary, or when raw transaction data needs to be aggregated for reporting

### HOW (violation detection)
- Pivot table built on source data containing merged cells, blank rows, or embedded subtotals (Step 1 data prep skipped = violation)
- Manual subtotals or SUMIFS used for multi-dimensional aggregation where a pivot table would be more appropriate and maintainable
- Pivot table created without slicers or filters when the analysis involves multiple dimensions (Year, Region, Department)
- Calculated field uses hardcoded values instead of referencing existing pivot fields

### CONNECT
- **Upstream**: Clean tabular data (F-081 lookup skills useful for data preparation, but not strictly required); F-002/F-003/F-004 ratio outputs as potential source data
- **Downstream**: F-083 (Dashboard Construction — pivot analyses become dashboard components), F-066 (FMCG Report — same source data, ad-hoc pivot analysis), F-084 (portfolio allocation aggregation by sector/asset class)

### VERIFY
- [ ] Procedura a fost executată complet (toate steps 1-8)?
- [ ] Output spec deliverables verificate (pivot workbook with 4+ pivot tables, calculated fields, date grouping, slicers, charts, dashboard sheet)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] Source data formatted as Excel Table / Sheets named range (auto-expanding)?
- [ ] Both Excel and Google Sheets versions delivered with UI differences documented?

**VK**: `[F-082] Pivot Table Analysis | {context} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-081 | Receives from | Lookup skills help prepare and validate source data before pivoting |
| F-083 | Feeds into | Pivot table outputs become chart components and data tables in the professional dashboard |
| F-066 | Parallel to | SAP transaction data from F-066 can be pivoted for ad-hoc analysis beyond the structured P&L report |
| F-002/F-003/F-004 | Receives from | Ratio analysis outputs can be pivoted for multi-company or multi-period comparison |
| F-084 | Feeds into | Portfolio allocation analysis uses pivot-style aggregation by sector and asset class |

---
---

# F-083: Professional Dashboard Construction

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-083

---

## S1 Problem

Raw financial data and ratio outputs are not decision-ready until presented in a clear, interactive visual format. Without dashboard construction skills, reports are static tables that require manual interpretation, slowing decision-making and increasing the risk of overlooking critical trends or outliers. Skipping this procedure means financial analyses from F-002/F-003/F-004 (ratios), F-066 (FMCG P&L), and F-082 (pivots) remain as raw numbers rather than actionable visual intelligence.

**Situations covered**:
- Building executive-level financial dashboards with KPI cards, charts, and detail tables
- Creating interactive controls (dropdowns, slicers) that filter all dashboard elements simultaneously
- Implementing RAG (Red/Amber/Green) conditional formatting for variance analysis
- Producing print-ready one-page financial summaries
- Applying consistent professional formatting standards across financial reports
- **NOT suitable for**: Real-time streaming dashboards requiring sub-second refresh (use BI tools like Power BI or Tableau), dashboards with more than 20 interactive controls (use dedicated BI platform), or mobile-first dashboard design (Excel/Sheets dashboards are desktop-oriented)

---

## S2 Procedure

1. **Define the dashboard layout grid.** Establish a fixed layout structure on a dedicated "Dashboard" sheet: (a) **Top row (rows 1-6)**: KPI cards — 4-6 key metrics displayed prominently (Revenue, Net Income, Gross Margin %, YoY Growth, Cash Position, Debt/Equity). (b) **Middle section (rows 7-22)**: 2-3 charts side by side (revenue trend line, expense composition bar, margin evolution). (c) **Bottom section (rows 23-35)**: detail table with conditional formatting (top 10 products/customers, or monthly summary). (d) **Right sidebar (column K+)**: filters and controls — dropdown selectors for year, region, department. Set column widths: A = 2 (spacer), B-I = 12-15 each, J = 2 (spacer). Set row heights uniformly. Hide gridlines: View > uncheck Gridlines. In openpyxl: `ws.sheet_view.showGridLines = False; ws.column_dimensions['A'].width = 2`.

2. **Build KPI cards with dynamic values.** Each KPI card occupies a merged cell range (e.g., B2:C5) with three layers: (a) Metric label (small font, gray, top of card). (b) Current value (large font, bold, center). (c) YoY change indicator (small font, green up-arrow or red down-arrow). Formulas: Current Revenue = `=INDEX(DataSheet!RevenueRow, MATCH(SelectedYear, DataSheet!YearHeaders, 0))`. YoY Change = `=(CurrentValue - PriorValue) / ABS(PriorValue)`. Conditional formatting: if YoY > 0, green font + upward triangle `=CHAR(9650)`; if YoY < 0, red font + downward triangle `=CHAR(9660)`. In openpyxl: merge cells, apply `Alignment(horizontal='center', vertical='center')`, conditional `Font(color='00B050')` for positive.

3. **Create dynamic charts with OFFSET-based ranges (Excel) or named ranges.** Define named ranges that auto-adjust to data length: `RevenueChartData = OFFSET(Data!$C$5, 0, 0, COUNTA(Data!$B$5:$B$100), 1)`. `ChartLabels = OFFSET(Data!$B$5, 0, 0, COUNTA(Data!$B$5:$B$100), 1)`. Build charts referencing these names: (a) Revenue trend: Line chart with ChartLabels as X-axis, RevenueChartData as series. (b) Expense waterfall: Stacked bar showing COGS + SG&A + R&D + Other. (c) Margin evolution: Combo chart — revenue as bars, margin % as line on secondary axis. Chart formatting: remove chart border, transparent plot area, consistent color palette (blue=#2F5496, green=#548235, red=#C00000, gray=#808080). In openpyxl: `chart = LineChart(); chart.title = "Revenue Trend"; chart.style = 10; chart.y_axis.title = "Revenue ($M)"`.

4. **Add conditional formatting for RAG (Red/Amber/Green) status.** Apply to variance columns and KPI targets: (a) **Icon sets**: 3-traffic-light icons on margin columns — green if margin > target, yellow if within 2% of target, red if below by >2%. Excel: Home > Conditional Formatting > Icon Sets > 3 Traffic Lights. (b) **Data bars**: apply to revenue columns to show relative magnitude. (c) **Color scales**: apply to heatmap-style tables (monthly revenue by product — dark green = high, light = low). In openpyxl: `from openpyxl.formatting.rule import IconSet, FormatObject; rule = IconSet(iconSet='3TrafficLights1', cfvo=[FormatObject(type='num', val=0), FormatObject(type='num', val=0.05), FormatObject(type='num', val=0.10)])`.

5. **Implement interactive controls with Data Validation dropdowns.** Create selector cells: (a) Year selector (B1): Data Validation > List > "2022,2023,2024,2025". (b) Region selector (D1): Data Validation > List > "All,EMEA,APAC,Americas". (c) Department selector (F1): Data Validation > List from a named range. Link all KPI formulas and chart data ranges to these selectors using INDEX-MATCH with the selector cell as the lookup criterion. Example: `=SUMIFS(Data!Revenue, Data!Year, $B$1, Data!Region, IF($D$1="All", Data!Region, $D$1))`. This makes the entire dashboard respond to dropdown selections. In openpyxl: `dv = DataValidation(type="list", formula1='"2022,2023,2024,2025"'); ws.add_data_validation(dv); dv.add('B1')`.

6. **Add sparklines for compact trend visualization.** Insert sparklines in KPI cards to show 12-month trends in a single cell: Excel: Insert > Sparklines > Line > Data Range = last 12 months, Location = cell within the KPI card. Set high point marker = green, low point marker = red. Sparklines provide trend context without consuming dashboard space. In openpyxl: sparklines are not natively supported — alternative: generate a small inline matplotlib chart, save as image, insert using `openpyxl.drawing.image.Image`.

7. **Build the detail table with formatting.** Bottom section contains a sortable summary table: (a) Columns: Product/Customer, Revenue, COGS, Gross Profit, Gross Margin %, Budget Variance, Status. (b) Header row: bold, dark background (#2F5496), white font. (c) Alternating row colors (white / #D9E2F3) for readability. (d) Status column formula: `=IF(Variance>0.05, "Above Target", IF(Variance>-0.05, "On Track", "Below Target"))` with conditional formatting (green/yellow/red fill). (e) Sort by Revenue descending, show top 10. In openpyxl: apply `PatternFill`, `Border`, and `Font` objects row-by-row.

8. **Finalize and protect the dashboard.** (a) Lock all formula cells and unlock only dropdown selector cells. Excel: Format Cells > Protection > Locked/Unlocked, then Review > Protect Sheet. (b) Set print area to the dashboard for one-page PDF output. (c) Add a last-updated timestamp: `="Last Updated: "&TEXT(NOW(),"YYYY-MM-DD HH:MM")`. (d) Add data source note in small font at the bottom. (e) Test all dropdown combinations to ensure no #REF or #N/A errors. In openpyxl: `ws.protection.sheet = True; ws.protection.password = 'view_only'`.

### Google Sheets Implementation

Google Sheets dashboards differ from Excel primarily in interactive controls and charting. Key differences and workarounds:

- **Layout and gridlines**: Identical approach. View > uncheck Gridlines. Column/row sizing via right-click > Resize. Merge cells via Format > Merge cells.

- **KPI cards**: Same approach — merged cells with INDEX-MATCH formulas. CHAR(9650) and CHAR(9660) work identically for triangle indicators. Conditional formatting: Format > Conditional formatting > custom formula rule.

- **Charts**: Insert > Chart opens the Chart editor panel on the right. Chart types are similar (line, bar, column, combo, pie) but the configuration UI is entirely different from Excel. Key differences:
  - No Form Controls (buttons, scroll bars) exist in Sheets
  - Combo charts: use "Customize > Series" to change individual series to a different chart type
  - Color theming: set in Customize > Chart style or per-series in Customize > Series

- **OFFSET-based dynamic ranges**: `OFFSET` works in Sheets, but a more native approach is to use `FILTER` or `QUERY` functions for dynamic ranges. Example: instead of OFFSET for chart data, use a helper range: `=FILTER(Data!C:C, Data!B:B<>"")`. Charts referencing these auto-resize.

- **Interactive controls — NO Form Controls, use Data Validation dropdowns + FILTER/QUERY**:
  - Year selector: Data > Data validation > Dropdown > list of values. Place in cell B1.
  - Instead of SUMIFS linked to Form Controls, use the same SUMIFS formula linked to the Data Validation cell: `=SUMIFS(Data!C:C, Data!A:A, B1)`.
  - For more complex filtering, use QUERY: `=QUERY(Data!A:F, "SELECT B, SUM(D) WHERE A='"&B1&"' GROUP BY B")`.
  - For cascading filters (region changes based on year), use dependent data validation: second dropdown list = `=FILTER(Regions, Years=B1)`.

- **Slicers**: Sheets supports slicers (Data > Add a slicer). They float over the sheet and filter the underlying data range. However, they filter the raw data range, not chart data directly — charts referencing that range update automatically.

- **Sparklines**: Sheets has NATIVE sparkline support as a formula: `=SPARKLINE(B2:M2, {"charttype","line";"color","blue";"linewidth",2})`. This is simpler than Excel sparklines and more powerful (supports line, bar, column, winloss types, all via formula parameters). Additional options: `{"highcolor","green";"lowcolor","red";"firstcolor","orange"}`.

- **Conditional formatting**: Format > Conditional formatting. Supports color scales, single color rules, and custom formula rules. **No icon sets** — workaround: use IF formulas with Unicode symbols in a helper column: `=IF(A2>target, CHAR(9679), IF(A2>target*0.98, CHAR(9675), CHAR(9679)))` with green/yellow/red font color via conditional formatting.

- **Sheet protection**: Data > Protect sheets and ranges. Can protect entire sheet except specific cells (dropdown cells). More granular than Excel — can set different permissions per range per user.

- **Apps Script for advanced interactivity**: For button-like controls, insert a Drawing (Insert > Drawing), then right-click > Assign script. The script can be an Apps Script function: `function updateDashboard() { var year = SpreadsheetApp.getActiveSheet().getRange('B1').getValue(); /* refresh logic */ }`.

**Output**: A professional financial dashboard workbook containing: (a) KPI cards with dynamic values and trend indicators (sparklines in Sheets native, images in Excel/openpyxl), (b) 3+ interactive charts (trend line, composition bar, margin combo), (c) RAG conditional formatting with icons (Excel) or Unicode symbols (Sheets), (d) dropdown controls for year/region/department filtering, (e) formatted detail table with top-N products/customers, (f) sheet protection and print-ready layout. Deliverable in both Excel and Google Sheets format.

---

## S3 Cortex Logging

```json
{
  "text": "F-083 Professional Dashboard Construction — executed for {context}",
  "collection": "procedures",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-083",
    "rule_id": "TRAIN-F-083",
    "domain": "finance",
    "level": "tools",
    "complexity": "INTERMEDIATE",
    "tags": ["excel", "google-sheets", "dashboard", "data-visualization", "kpi", "conditional-formatting", "interactive"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Pipeline — Tools Level

### WHEN
When building any visual financial report, executive summary, or management dashboard

### HOW (violation detection)
- Dashboard built without a defined layout grid (Step 1 skipped — ad-hoc element placement = violation)
- KPI cards display static values instead of dynamic INDEX-MATCH formulas linked to data source
- Interactive controls (dropdowns/slicers) not connected to all dashboard elements (charts + tables must all respond to filter changes)
- Dashboard shipped without sheet protection on formula cells (end-user can accidentally overwrite formulas)

### CONNECT
- **Upstream**: F-081 (lookup functions for KPI formulas), F-082 (pivot tables for data summarization), F-002/F-003/F-004 (ratio outputs as source data), F-066 (FMCG P&L data for revenue/expense charts)
- **Downstream**: F-084 (Portfolio Dashboard — applies the same layout principles to investment data), F-014 (Comprehensive Ratio Dashboard — shares layout patterns)

### VERIFY
- [ ] Procedura a fost executată complet (toate steps 1-8)?
- [ ] Output spec deliverables verificate (KPI cards, 3+ charts, RAG formatting, dropdowns, detail table, protection)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] All dropdown combinations tested with no #REF or #N/A errors?
- [ ] Dashboard fits on one printable page (print area set)?

**VK**: `[F-083] Professional Dashboard Construction | {context} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-081 | Receives from | INDEX-MATCH used in KPI card formulas (Step 2) and linked dropdowns (Step 5) |
| F-082 | Receives from | Pivot table outputs become chart data sources and detail tables in the dashboard |
| F-002/F-003/F-004 | Receives from | Ratio analysis outputs (profitability, liquidity, activity) are the KPI values displayed |
| F-066 | Receives from | FMCG P&L data provides the source for revenue/expense charts |
| F-084 | Feeds into | Portfolio Dashboard in F-084 reuses the KPI card design, chart layout, and protection patterns |
| F-014 | Parallel to | Comprehensive Ratio Dashboard shares layout principles — F-083 is the generalized version, F-014 is ratio-specific |

---
---

# F-084: Portfolio Dashboard with Live Market Data

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-084

---

## S1 Problem

Investment portfolios require continuous monitoring of positions, returns, allocation, and performance vs. benchmarks. Without a structured dashboard that pulls live market data, an investor relies on brokerage platform views that cannot be customized, lack total-portfolio analytics (alpha, beta, Sharpe ratio), and do not integrate with personal cost-basis tracking. Skipping this procedure means portfolio analysis is fragmented across multiple tools, returns are calculated ad-hoc (often incorrectly), and concentration risk goes unmonitored.

**Situations covered**:
- Tracking a personal or model investment portfolio with live price updates
- Calculating unrealized P&L, portfolio returns, and holding-level performance
- Analyzing allocation by sector, asset class, and individual position concentration
- Benchmarking portfolio performance against S&P 500 or other indices with alpha/beta/Sharpe metrics
- Tracking dividend income and projecting annual yield
- Building an automated daily-refresh pipeline for portfolio monitoring
- **NOT suitable for**: High-frequency trading requiring millisecond latency (use dedicated trading platforms), portfolios with complex derivatives (options/futures require specialized pricing models), or institutional-grade risk management (use Bloomberg Terminal or Aladdin)

---

## S2 Procedure

1. **Define the portfolio positions table.** Create a structured input with columns: Ticker, Company_Name, Shares_Held, Purchase_Date, Cost_Basis_Per_Share, Sector, Asset_Class (Equity, Fixed Income, Alternative). Calculate Total_Cost_Basis = Shares_Held * Cost_Basis_Per_Share. Example: 10-15 diversified holdings across sectors. In Python: `portfolio = pd.DataFrame({'Ticker': ['AAPL', 'MSFT', 'JNJ', 'JPM', 'VTI'], 'Shares': [50, 30, 40, 25, 100], 'Cost_Basis': [145.00, 280.00, 160.00, 140.00, 210.00], 'Sector': ['Technology', 'Technology', 'Healthcare', 'Financials', 'Broad Market']})`.

2. **Retrieve current market data.** Using Python yfinance library: `import yfinance as yf; data = yf.download(tickers, period='1y', interval='1d')`. For each holding retrieve: Current_Price, Previous_Close, 52_Week_High, 52_Week_Low, Market_Cap, PE_Ratio, Dividend_Yield. Single ticker info: `ticker = yf.Ticker('AAPL'); info = ticker.info; current_price = info['currentPrice']`. For Excel 365: use `=STOCKHISTORY(ticker, start_date, end_date, 0, 0, 0, 1)` for historical data. **Platform note**: STOCKHISTORY is available ONLY in Excel 365 (Microsoft 365 subscription) and has inconsistent regional availability. It may not be available in all markets. Alternative: use the Power Query approach (Get Data > From Web) with a financial data URL, or use Google Sheets GOOGLEFINANCE which has broader availability. In Python batch: `for t in portfolio['Ticker']: info = yf.Ticker(t).info; portfolio.loc[portfolio['Ticker']==t, 'Current_Price'] = info.get('currentPrice', 0)`.

3. **Calculate portfolio P&L and returns.** For each holding: Current_Value = Shares * Current_Price. Unrealized_PnL = Current_Value - Total_Cost_Basis. Return_Pct = (Current_Price - Cost_Basis) / Cost_Basis. Portfolio totals: Total_Cost = sum of all Cost_Basis. Total_Current_Value = sum of all Current_Value. Total_PnL = Total_Current_Value - Total_Cost. Portfolio_Return = Total_PnL / Total_Cost. Weight = Current_Value / Total_Current_Value. Weighted_Return = sum of (Weight_i * Return_i). In pandas: `portfolio['Current_Value'] = portfolio['Shares'] * portfolio['Current_Price']; portfolio['PnL'] = portfolio['Current_Value'] - portfolio['Shares'] * portfolio['Cost_Basis']; portfolio['Return_Pct'] = portfolio['PnL'] / (portfolio['Shares'] * portfolio['Cost_Basis'])`.

4. **Build the allocation analysis.** (a) **By holding**: pie chart showing each position's weight. (b) **By sector**: aggregate weights — `sector_alloc = portfolio.groupby('Sector')['Current_Value'].sum() / portfolio['Current_Value'].sum()`. (c) **By asset class**: aggregate by Asset_Class. Check concentration risk: flag any single holding > 15% of portfolio or any sector > 30%. In matplotlib: `sector_alloc.plot.pie(autopct='%1.1f%%', title='Sector Allocation')`.

5. **Generate performance charts vs benchmark.** Download benchmark data (e.g., S&P 500): `benchmark = yf.download('^GSPC', start=earliest_purchase_date, end='today')`. Calculate portfolio daily value: for each date, sum (Shares_i * Close_Price_i). **Date alignment warning**: Different securities may have different trading calendars (e.g., US vs European markets, holidays). When combining daily prices, use a master date index and handle missing dates with forward-fill (`VLOOKUP approximate match` or Sheets `FILTER` with date tolerance). Missing a trading day for one ticker while others have data will produce incorrect portfolio values. Normalize both to 100 at start date for comparison. Chart: dual line — Portfolio (blue) vs Benchmark (gray dashed). Calculate risk-adjusted metrics: Alpha = Portfolio_Return - Benchmark_Return. Beta = covariance(portfolio_returns, benchmark_returns) / variance(benchmark_returns). Sharpe Ratio = (Portfolio_Return - Risk_Free_Rate) / Portfolio_StdDev. In Python: `portfolio_daily = sum([shares * yf.download(ticker)['Close'] for ticker, shares in positions]); normalized = portfolio_daily / portfolio_daily.iloc[0] * 100`.

6. **Track dividends and income.** For each holding retrieve dividend history: `divs = yf.Ticker(ticker).dividends`. Calculate: Annual_Dividend_Income = Shares * Annual_Dividend_Per_Share. Dividend_Yield_on_Cost = Annual_Dividend / Cost_Basis. Portfolio_Yield = Total_Annual_Dividends / Total_Cost_Basis. Create a dividend calendar: upcoming ex-dates and payment dates. In pandas: `div_history = pd.concat([yf.Ticker(t).dividends.rename(t) for t in tickers], axis=1)`. Sum by month for a dividend income timeline chart.

7. **Build the dashboard layout.** Apply F-083 dashboard principles to the investment context: (a) **KPI Cards (top)**: Total Portfolio Value, Total P&L ($ and %), Today's Change, Portfolio Yield, Sharpe Ratio. (b) **Charts (middle-left)**: Performance vs Benchmark line chart. (c) **Charts (middle-right)**: Sector allocation donut chart. (d) **Detail Table (bottom)**: holdings table with Ticker, Shares, Cost, Current Price, Value, P&L, Return%, Weight, 52W Range indicator. Sort by weight descending. Conditional formatting: green fill for positive P&L, red for negative. (e) **Controls**: dropdown for time period (1M, 3M, 6M, YTD, 1Y, All). In openpyxl: construct all elements, place charts with `ws.add_chart()`.

8. **Implement auto-refresh and data export.** Python script for daily update: (a) read portfolio positions from CSV/Excel. (b) Pull latest prices via yfinance. (c) Recalculate all metrics. (d) Generate updated Excel dashboard with openpyxl. (e) Save with date stamp: `portfolio_dashboard_2026-03-03.xlsx`. (f) Optionally export summary as JSON for external integrations: `{'total_value': X, 'pnl': Y, 'return_pct': Z, 'top_movers': [...]}`. Schedule via cron or LaunchAgent for daily market-close execution.

### Google Sheets Implementation

Google Sheets offers a major advantage for portfolio dashboards: the native `GOOGLEFINANCE` function provides real-time and historical market data directly in cells, eliminating the need for external API scripts.

- **GOOGLEFINANCE for real-time data** (replaces Yahoo Finance API for current prices):
  - Current price: `=GOOGLEFINANCE("GOOG", "price")` — returns the latest price (delayed ~15 minutes)
  - Previous close: `=GOOGLEFINANCE("GOOG", "closeyest")`
  - Market cap: `=GOOGLEFINANCE("GOOG", "marketcap")`
  - P/E ratio: `=GOOGLEFINANCE("GOOG", "pe")`
  - 52-week high: `=GOOGLEFINANCE("GOOG", "high52")`
  - 52-week low: `=GOOGLEFINANCE("GOOG", "low52")`
  - Volume: `=GOOGLEFINANCE("GOOG", "volume")`
  - Change today: `=GOOGLEFINANCE("GOOG", "changepct")` (returns percentage)
  - Dividend yield: Not directly available via GOOGLEFINANCE — use a helper lookup or Apps Script

- **GOOGLEFINANCE for historical data** (replaces STOCKHISTORY / yfinance for charts):
  - `=GOOGLEFINANCE("GOOG", "price", DATE(2025,1,1), DATE(2025,12,31), "DAILY")` returns a two-column array (Date, Close) that auto-fills downward
  - For benchmark comparison: `=GOOGLEFINANCE(".INX", "price", start, end, "DAILY")` (S&P 500 uses ".INX" or "INDEXSP:.INX")
  - Use this array output as chart data source directly

- **Portfolio P&L formulas in Sheets** (no Python needed for basic version):
  - Current Price column: `=GOOGLEFINANCE(A2, "price")` where A2 contains the ticker
  - Current Value: `=Shares * GOOGLEFINANCE(A2, "price")`
  - P&L: `=CurrentValue - CostBasis`
  - Return%: `=PnL / CostBasis`
  - Weight: `=CurrentValue / SUM(CurrentValueColumn)`
  - Today's change: `=GOOGLEFINANCE(A2, "changepct") / 100`

- **Allocation charts in Sheets**: Use the same data validation dropdowns as F-083. Sector aggregation via SUMIFS or QUERY: `=QUERY(Holdings!A:G, "SELECT F, SUM(E) GROUP BY F LABEL SUM(E) 'Value'")`. Insert > Chart > Pie/Donut for allocation visualization.

- **Performance vs. Benchmark chart**:
  - Column with portfolio daily value: use GOOGLEFINANCE historical arrays for each ticker, multiply by shares, sum across row
  - Normalize: divide each day's value by the first day's value, multiply by 100
  - Chart both portfolio and benchmark normalized series as line chart

- **Limitations of GOOGLEFINANCE**:
  - Data is delayed ~15 minutes (not real-time)
  - Dividend history is not available — for dividend tracking, keep the yfinance Python path: `yf.Ticker(ticker).dividends`
  - Historical data may have gaps or inconsistencies for some tickers
  - Some international tickers require exchange prefix: `"LON:VOD"`, `"TYO:7203"`
  - Cryptocurrency: `=GOOGLEFINANCE("BTCUSD")` works for major crypto pairs
  - GOOGLEFINANCE results can be slow to refresh (Google rate-limits heavy usage)

- **Apps Script for advanced features** (replaces the Python auto-refresh script):
  ```
  function refreshPortfolio() {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName('Holdings');
    // Force GOOGLEFINANCE refresh by clearing and re-entering formulas
    var range = sheet.getRange('D2:D20'); // Price column
    var formulas = range.getFormulas();
    range.clearContent();
    SpreadsheetApp.flush();
    range.setFormulas(formulas);
  }
  ```
  Set a time-driven trigger: Edit > Current project's triggers > Add trigger > refreshPortfolio > Time-driven > Day timer > 6pm-7pm (after market close).

- **Keep Python/yfinance path for**: Historical data downloads >5 years, dividend history, detailed ticker.info (sector, beta, earnings dates), batch processing of 50+ tickers, and Sharpe/Alpha/Beta calculations that require daily return arrays.

**Output**: A portfolio dashboard system containing: (a) live portfolio positions table with P&L and returns (GOOGLEFINANCE in Sheets, yfinance in Python/Excel), (b) allocation analysis by holding, sector, and asset class, (c) performance vs benchmark chart with alpha/beta/Sharpe metrics, (d) dividend tracking and income projection, (e) formatted dashboard following F-083 layout principles, (f) auto-refresh mechanism (Apps Script trigger in Sheets, Python cron/LaunchAgent for Excel). Deliverable as Google Sheets (primary for live data), Excel (.xlsx snapshot), and Python script (for advanced analytics and automation).

---

## S3 Cortex Logging

```json
{
  "text": "F-084 Portfolio Dashboard with Live Market Data — executed for {context}",
  "collection": "procedures",
  "metadata": {
    "type": "training-execution",
    "procedure": "F-084",
    "rule_id": "TRAIN-F-084",
    "domain": "finance",
    "level": "tools",
    "complexity": "INTERMEDIATE",
    "tags": ["excel", "google-sheets", "portfolio", "dashboard", "googlefinance", "yfinance", "investment", "market-data"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Training Pipeline — Tools Level

### WHEN
When building or updating a portfolio monitoring system, or when applying dashboard skills to investment data

### HOW (violation detection)
- Portfolio P&L calculated without accounting for cost basis per share (using total cost instead of per-share basis = incorrect return calculation)
- Daily portfolio value computed across multi-market holdings without date alignment / forward-fill for missing trading days
- STOCKHISTORY used without documenting platform availability limitation (Excel 365 only, regional restrictions)
- Concentration risk thresholds (>15% single holding, >30% single sector) not checked or flagged in allocation analysis

### CONNECT
- **Upstream**: F-081 (lookup functions for position data retrieval), F-083 (dashboard construction layout and formatting patterns), F-082 (pivot-style aggregation concepts for sector/asset class)
- **Downstream**: Crypto monitoring (`~/.nexus/projects/crypto/`), F-014 (Comprehensive Ratio Dashboard — shared visualization patterns)

### VERIFY
- [ ] Procedura a fost executată complet (toate steps 1-8)?
- [ ] Output spec deliverables verificate (live positions table, allocation analysis, benchmark chart, dividend tracking, formatted dashboard, auto-refresh)?
- [ ] VK emis cu toate câmpurile completate?
- [ ] STOCKHISTORY platform caveat documented and Power Query / GOOGLEFINANCE alternatives provided?
- [ ] Date alignment handled for multi-market portfolios (forward-fill on missing trading days)?

**VK**: `[F-084] Portfolio Dashboard with Live Market Data | {context} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-081 | Receives from | INDEX-MATCH/XLOOKUP used for looking up position data and market metrics from reference tables |
| F-083 | Receives from | Dashboard layout grid, KPI card design, chart formatting, conditional formatting patterns, and sheet protection all come from F-083 |
| F-082 | Receives from | Sector/asset class aggregation uses pivot-style grouping concepts from F-082 |
| F-002/F-003 | Parallel to | Financial ratios (P/E, D/E) retrieved for individual holdings mirror ratio analysis methodology |
| Crypto monitoring | Feeds into | Portfolio dashboard patterns and GOOGLEFINANCE crypto support extend to `~/.nexus/projects/crypto/` |
| F-014 | Parallel to | Comprehensive Ratio Dashboard shares visualization patterns — F-084 is investment-focused, F-014 is company-focused |

---
---

# End of File
# Procedures: 4 (F-081, F-082, F-083, F-084)
# Domain: Finance — Excel & Google Sheets Mastery
# Template: FORGE Standard (Training Variant) v1.0
