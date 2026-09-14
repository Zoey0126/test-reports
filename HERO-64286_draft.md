# POS225 OC/ENT Discount Report - Missing summary section Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | OC/ENT Discount Report |
| Code | POS225 |

## Report Functional Verification

### 1.Summary Section Display

#### 1.1.Verify the summary section is displayed in the on-screen report
**Prerequisite(s):**
  1. POS225 OC/ENT Discount Report is accessible in the test environment.
  2. OC/ENT discount data exists for the selected period.
**Step(s):**
  1. Open POS225 OC/ENT Discount Report.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Check the report layout after the crosstab detail area.
**Reproduction Result(s):**
  1. Before the fix, the summary section of POS225 was missing when the report was generated, because the crosstab script function onPrepareCell(cell, reportContext) called cell.getStyle().setDisplay("none") and hid the summary cells.
**Fix Result(s):**
  1. After the fix, the summary section is displayed correctly in the on-screen report.
  2. The report loads successfully with no errors displayed.

#### 1.2.Verify the summary section is displayed in all output formats
**Prerequisite(s):**
  1. OC/ENT discount data exists for the selected period.
**Step(s):**
  1. Open POS225 OC/ENT Discount Report and generate the report with default parameters.
  2. Export the report to PDF and check the summary section.
  3. Export the report to Excel/CSV and check the summary section.
  4. Print the report and check the printed output.
**Reproduction Result(s):**
  1. Before the fix, the summary section was also missing in the exported and printed outputs.
**Fix Result(s):**
  1. After the fix, the summary section is displayed correctly in the PDF, Excel/CSV, and printed outputs.
  2. All outputs are generated successfully with no errors displayed.

### 2.Verify summary values match the detail data
**Prerequisite(s):**
  1. OC/ENT discount data covering multiple discount types and outlets exists for the selected period.
**Step(s):**
  1. Open POS225 OC/ENT Discount Report and generate the report with default parameters.
  2. Sum the discount amounts in the crosstab detail area by discount type.
  3. Compare the sums with the corresponding values in the summary section.
**Reproduction Result(s):**
  1. Before the fix, the summary section was missing, so the totals could not be verified in the report.
**Fix Result(s):**
  1. After the fix, each summary value matches the sum of the corresponding detail data.
  2. No data is lost or wrongly aggregated.

### 3.Regression Scope
- POS225 OC/ENT Discount Report generation with default and common parameter combinations
- POS225 export and print functions (PDF, Excel/CSV, print)
- Report templates using similar crosstab script rendering, if any
- Related discount reports (e.g., POS202 Discount Detail Report, POS015 Discount Report By Check)

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
