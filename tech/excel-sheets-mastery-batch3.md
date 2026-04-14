---
type: procedure
created: 2026-03-17
status: active
slug: excel-sheets-mastery-batch3
tags: [procedure, nexus]
---

# FORGE Procedures — Excel & Sheets Mastery (Batch 3)
# Procedure Count: 3
# Generated: 2026-03-03
# Template: FORGE Standard (Training Variant)
# Domain: Finance — Excel & Sheets Mastery

**Note on §5 (Dependencies)**: All procedures in this file include a §5 Dependencies section beyond the FORGE Standard requirement (§1-§4). This is a FORGE Training Variant extension that documents inter-procedure relationships and is retained for pedagogical value.

**Note on Google Sheets Implementation**: Each procedure includes a `### Google Sheets Implementation` subsection within §2 that provides the equivalent workflow for Google Sheets, including Apps Script, Solver add-on, and QUERY function alternatives where applicable.

---

## Table of Contents

### Tools Level — Excel & Sheets Mastery
| # | ID | Name | Complexity |
|---|-----|------|-----------|
| 1 | F-085 | Excel Macro for Recurring Financial Reports | INTERMEDIATE |
| 2 | F-086 | Goal Seek for Financial Decision-Making | BASIC |
| 3 | F-089 | P&L Database Construction with SUMIF | INTERMEDIATE |

---
---

# F-085: Excel Macro for Recurring Financial Reports

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-085

---

## S1 Problem

Financial teams spend 2-8 hours per period manually regenerating recurring reports: copying templates, pasting new data, reformatting, updating dates, and saving with timestamps. This manual cycle is error-prone (wrong month pasted, formatting inconsistencies, missed subtotals) and does not scale across departments or frequencies. Without automating recurring report generation, the same mechanical steps are repeated every week/month/quarter, consuming analyst time that should go to analysis rather than data assembly.

**Situations covered**:
- Monthly management P&L report generation from refreshed data
- Weekly cash position reports with standardized formatting
- Quarterly board summary packages with date-stamped archiving
- Multi-department or multi-entity report generation in batch
- Automating any Excel workflow that follows a repeatable template-to-output pattern
- **NOT suitable for**: Ad-hoc one-time analyses, reports requiring significant human judgment in layout (e.g., investor presentations with custom narratives), or environments where macro-enabled files (.xlsm) are blocked by IT policy

---

## S2 Procedure

1. **Identify the recurring report pattern.** Document the manual report workflow to be automated: (a) What is the report? (e.g., monthly management P&L, weekly cash position, quarterly board summary). (b) What are the sequential steps? (e.g., copy template, paste new data, format headers, update dates, calculate totals, save with date stamp). (c) What is the frequency? (weekly/monthly/quarterly). (d) What varies each period? (data values, dates in headers, sheet names). (e) What stays constant? (layout, formulas, formatting). Produce a written workflow specification with step count and estimated manual time per cycle.

2. **Record a macro for the base workflow.** In Excel: View > Macros > Record Macro. Name: `Generate_Monthly_Report`. Shortcut: Ctrl+Shift+R. Store in: This Workbook. Perform the manual steps once while recording: select template sheet, right-click > Move or Copy > Create a copy, rename sheet, select data paste range, format as table, update header cell with current date. Stop recording. View the generated code: Alt+F11 > Module1. This produces a starting VBA `Sub` that captures the raw sequence.

3. **Edit the VBA code for parameterization.** Open the VBA editor (Alt+F11). Refactor the recorded macro to accept parameters and handle dynamic elements:
   ```vba
   Sub Generate_Monthly_Report()
       Dim ws As Worksheet
       Dim reportDate As String
       Dim reportName As String

       reportDate = Format(Date, "YYYY-MM")
       reportName = "P&L_" & reportDate

       ' Copy template
       Sheets("Template").Copy After:=Sheets(Sheets.Count)
       ActiveSheet.Name = reportName

       ' Update header date
       Range("B1").Value = "P&L Report — " & Format(Date, "MMMM YYYY")

       ' Paste data from raw data sheet
       Sheets("RawData").Range("A1").CurrentRegion.Copy
       Sheets(reportName).Range("A5").PasteSpecial xlPasteValues
       Application.CutCopyMode = False

       ' Format
       Range("A5").CurrentRegion.Columns.AutoFit

       ' Calculate totals
       Dim lastRow As Long
       lastRow = Cells(Rows.Count, "A").End(xlUp).Row
       Range("E" & lastRow + 1).Formula = "=SUM(E5:E" & lastRow & ")"
   End Sub
   ```
   Document each VBA element: `Sub`/`End Sub` (procedure boundaries), `Dim` (variable declaration), `Range`/`Cells` (cell references), `For...Next` (loops), `If...Then...Else` (conditionals).

4. **Add loop logic for multi-sheet or multi-period generation.** For generating reports across multiple months or departments:
   ```vba
   Sub Generate_All_Monthly_Reports()
       Dim months As Variant
       Dim i As Integer
       months = Array("Jan", "Feb", "Mar", "Apr", "May", "Jun", _
                       "Jul", "Aug", "Sep", "Oct", "Nov", "Dec")

       Application.ScreenUpdating = False
       For i = 0 To UBound(months)
           ' Copy template for each month
           Sheets("Template").Copy After:=Sheets(Sheets.Count)
           ActiveSheet.Name = months(i) & "_2025"
           ' Filter raw data for this month
           Sheets("RawData").Range("A1").AutoFilter Field:=2, _
               Criteria1:=months(i)
           ' Copy filtered data
           Sheets("RawData").Range("A1").CurrentRegion.SpecialCells( _
               xlCellTypeVisible).Copy
           Sheets(months(i) & "_2025").Range("A5").PasteSpecial xlPasteValues
       Next i
       Application.ScreenUpdating = True
   End Sub
   ```

5. **Add formatting standardization routines.** Create a reusable formatting sub that applies financial report styling consistently:
   ```vba
   Sub FormatFinancialReport(ws As Worksheet)
       With ws
           ' Header formatting
           .Range("A1:H1").Font.Bold = True
           .Range("A1:H1").Font.Size = 14
           .Range("A1:H1").Interior.Color = RGB(47, 84, 150)
           .Range("A1:H1").Font.Color = RGB(255, 255, 255)

           ' Number formatting
           .Range("C5:H100").NumberFormat = "#,##0"
           .Range("I5:I100").NumberFormat = "0.0%"

           ' Subtotal rows bold
           Dim r As Long
           For r = 5 To .Cells(.Rows.Count, 1).End(xlUp).Row
               If Left(.Cells(r, 1).Value, 5) = "Total" Then
                   .Rows(r).Font.Bold = True
                   .Rows(r).Borders(xlEdgeTop).LineStyle = xlContinuous
               End If
           Next r

           ' Column autofit
           .Columns("A:I").AutoFit
       End With
   End Sub
   ```

6. **Implement date-stamped file saving.** Add automatic save-as functionality with directory creation:
   ```vba
   Sub SaveReportWithTimestamp()
       Dim savePath As String
       Dim fileName As String

       savePath = ThisWorkbook.Path & "\Reports\"
       fileName = "Financial_Report_" & Format(Date, "YYYY-MM-DD") & ".xlsx"

       ' Create directory if needed
       If Dir(savePath, vbDirectory) = "" Then MkDir savePath

       ' Save copy (keep original template open)
       ThisWorkbook.SaveCopyAs savePath & fileName
       MsgBox "Report saved: " & savePath & fileName
   End Sub
   ```

7. **Assign macros to buttons and configure security.** (a) Insert a button: Developer > Insert > Button (Form Control) > draw on sheet > assign macro. Label: "Generate Monthly Report". (b) Macro security: File > Options > Trust Center > Macro Settings > "Disable all macros with notification" (recommended). (c) Save workbook as `.xlsm` (macro-enabled format). (d) Add a startup message:
   ```vba
   Private Sub Workbook_Open()
       MsgBox "Financial Report Generator v1.0" & vbNewLine & _
              "Click 'Generate Report' button on the Dashboard sheet."
   End Sub
   ```

8. **Produce the Python equivalent for CLI execution.** Since Genie operates in CLI, the equivalent automation using openpyxl and Python:
   ```python
   import openpyxl
   from datetime import datetime
   from openpyxl.styles import Font, PatternFill, Alignment, Border, Side
   import pandas as pd

   def generate_monthly_report(template_path, data_path, output_dir):
       wb = openpyxl.load_workbook(template_path)
       template = wb['Template']

       # Create new sheet from template
       report_name = f"PL_{datetime.now().strftime('%Y_%m')}"
       new_ws = wb.copy_worksheet(template)
       new_ws.title = report_name

       # Load and paste data
       data = pd.read_csv(data_path)
       for r_idx, row in enumerate(data.values, start=5):
           for c_idx, value in enumerate(row, start=1):
               new_ws.cell(row=r_idx, column=c_idx, value=value)

       # Apply formatting
       header_fill = PatternFill('solid', fgColor='2F5496')
       header_font = Font(bold=True, color='FFFFFF', size=14)
       for cell in new_ws[1]:
           cell.fill = header_fill
           cell.font = header_font

       # Save with timestamp
       output = f"{output_dir}/Financial_Report_{datetime.now():%Y-%m-%d}.xlsx"
       wb.save(output)
       return output
   ```
   This Python script replicates the VBA macro functionality for non-Excel environments.

### Google Sheets Implementation

Google Sheets does not support VBA. The equivalent automation layer is **Google Apps Script** (JavaScript-based), accessible via Extensions > Apps Script.

**Step-by-step Apps Script equivalents:**

**Step 3 equivalent — Parameterized report generation:**
```javascript
function generateMonthlyReport() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const template = ss.getSheetByName('Template');
  const rawData = ss.getSheetByName('RawData');

  // Date formatting
  const now = new Date();
  const reportDate = Utilities.formatDate(now, Session.getScriptTimeZone(), 'yyyy-MM');
  const reportName = 'P&L_' + reportDate;

  // Copy template to new sheet
  const newSheet = template.copyTo(ss);
  newSheet.setName(reportName);

  // Update header date
  const headerDate = Utilities.formatDate(now, Session.getScriptTimeZone(), 'MMMM yyyy');
  newSheet.getRange('B1').setValue('P&L Report — ' + headerDate);

  // Get raw data and paste values only
  const dataRange = rawData.getDataRange();
  const dataValues = dataRange.getValues();
  newSheet.getRange(5, 1, dataValues.length, dataValues[0].length).setValues(dataValues);

  // Auto-resize columns
  for (let col = 1; col <= dataValues[0].length; col++) {
    newSheet.autoResizeColumn(col);
  }

  // Calculate totals
  const lastRow = newSheet.getLastRow();
  newSheet.getRange(lastRow + 1, 5).setFormula('=SUM(E5:E' + lastRow + ')');
}
```

**Step 4 equivalent — Multi-period batch generation:**
```javascript
function generateAllMonthlyReports() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const template = ss.getSheetByName('Template');
  const rawData = ss.getSheetByName('RawData');
  const months = ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'];
  const allData = rawData.getDataRange().getValues();
  const headers = allData[0];
  const monthColIndex = 1; // Column B = month field (0-indexed)

  months.forEach(function(month) {
    const newSheet = template.copyTo(ss);
    newSheet.setName(month + '_2025');

    // Filter data for this month
    const filtered = allData.filter(function(row, idx) {
      return idx === 0 || row[monthColIndex] === month;
    });

    if (filtered.length > 1) {
      newSheet.getRange(5, 1, filtered.length, filtered[0].length).setValues(filtered);
    }
  });

  SpreadsheetApp.flush(); // Force UI update
}
```

**Step 5 equivalent — Formatting standardization:**
```javascript
function formatFinancialReport(sheet) {
  // Header formatting
  const headerRange = sheet.getRange('A1:H1');
  headerRange.setFontWeight('bold');
  headerRange.setFontSize(14);
  headerRange.setBackground('#2F5496');
  headerRange.setFontColor('#FFFFFF');

  // Number formatting
  const lastRow = sheet.getLastRow();
  sheet.getRange(5, 3, lastRow - 4, 6).setNumberFormat('#,##0');
  sheet.getRange(5, 9, lastRow - 4, 1).setNumberFormat('0.0%');

  // Bold subtotal rows
  const values = sheet.getRange(5, 1, lastRow - 4, 1).getValues();
  values.forEach(function(row, idx) {
    if (String(row[0]).substring(0, 5) === 'Total') {
      const rowNum = idx + 5;
      sheet.getRange(rowNum, 1, 1, 9).setFontWeight('bold');
      sheet.getRange(rowNum, 1, 1, 9).setBorder(true, null, null, null, null, null);
    }
  });
}
```

**Step 6 equivalent — Date-stamped saving (Google Drive):**
```javascript
function saveReportCopy() {
  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const now = new Date();
  const dateStr = Utilities.formatDate(now, Session.getScriptTimeZone(), 'yyyy-MM-dd');
  const fileName = 'Financial_Report_' + dateStr;

  // Get or create Reports folder
  let folders = DriveApp.getFoldersByName('Reports');
  let folder;
  if (folders.hasNext()) {
    folder = folders.next();
  } else {
    folder = DriveApp.createFolder('Reports');
  }

  // Make a copy in the Reports folder
  const file = DriveApp.getFileById(ss.getId());
  const copy = file.makeCopy(fileName, folder);

  SpreadsheetApp.getUi().alert('Report saved: ' + copy.getUrl());
}
```

**Step 7 equivalent — Trigger assignment (replaces VBA buttons):**
```javascript
// Menu-based trigger (replaces VBA button assignment)
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('Report Generator')
    .addItem('Generate Monthly Report', 'generateMonthlyReport')
    .addItem('Generate All Monthly Reports', 'generateAllMonthlyReports')
    .addItem('Save Report Copy', 'saveReportCopy')
    .addToUi();
}

// Time-driven trigger (replaces manual button click — auto-run monthly)
// Set via Apps Script > Triggers > Add Trigger > generateMonthlyReport > Month timer
```

**Key VBA-to-Apps-Script mapping reference:**

| VBA Construct | Apps Script Equivalent |
|---|---|
| `Range("A1").Value` | `sheet.getRange('A1').getValue()` |
| `Range("A1:H1")` | `sheet.getRange('A1:H1')` |
| `Cells(r, c)` | `sheet.getRange(r, c)` |
| `ActiveSheet.Name = "X"` | `sheet.setName('X')` |
| `Sheets("X").Copy After:=...` | `sheet.copyTo(ss)` then `.setName()` |
| `Application.ScreenUpdating = False` | `SpreadsheetApp.flush()` at end |
| `MsgBox "text"` | `SpreadsheetApp.getUi().alert('text')` |
| `ThisWorkbook.SaveCopyAs` | `DriveApp.getFileById(id).makeCopy()` |
| `For i = 0 To UBound(arr)` | `arr.forEach(function(item) { })` |
| `If...Then...Else` | `if...else` |
| `Dim x As Type` | `const x = ...` or `let x = ...` (JavaScript typed via JSDoc) |
| `Format(Date, "YYYY-MM")` | `Utilities.formatDate(new Date(), tz, 'yyyy-MM')` |
| `Range.CurrentRegion` | `sheet.getDataRange()` (entire data region) |
| `.PasteSpecial xlPasteValues` | `target.setValues(source.getValues())` (values-only copy) |
| `.AutoFilter Field:=N, Criteria1:=X` | Manual filter via `getValues()` + `.filter()` in JS |
| `.SpecialCells(xlCellTypeVisible)` | No direct equivalent — filter array in JS, then `setValues()` |
| `.Font.Bold = True` / `.Font.Size = N` | `.setFontWeight('bold')` / `.setFontSize(N)` |
| `.Interior.Color = RGB(r,g,b)` | `.setBackground('#RRGGBB')` |
| `.Font.Color = RGB(r,g,b)` | `.setFontColor('#RRGGBB')` |
| `.NumberFormat = "#,##0"` | `.setNumberFormat('#,##0')` |
| `.Borders(xlEdgeTop).LineStyle` | `.setBorder(true, null, null, null, null, null)` |
| `Dir(path) / MkDir path` | `DriveApp.getFoldersByName(name)` / `DriveApp.createFolder(name)` |
| `Private Sub Workbook_Open()` | `function onOpen() { }` (reserved trigger name) |
| `Sub name() / End Sub` | `function name() { }` |

**Output**: A macro-enabled financial report generator containing: (a) VBA code for automated report generation from template, (b) parameterized macros for multi-period generation, (c) formatting standardization routines, (d) date-stamped auto-save functionality, (e) button-assigned macros with security configuration, (f) equivalent Python script for CLI-based execution, (g) full Google Apps Script equivalents for all VBA routines.

---

## S3 Cortex Logging

```json
{
  "text": "F-085 Excel Macro for Recurring Financial Reports — report automation built for {report_type}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-085",
    "domain": "finance",
    "level": "tools",
    "report_type": "{report_type}",
    "automation_method": "{vba|apps_script|python}",
    "tags": ["excel-mastery", "vba-macros", "apps-script", "automation", "recurring-reports", "openpyxl"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Business Evaluation Pipeline — Tools Level. Applied whenever an analyst or AI agent builds or reviews automated report generation workflows.

### WHEN
When any analyst or AI agent needs to automate a recurring financial report workflow in Excel or Google Sheets. Triggered at report template design phase, not at each report run.

### HOW
Violation detection rules:
- Macro created without parameterization (hardcoded dates, sheet names, or file paths) → violation
- VBA macro delivered without corresponding Apps Script equivalent for Google Sheets users → violation (completeness check)
- Report automation deployed without a date-stamped save routine (step 6) → violation (audit trail missing)
- Formatting applied inline instead of through a reusable `FormatFinancialReport` sub/function → violation (DRY principle)

### CONNECT
**Upstream dependencies:**
- F-066 (FMCG Report) or F-082 (Pivot Table Analysis) — should exist as the report content to be automated
- Familiarity with the report's manual workflow (documented in step 1)

**Downstream consumers:**
- F-066 — automates FMCG report production
- F-082 — automates pivot-based report refresh
- F-083 (Dashboard Construction) — macro can trigger dashboard updates

### VERIFY
- [ ] Procedura a fost executata complet (toate steps 1-8)?
- [ ] Output spec deliverables verificate (VBA + Apps Script + Python)?
- [ ] VK emis cu toate campurile completate?
- [ ] VBA-to-Apps-Script mapping table contains all constructs used in the procedure?
- [ ] Date-stamped save routine tested with valid output path?

**VK**: `[F-085] Excel Macro for Recurring Financial Reports | {report_type} | automation_method: {vba|apps_script|python} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-066 | Downstream target | FMCG P&L report is a prime candidate for macro automation |
| F-082 | Downstream target | Pivot-based reports benefit from automated refresh and regeneration |
| F-083 | Feeds into | Dashboard construction can be triggered by the same macro workflow |
| F-070 | Reference | Financial model sheet architecture provides the template patterns that macros replicate |

---
---

# F-086: Goal Seek for Financial Decision-Making

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: BASIC
**Rule**: TRAIN-F-086

---

## S1 Problem

Financial analysts frequently need to answer "what if" questions in reverse: given a desired outcome (target profit, required return, maximum affordable price), what input must change to achieve it? Without Goal Seek or equivalent solver capability, analysts resort to manual trial-and-error or build cumbersome data tables to approximate a single-point answer. This wastes time and introduces imprecision. Goal Seek solves one-variable inverse problems instantly, and Solver extends this to multi-variable constrained optimization. Without this procedure, break-even calculations, implied growth rate extraction, maximum acquisition pricing, and loan affordability analysis all require manual iteration.

**Situations covered**:
- Break-even unit/price analysis for product launches or budget planning
- Extracting market-implied growth rates from DCF models (reverse-engineering valuation)
- Finding the maximum acquisition price for a positive-NPV investment
- Determining the maximum affordable interest rate on a loan given a payment constraint
- Required margin improvement to hit an EBIT target
- Multi-variable portfolio optimization (Solver extension)
- **NOT suitable for**: Problems with multiple equally valid solutions (non-convex), discrete/integer optimization (use Solver with integer constraints), or problems where the relationship between input and output is not continuous (step functions, lookup-dependent models)

---

## S2 Procedure

1. **Understand Goal Seek parameters.** Goal Seek solves for one unknown variable given a target outcome. Three inputs required: (a) **Set Cell** — the cell containing the formula whose result you want to achieve (the output). (b) **To Value** — the target value you want the Set Cell to reach. (c) **By Changing Cell** — the single input cell that Goal Seek will adjust to reach the target. Excel: Data > What-If Analysis > Goal Seek. Constraint: Goal Seek adjusts exactly ONE variable at a time. For multi-variable optimization, use Solver (see step 8).

2. **Use Case 1: Break-even analysis.** Given: Revenue = Price x Units, Total Cost = Fixed Costs + (Variable Cost x Units), Profit = Revenue - Total Cost. Set up in Excel: Cell B1 = Price (e.g., $25), B2 = Variable Cost ($12), B3 = Fixed Costs ($100,000), B4 = Units (unknown), B5 = `=B1*B4 - B3 - B2*B4` (Profit). Goal Seek: Set Cell = B5, To Value = 0, By Changing Cell = B4. Result: Units = Fixed Costs / (Price - Variable Cost) = 100,000 / (25-12) = 7,693 units. In Python: `from scipy.optimize import brentq; breakeven = brentq(lambda units: 25*units - 100000 - 12*units, 0, 100000)`.

3. **Use Case 2: Required revenue growth rate for target valuation.** Link to F-065 (3-Statement Model) and F-069 (Sensitivity Analysis). Given a DCF model where Enterprise Value depends on projected revenue growth: Set Cell = Enterprise Value cell, To Value = current market cap (e.g., $50B), By Changing Cell = Revenue Growth Rate input cell. Goal Seek finds the growth rate implied by the current stock price (the "market-implied growth rate"). If the result (e.g., 12%) exceeds your base case (e.g., 8%), the market is pricing in higher growth than your model. In Python: `implied_growth = brentq(lambda g: dcf_model(growth=g) - 50e9, 0.01, 0.30)`.

4. **Use Case 3: Maximum acquisition price for positive NPV.** Given a project/acquisition with projected cash flows and a required WACC: NPV = -Acquisition_Price + PV(future_cash_flows). Set Cell = NPV cell, To Value = 0, By Changing Cell = Acquisition Price cell. Goal Seek returns the maximum price at which the investment breaks even. Any price below this creates value. Example: `=NPV(0.10, CF1:CF10) - AcquisitionPrice`. In Python: `max_price = npf.npv(0.10, cash_flows)` (the NPV of cash flows is itself the maximum price).

5. **Use Case 4: Required interest rate for loan affordability.** Given: desired monthly payment = $2,000, loan amount = $400,000, term = 30 years. What is the maximum interest rate? Cell B5 = `=PMT(B1/12, 30*12, -400000)` where B1 = interest rate. Goal Seek: Set Cell = B5, To Value = 2000, By Changing Cell = B1. Result: maximum rate where payment stays at $2,000. Links to F-068 (Loan Amortization). In Python: `max_rate = brentq(lambda r: npf.pmt(r/12, 360, -400000) - 2000, 0.001, 0.20)`.

6. **Use Case 5: Required margin improvement for EBIT target.** Given an income statement model: EBIT = Revenue x (1 - COGS%) x (1 - OpEx_as_%_of_GP). Target EBIT = $5M. Set Cell = EBIT, To Value = 5,000,000, By Changing Cell = COGS% (or OpEx%). Goal Seek finds the cost reduction needed. Combine with F-066 (FMCG Report) to find required margin by product group.

7. **Build a Goal Seek calculator workbook.** Create a sheet with all five use cases pre-built: (a) Break-even calculator — inputs for price, variable cost, fixed cost; output = break-even units. (b) Implied growth rate — inputs for DCF parameters; output = market-implied growth. (c) Maximum price — inputs for cash flows and WACC; output = max acquisition price. (d) Loan rate finder — inputs for payment, principal, term; output = max rate. (e) Margin target — inputs for revenue, target EBIT; output = required margin. Each section has clearly labeled Set Cell, To Value, and By Changing Cell with instructions. In Python: create a unified solver function:
   ```python
   from scipy.optimize import brentq

   def goal_seek(func, target, x_range=(0, 1)):
       """Single-variable root finder — equivalent to Excel Goal Seek."""
       return brentq(lambda x: func(x) - target, *x_range)
   ```

8. **Document Solver for multi-variable optimization.** When Goal Seek's single-variable limitation is insufficient: Excel Solver (Data > Solver) handles: (a) multiple changing cells simultaneously, (b) constraints (e.g., all values >= 0, budget <= $1M), (c) optimization objectives (maximize profit, minimize cost). Example: maximize portfolio return subject to total weight = 100%, each weight between 0-25%, portfolio volatility <= 15%. Solver setup: Objective = portfolio return cell, By Changing = weight cells, Subject to constraints. In Python:
   ```python
   from scipy.optimize import minimize

   result = minimize(
       lambda w: -portfolio_return(w),
       x0=initial_weights,
       constraints={'type': 'eq', 'fun': lambda w: sum(w) - 1},
       bounds=[(0, 0.25)] * n_assets
   )
   ```
   Note: Solver requires the Excel Solver Add-in to be enabled (File > Options > Add-ins > Manage Excel Add-ins > Solver Add-in).

### Google Sheets Implementation

Google Sheets has **no native Goal Seek** feature. Three alternative paths are documented below, from simplest to most powerful.

**Path A: Solver Add-on for Google Sheets**

Install a Solver add-on from Extensions > Add-ons > Get add-ons > search "Solver". Note: Google Sheets has **no native Solver** — all options are third-party add-ons. The most established free option is "Solver" by FrontlineSolvers (same company behind Excel's Solver). "OpenSolver" is another alternative. Once installed:

1. Set up the spreadsheet identically to the Excel version (Set Cell with formula, By Changing Cell with initial guess).
2. Extensions > Solver > define: Objective cell, Variable cells, Constraints, Goal (maximize/minimize/target value).
3. Click Solve. The add-on iterates to find the solution.

Limitations: Solver add-ons in Sheets are slower than Excel's native Solver and may not handle complex non-linear models well. Best for simple single-variable and small multi-variable problems.

**Path B: Custom Apps Script Goal Seek implementation**

Build a Goal Seek equivalent directly in Apps Script using bisection method:

```javascript
/**
 * Goal Seek implementation for Google Sheets.
 * Adjusts changingCell until setCell reaches targetValue.
 *
 * @param {string} setCellRef - Reference of the output cell (e.g., 'B5')
 * @param {number} targetValue - Desired value for the Set Cell
 * @param {string} changingCellRef - Reference of the input cell to adjust (e.g., 'B4')
 * @param {number} lowerBound - Lower bound for search (default: 0)
 * @param {number} upperBound - Upper bound for search (default: 1000000)
 * @param {number} tolerance - Acceptable error (default: 0.001)
 * @param {number} maxIter - Maximum iterations (default: 100)
 */
function goalSeek(setCellRef, targetValue, changingCellRef, lowerBound, upperBound, tolerance, maxIter) {
  lowerBound = lowerBound || 0;
  upperBound = upperBound || 1000000;
  tolerance = tolerance || 0.001;
  maxIter = maxIter || 100;

  const ss = SpreadsheetApp.getActiveSpreadsheet();
  const sheet = ss.getActiveSheet();
  const setCell = sheet.getRange(setCellRef);
  const changingCell = sheet.getRange(changingCellRef);

  for (let i = 0; i < maxIter; i++) {
    const mid = (lowerBound + upperBound) / 2;
    changingCell.setValue(mid);
    SpreadsheetApp.flush(); // Force recalculation

    const currentValue = setCell.getValue();
    const diff = currentValue - targetValue;

    if (Math.abs(diff) < tolerance) {
      return mid; // Solution found
    }

    // Determine search direction
    // Test: does increasing the changing cell increase or decrease the set cell?
    changingCell.setValue(mid + tolerance);
    SpreadsheetApp.flush();
    const testValue = setCell.getValue();
    const isIncreasing = testValue > currentValue;

    if ((diff > 0 && isIncreasing) || (diff < 0 && !isIncreasing)) {
      upperBound = mid;
    } else {
      lowerBound = mid;
    }
  }

  // Return best approximation after max iterations
  return (lowerBound + upperBound) / 2;
}

// Menu integration
function onOpen() {
  SpreadsheetApp.getUi()
    .createMenu('Goal Seek')
    .addItem('Run Goal Seek...', 'showGoalSeekDialog')
    .addToUi();
}

function showGoalSeekDialog() {
  const html = HtmlService.createHtmlOutputFromFile('GoalSeekDialog')
    .setWidth(350)
    .setHeight(300);
  SpreadsheetApp.getUi().showModalDialog(html, 'Goal Seek');
}
```

**Path C: Python scipy.optimize (preferred for CLI — most powerful)**

This is the recommended path for Genie and any CLI-based execution. All five use cases implemented:

```python
from scipy.optimize import brentq, minimize
import numpy_financial as npf

# Use Case 1: Break-even
def breakeven_units(price, var_cost, fixed_cost):
    return brentq(lambda u: price * u - fixed_cost - var_cost * u, 0, 1e8)

# Use Case 2: Implied growth rate
def implied_growth(dcf_func, market_cap, growth_range=(0.01, 0.50)):
    return brentq(lambda g: dcf_func(growth=g) - market_cap, *growth_range)

# Use Case 3: Max acquisition price (direct — NPV of cash flows)
def max_acquisition_price(cash_flows, wacc):
    return npf.npv(wacc, cash_flows)

# Use Case 4: Max affordable interest rate
def max_loan_rate(payment, principal, years):
    return brentq(lambda r: npf.pmt(r / 12, years * 12, -principal) - payment, 0.001, 0.30)

# Use Case 5: Required margin for EBIT target
def required_margin(revenue, target_ebit, opex_pct):
    return brentq(lambda cogs_pct: revenue * (1 - cogs_pct) * (1 - opex_pct) - target_ebit, 0.01, 0.99)
```

**Decision framework: Goal Seek vs Solver vs Sensitivity Analysis (F-069)**

| Need | Tool | Sheets Path |
|---|---|---|
| Single-variable, single target value | Goal Seek | Apps Script (Path B) or Python (Path C) |
| Multi-variable, constrained optimization | Solver | Solver Add-on (Path A) or Python scipy (Path C) |
| Map the entire output surface across input ranges | Sensitivity / Data Tables (F-069) | Manual or Python parameter sweep |
| Validate a sensitivity table result at a specific point | Goal Seek first, then confirm on sensitivity surface | Combine Paths B/C with F-069 |

**Output**: A Goal Seek reference workbook containing: (a) five pre-built financial decision-making calculators, (b) step-by-step Goal Seek instructions for each, (c) Python scipy equivalents for CLI execution, (d) Google Sheets implementation via three paths (Solver add-on, Apps Script custom Goal Seek, Python scipy), (e) Solver documentation for multi-variable scenarios, (f) decision framework for when to use Goal Seek vs Solver vs Sensitivity Analysis.

---

## S3 Cortex Logging

```json
{
  "text": "F-086 Goal Seek for Financial Decision-Making — solved {use_case} for {context}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-086",
    "domain": "finance",
    "level": "tools",
    "use_case": "{breakeven|implied_growth|max_price|loan_rate|margin_target|solver_multi}",
    "method": "{excel_goal_seek|sheets_solver_addon|apps_script|python_scipy}",
    "tags": ["excel-mastery", "goal-seek", "solver", "optimization", "what-if-analysis", "scipy"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Business Evaluation Pipeline — Tools Level. Applied whenever an analyst or AI agent performs reverse-solve / what-if analysis on financial models.

### WHEN
When any analyst or AI agent needs to reverse-solve a financial model for a target outcome (break-even, implied rate, max price, required margin). Triggered at the analysis phase after the financial model is built.

### HOW
Violation detection rules:
- Goal Seek applied to a model where the relationship between input and output is non-continuous (step functions, lookup-dependent) → violation (wrong tool — use Solver or manual data table instead)
- Single-variable Goal Seek used when the problem has multiple changing variables or constraints → violation (should escalate to Solver / scipy.optimize.minimize)
- Goal Seek result accepted without sanity-checking the output against known bounds or business logic → violation (e.g., negative units, growth rate > 100%)
- Google Sheets implementation missing entirely when deliverable targets Sheets users → violation (must include at least one of the 3 alternative paths)

### CONNECT
**Upstream dependencies:**
- A working financial model or formula structure with one output cell and one adjustable input cell
- For Use Cases 2-3: F-065 (3-Statement Model) or F-029/F-030 (NPV/IRR) should be built first
- For Use Case 4: F-068 (Loan Amortization) provides the PMT formula structure

**Downstream consumers:**
- F-069 (Sensitivity Analysis) — Goal Seek validates single points on the sensitivity surface
- F-065 (3-Statement Model) — implied growth rate extraction
- F-068 (Loan Amortization) — affordability analysis
- F-066 (FMCG P&L) — required margin by product group

### VERIFY
- [ ] Procedura a fost executata complet (toate steps 1-8)?
- [ ] Output spec deliverables verificate (5 use cases + Solver + Google Sheets 3 paths)?
- [ ] VK emis cu toate campurile completate?
- [ ] Goal Seek result sanity-checked against business logic bounds?
- [ ] Google Sheets alternative path tested (at least one of: Solver add-on, Apps Script, Python)?

**VK**: `[F-086] Goal Seek for Financial Decision-Making | use_case: {use_case} | method: {method} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-069 | Bidirectional | Sensitivity Analysis maps the surface; Goal Seek finds a single point on it — use together |
| F-065 | Upstream for UC2 | 3-Statement Model provides the DCF structure for implied growth rate extraction |
| F-029/F-030 | Upstream for UC3 | NPV/IRR project evaluation provides the cash flow structure for max price calculation |
| F-068 | Upstream for UC4 | Loan Amortization model provides the PMT formula for affordability analysis |
| F-066 | Upstream for UC5 | FMCG P&L provides the margin structure for required margin improvement |
| F-033 | Related | Operating Breakeven Analysis covers the analytical (non-solver) approach to break-even |

---
---

# F-089: P&L Database Construction with SUMIF

**Status**: ACTIVE
**Created**: 2026-03-03
**Version**: 1.0
**Domain**: Finance — Excel & Sheets Mastery
**Level**: Tools
**Complexity**: INTERMEDIATE
**Rule**: TRAIN-F-089

---

## S1 Problem

Every financial analysis — ratio calculation, trend analysis, budgeting, forecasting — starts from a structured Profit & Loss statement. But raw accounting data arrives as flat transaction ledgers (from ERP systems, bank exports, or manual entries) without P&L hierarchy. Without a systematic method to transform raw transactions into a structured P&L using SUMIF/SUMIFS aggregation and a Chart of Accounts mapping, the analyst must manually classify and sum hundreds or thousands of transactions each period. This is slow, error-prone, and unrepeatable. Worse, without reconciliation checks, errors in the P&L propagate silently into all downstream analyses (F-002 ratios, F-005 forecasts, F-014 dashboards).

**Situations covered**:
- Transforming raw transaction ledgers (SAP, QuickBooks, CSV bank exports) into monthly P&L statements
- Building reusable P&L templates that auto-generate for any selected period
- Departmental/cost-center P&L breakdowns from a single transaction source
- Variance analysis comparing Actuals vs Budget vs Prior Year
- Annual P&L summaries with all 12 months and trend analysis
- Reconciliation of transaction totals to P&L Net Income
- **NOT suitable for**: Balance sheet construction (different aggregation logic — see F-010), cash flow statement (requires IS+BS delta calculation — see F-011), or non-standard accounting frameworks where standard P&L hierarchy does not apply

---

## S2 Procedure

1. **Design the raw transaction data structure.** Create or receive a transaction ledger with columns: Transaction_ID (unique), Date (YYYY-MM-DD), Posting_Period (1-12), Fiscal_Year, Account_Code (standardized numeric code), Account_Name, Department (optional), Amount (positive for credits/revenue, negative for debits/costs per accounting convention — OR all positive with a separate Debit/Credit indicator column), Description/Memo. Minimum viable dataset: 200+ transactions across 12 months, 15+ account codes. In Python:
   ```python
   import pandas as pd
   transactions = pd.DataFrame(columns=[
       'Txn_ID', 'Date', 'Period', 'Year',
       'Account_Code', 'Account_Name', 'Department',
       'Amount', 'Description'
   ])
   # Validate
   assert transactions['Txn_ID'].is_unique
   assert transactions['Amount'].dtype in ['float64', 'int64']
   ```

2. **Build the Chart of Accounts (COA) hierarchy.** Create a mapping table that classifies each account code into the P&L hierarchy:

   | Account_Code | Account_Name | P&L_Line | P&L_Section | Sign_Convention | Sort_Order |
   |---|---|---|---|---|---|
   | 4000 | Product Revenue | Revenue - Products | Net Revenue | + | 10 |
   | 4100 | Service Revenue | Revenue - Services | Net Revenue | + | 20 |
   | 5000 | Raw Materials | COGS - Materials | Cost of Goods Sold | - | 30 |
   | 5100 | Direct Labor | COGS - Labor | Cost of Goods Sold | - | 40 |
   | 6000 | Salaries & Wages | SG&A - Personnel | Operating Expenses | - | 50 |
   | 6100 | Rent & Utilities | SG&A - Facilities | Operating Expenses | - | 60 |
   | 7000 | Interest Expense | Interest | Below EBIT | - | 70 |
   | 8000 | Income Tax | Tax | Below EBT | - | 80 |

   The Sort_Order column ensures P&L lines appear in standard order. In Python: `coa = pd.read_csv('chart_of_accounts.csv')`.

3. **Aggregate transactions into P&L lines using SUMIF.** For each account code and period, sum all transaction amounts. Basic SUMIF: `=SUMIF(Transactions[Account_Code], COA_Code, Transactions[Amount])`. For period-specific aggregation using SUMIFS: `=SUMIFS(Transactions[Amount], Transactions[Account_Code], "4000", Transactions[Posting_Period], 1, Transactions[Fiscal_Year], 2025)` — this returns total Product Revenue for January 2025. Build a summary matrix: rows = Account_Codes (from COA), columns = Posting_Periods (1-12). Each cell formula:
   ```
   =SUMIFS(Transactions[Amount],
           Transactions[Account_Code], $A5,
           Transactions[Posting_Period], B$3,
           Transactions[Fiscal_Year], $B$1)
   ```
   In pandas: `monthly_summary = transactions.groupby(['Account_Code', 'Period', 'Year'])['Amount'].sum().reset_index()`.

4. **Add department/cost center dimension with SUMIFS.** Extend the aggregation to include department breakdowns:
   ```
   =SUMIFS(Transactions[Amount],
           Transactions[Account_Code], AccountCode,
           Transactions[Posting_Period], Month,
           Transactions[Department], "Marketing")
   ```
   This enables departmental P&L reporting. Create a 3D summary: Account x Period x Department. In Excel, build separate sheets per department or use a pivot table (F-082). In pandas:
   ```python
   dept_summary = transactions.groupby(
       ['Account_Code', 'Period', 'Department']
   )['Amount'].sum().unstack('Department').fillna(0)
   ```

5. **Construct the P&L hierarchy with calculated subtotals.** Build the standard P&L structure:
   - **Net Revenue** = SUM of all 4xxx account codes. Excel: `=SUMIFS(Transactions[Amount], Transactions[Account_Code], ">="&4000, Transactions[Account_Code], "<="&4999, Transactions[Posting_Period], Month)`.
   - **Cost of Goods Sold** = SUM of all 5xxx account codes.
   - **Gross Profit** = Net Revenue - COGS (formula, not SUMIF).
   - **Operating Expenses** = SUM of all 6xxx account codes.
   - **EBIT** = Gross Profit - Operating Expenses.
   - **Interest** = SUM of 7xxx accounts.
   - **EBT** = EBIT - Interest.
   - **Tax** = SUM of 8xxx accounts.
   - **Net Income** = EBT - Tax.

   Calculate margin percentages: Gross Margin = Gross Profit / Revenue, Operating Margin = EBIT / Revenue, Net Margin = Net Income / Revenue. In Python:
   ```python
   sections = {
       'Revenue': (4000, 4999), 'COGS': (5000, 5999),
       'OpEx': (6000, 6999), 'Interest': (7000, 7999), 'Tax': (8000, 8999)
   }
   for name, (lo, hi) in sections.items():
       mask = (summary['Account_Code'] >= lo) & (summary['Account_Code'] <= hi)
       pnl[name] = summary.loc[mask, 'Amount'].sum()
   pnl['Gross_Profit'] = pnl['Revenue'] - abs(pnl['COGS'])
   pnl['EBIT'] = pnl['Gross_Profit'] - abs(pnl['OpEx'])
   ```

6. **Generate automated monthly P&L statements.** Create a template that produces a full P&L for any selected month by changing a single cell (the month selector). Structure: Column A = P&L line items (from COA Sort_Order), Column B = Current Month Actual, Column C = Current Month Budget (if available), Column D = Variance, Column E = Variance %, Column F = YTD Actual, Column G = YTD Budget, Column H = YTD Variance. Each cell uses SUMIFS referencing the month selector:
   ```
   =SUMIFS(Transactions[Amount],
           Transactions[Account_Code], ">="&LookupStart,
           Transactions[Account_Code], "<="&LookupEnd,
           Transactions[Posting_Period], MonthSelector)
   ```
   YTD: change the period criteria to `"<="&MonthSelector`. In Python: parameterize by month and generate with `def generate_pnl(transactions, coa, year, month): ...`.

7. **Add variance analysis and commentary fields.** For each P&L line with variance > 5% (or a materiality threshold): (a) Absolute variance: Actual - Budget. (b) Percentage variance: (Actual - Budget) / ABS(Budget). (c) Conditional formatting: favorable variances (revenue above budget or cost below budget) in green, unfavorable in red. (d) Commentary column for management notes explaining significant variances. (e) Bridge analysis: rank the top 5 drivers of total variance from budget, largest impact first. In Python:
   ```python
   variance = pnl_actual - pnl_budget
   variance_pct = variance / pnl_budget.abs()
   top_drivers = variance.abs().nlargest(5)
   ```

8. **Build the annual P&L summary with all 12 months.** Create the full-year view: rows = P&L line items, columns = Jan through Dec + Full Year Total. Each monthly column uses SUMIFS for that period. Full Year = SUM of all 12 monthly columns. Add trend analysis: (a) Trailing 3-month average for each line item. (b) Run-rate projection: (YTD Actual / months elapsed) x 12. (c) Month-over-month change and trend direction indicator. Format with subtotal rows bolded, double-underline on Net Income, thousands separator, negative values in parentheses. In openpyxl: `numbers.FORMAT_NUMBER_COMMA_SEPARATED1` and custom format `'#,##0;(#,##0)'`.

9. **Validate and reconcile.** Run integrity checks: (a) Transaction total: SUM of all transactions = Net Income on the P&L (after accounting for sign convention). (b) Period check: SUM of all 12 monthly P&L = Full Year P&L. (c) Account completeness: every account code in transactions appears in the COA. (d) Balance check: Revenue + COGS + OpEx + Interest + Tax should net to Net Income. Excel:
   ```
   =IF(ABS(SUM(AllTransactions) - NetIncome) < 0.01,
       "RECONCILED",
       "DISCREPANCY: " & SUM(AllTransactions) - NetIncome)
   ```
   In Python: `assert abs(transactions['Amount'].sum() - pnl['Net_Income']) < 0.01, "Reconciliation failed"`.

### Google Sheets Implementation

SUMIF and SUMIFS work **identically** in Google Sheets — all formulas from steps 3-6 transfer without modification. The key Sheets-native addition is the **QUERY function**, which provides SQL-like aggregation that is more powerful than SUMIF for complex multi-dimensional groupings.

**QUERY function as Sheets-native alternative to SUMIF/SUMIFS:**

The QUERY function uses Google Visualization API Query Language (a SQL subset). It replaces multiple SUMIFS formulas with a single expression.

**Step 3 equivalent — Aggregate by account and period:**
```
=QUERY(Transactions!A:I,
       "SELECT Col5, SUM(Col8)
        WHERE Col5 IS NOT NULL AND Col4 = 2025 AND Col3 = 1
        GROUP BY Col5
        LABEL SUM(Col8) 'Jan_2025'")
```
Note: QUERY uses `Col1`, `Col2`, etc. (1-indexed positional references) when the data source is a range. Column mapping: Col1=Txn_ID, Col2=Date, Col3=Posting_Period, Col4=Fiscal_Year, Col5=Account_Code, Col6=Account_Name, Col7=Department, Col8=Amount, Col9=Description.

This returns all account codes with their January 2025 totals in a single formula. Compare to Excel where you need one SUMIFS per account code per month.

**Step 4 equivalent — Department dimension:**
```
=QUERY(Transactions!A:I,
       "SELECT Col5, Col7, SUM(Col8)
        WHERE Col5 >= 4000 AND Col5 <= 4999 AND Col3 = 1
        GROUP BY Col5, Col7
        LABEL SUM(Col8) 'Amount'")
```
This returns revenue by account code and department in one call.

**Step 5 equivalent — P&L section totals:**
```
=QUERY(Transactions!A:I,
       "SELECT SUM(Col8)
        WHERE Col5 >= 4000 AND Col5 <= 4999 AND Col3 = " & MonthSelector & "
        LABEL SUM(Col8) 'Net Revenue'")
```
For the full P&L hierarchy, QUERY's Google Visualization API Query Language does **NOT** support `CASE` expressions. Use separate QUERY calls per section or the FILTER + SUMPRODUCT fallback below:
```
=SUMPRODUCT((Transactions!E:E >= 4000) * (Transactions!E:E <= 4999) *
            (Transactions!C:C = MonthSelector) * (Transactions!D:D = YearSelector) *
            Transactions!H:H)
```
Note: In the SUMPRODUCT fallback, use actual column letters (E, C, D, H) because the data source is a column-range reference, not a QUERY range. Repeat for each P&L section (COGS: 5000-5999, OpEx: 6000-6999, Interest: 7000-7999, Tax: 8000-8999).

**Step 6 equivalent — Month selector with QUERY:**
```
=QUERY(Transactions!A:I,
       "SELECT Col5, SUM(Col8)
        WHERE Col3 <= " & MonthSelector & " AND Col4 = " & YearSelector & "
        GROUP BY Col5
        ORDER BY Col5
        LABEL SUM(Col8) 'YTD Actual'")
```

**Step 8 equivalent — Full year 12-month matrix:**
```
=QUERY(Transactions!A:I,
       "SELECT Col5, SUM(Col8)
        WHERE Col4 = " & YearSelector & "
        GROUP BY Col5
        PIVOT Col3")
```
The `PIVOT Col3` clause creates one column per unique Posting_Period value (1-12) — producing the full 12-month matrix in a single formula. Correct QUERY PIVOT syntax: `SELECT Col_agg, AGG_FUNC(Col_value) GROUP BY Col_agg PIVOT Col_pivot`. The PIVOT column (Col3 = Posting_Period) must appear in neither SELECT nor GROUP BY.

**PIVOT limitations and caveats:**
- PIVOT output columns are dynamically generated — their count and order depend on the data. If Posting_Period values are not integers 1-12 (e.g., dates, text month names), column ordering may be unexpected.
- Other formulas cannot reliably reference PIVOT output columns by position because the column count changes if data changes (e.g., a month with zero transactions may be omitted).
- For large datasets with many unique PIVOT values, the formula may hit Google Sheets' computation limits.
- **Alternative for stable column references**: Use 12 separate QUERY formulas (one per month) or a FILTER + SUMIFS combination:
  ```
  =SUMIFS(Transactions!H:H, Transactions!E:E, AccountCode, Transactions!C:C, MonthNumber, Transactions!D:D, YearSelector)
  ```
  This produces one value per cell with stable, referenceable output — preferred when downstream formulas need to reference specific month columns.

**When to use QUERY vs SUMIFS in Google Sheets:**

| Scenario | Recommended | Why |
|---|---|---|
| Single cell aggregation | SUMIFS | Simpler, familiar |
| Full table with multiple groupings | QUERY | One formula replaces entire grid |
| Dynamic column pivoting (months as columns) | QUERY with PIVOT | Not possible with SUMIFS alone |
| Complex filtering (LIKE, partial match) | QUERY with CONTAINS | More flexible than SUMIFS wildcards |
| Integration with existing Excel workbooks | SUMIFS | 1:1 compatibility |

**Output**: A complete P&L database system containing: (a) structured transaction ledger with validation, (b) Chart of Accounts mapping table with hierarchy, (c) SUMIF/SUMIFS-based aggregation engine producing monthly P&L statements, (d) departmental breakdown capability, (e) automated monthly and annual P&L generation, (f) variance analysis with budget comparison and commentary, (g) full-year 12-month summary with trend analysis, (h) reconciliation checks confirming data integrity, (i) Google Sheets QUERY function alternatives for all aggregation steps.

---

## S3 Cortex Logging

```json
{
  "text": "F-089 P&L Database Construction with SUMIF — built for {company_or_context} covering {period_range}",
  "collection": "business_evaluation",
  "metadata": {
    "type": "procedure_execution",
    "procedure": "F-089",
    "domain": "finance",
    "level": "tools",
    "company": "{company_or_context}",
    "period_range": "{period_range}",
    "account_count": "{number_of_accounts}",
    "aggregation_method": "{sumifs|query|python_pandas}",
    "tags": ["excel-mastery", "sumif", "sumifs", "query-function", "pnl-construction", "chart-of-accounts", "reconciliation"]
  }
}
```

---

## S4 Enforcement

### WHERE
PRISM Business Evaluation Pipeline — Tools Level. Applied whenever raw transaction data is transformed into a structured P&L statement.

### WHEN
When building a P&L from raw transaction data for any business evaluation, budgeting, or reporting task. Triggered at the data-to-analysis transition phase.

### HOW
Violation detection rules:
- P&L constructed without a Chart of Accounts (COA) mapping table → violation (step 2 skipped — ad-hoc classification is unrepeatable)
- SUMIF/SUMIFS aggregation completed without reconciliation check (step 9) → violation (undetected discrepancies propagate to all downstream analyses)
- Google Sheets QUERY formulas use cell references (A2, B3) instead of Col1/Col2 format when the data source is a range → violation (QUERY syntax error)
- P&L delivered without at least one variance analysis dimension (Actual vs Budget or vs Prior Year) → violation (step 7 skipped — raw P&L without variance has limited analytical value)

### CONNECT
**Upstream dependencies:**
- Raw transaction data (CSV, ERP export, or manual ledger) with account codes
- Chart of Accounts mapping must be defined or received
- F-081 (VLOOKUP/INDEX-MATCH) — COA lookup patterns used within this procedure

**Downstream consumers:**
- F-066 (FMCG Report) — adds SAP-specific and product/region dimensions on top of this P&L foundation
- F-014 (Comprehensive Ratio Dashboard) — uses P&L ratios
- F-082 (Pivot Table Analysis) — on the transaction data
- F-005 (Revenue Forecasting) — uses historical revenue from this P&L as starting point
- F-002/F-003/F-004 (Ratio Analysis) — uses margin figures derived here

### VERIFY
- [ ] Procedura a fost executata complet (toate steps 1-9)?
- [ ] Output spec deliverables verificate (P&L + COA + reconciliation)?
- [ ] VK emis cu toate campurile completate?
- [ ] Reconciliation check passes (transaction total = Net Income within 0.01)?
- [ ] QUERY formulas use correct Col1/Col2 column references (not cell references) for range data sources?

**VK**: `[F-089] P&L Database Construction with SUMIF | {company_or_context} | {period_range} | aggregation: {sumifs|query|python} | Tools level complete`

---

## S5 Dependencies

| Procedure | Relationship | Why |
|-----------|-------------|-----|
| F-066 | Feeds into (extension) | FMCG Report extends this P&L structure with SAP data, product/region dimensions |
| F-014 | Feeds into | Comprehensive Ratio Dashboard consumes P&L margin and profitability data |
| F-082 | Parallel/alternative | Pivot Table Analysis can perform similar aggregation but without enforced P&L hierarchy |
| F-005 | Feeds into | Revenue Forecasting uses the historical revenue series produced here |
| F-002 | Feeds into | Profitability Ratio Analysis requires Gross Profit, EBIT, Net Income from this P&L |
| F-003 | Feeds into | Liquidity Ratio Analysis cross-references P&L revenue for working capital ratios |
| F-065 | Related | 3-Statement Model references P&L structure for the Income Statement projection |
| F-081 | Dependency | VLOOKUP/INDEX-MATCH (F-081) patterns are used for COA lookups within this procedure |

---
---

# End of File
# Procedures: 3 (F-085, F-086, F-089)
# Total sections: 15 (3 procedures x 5 sections each)
# Cross-references: F-002, F-003, F-004, F-005, F-014, F-029, F-030, F-033, F-065, F-066, F-068, F-069, F-070, F-081, F-082, F-083, F-084 (17 unique)
