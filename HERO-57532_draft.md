# HERO-57532 POS043 Audit Control Report | PDF can not show whole report Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | POS043 Audit Control Report |
| Code | POS043 |
| Issue Key | HERO-57532 |
| Issue Type | Bug |

## Bug Description

When exporting POS043 "Audit Control Report" to PDF with the Auto page size option, the report table is wider than the available page width on the current paper size (A3 portrait). The rightmost column "Fiscal Number" is truncated and cannot be shown completely in the exported PDF.

## Regression Test Scope

The following related modules and functions should be covered in regression testing:
- POS043 Audit Control Report PDF export functionality
- Other report PDF exports with Auto page size option
- Report table layout and column width calculations for PDF export
- Other page size options (A4, Letter, etc.) for POS043 report

## Test Cases

### 1. Verify PDF Export with Auto Page Size Option - Fiscal Number Column Display

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. POS043 report data exists with Fiscal Number values
  3. User has permission to run POS043 report and export to PDF

**Step(s):**
  1. Open Infrasys Cloud and go to the report module
  2. Select report POS043 "Audit Control Report"
  3. Enter the required report parameters (Business Day and Check No.) and run the report
  4. Export the result to PDF and choose the Auto page size option
  5. Open the exported PDF
  6. Check the table header on the right side for the "Fiscal Number" column

**Reproduction Result(s):**
  1. Report runs without error
  2. PDF export completes
  3. Before fix: the column "Fiscal Number" is cut off (for example only "Fiscal Num" is visible)
  4. The full Fiscal Number header and values cannot be displayed within the page margins

**Fix Result(s):**
  1. Report runs successfully
  2. PDF export completes successfully
  3. After fix: the full "Fiscal Number" column header and values are visible within the page margins
  4. All columns including Time, Outlet, Employee, Action, and Fiscal Number are fully displayed
  5. No truncation of column headers or data values

### 2. Verify PDF Export with A3 Portrait Page Size

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. POS043 report data exists

**Step(s):**
  1. Open Infrasys Cloud and go to the report module
  2. Select report POS043 "Audit Control Report"
  3. Enter the required report parameters and run the report
  4. Export the result to PDF and choose A3 portrait page size
  5. Open the exported PDF
  6. Verify all columns are fully visible

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: Fiscal Number column may be truncated on A3 portrait

**Fix Result(s):**
  1. Report runs successfully
  2. PDF export completes successfully
  3. All columns including Fiscal Number are fully visible on A3 portrait
  4. Column widths are adjusted to fit within the page size
  5. No errors displayed

### 3. Verify PDF Export with A4 Page Size

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. POS043 report data exists

**Step(s):**
  1. Open Infrasys Cloud and go to the report module
  2. Select report POS043 "Audit Control Report"
  3. Enter the required report parameters and run the report
  4. Export the result to PDF and choose A4 page size
  5. Open the exported PDF
  6. Verify all columns are fully visible

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: column truncation may occur on smaller page sizes

**Fix Result(s):**
  1. Report runs successfully
  2. PDF export completes successfully
  3. All columns are fully visible on A4 page size
  4. Column widths scale appropriately for the selected page size
  5. No errors displayed

### 4. Verify Report HTML Display

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. POS043 report data exists

**Step(s):**
  1. Open Infrasys Cloud and go to the report module
  2. Select report POS043 "Audit Control Report"
  3. Enter the required report parameters and run the report
  4. Verify the on-screen HTML report display
  5. Check that all columns including Fiscal Number are fully visible

**Reproduction Result(s):**
  1. Report runs without error
  2. HTML display shows all columns

**Fix Result(s):**
  1. Report runs successfully
  2. HTML display shows all columns correctly
  3. No layout issues in the HTML view
  4. No errors displayed

### 5. Verify Report Data Integrity After Layout Fix

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. Known POS043 report data with Fiscal Number values

**Step(s):**
  1. Open Infrasys Cloud and go to the report module
  2. Select report POS043 "Audit Control Report"
  3. Enter the required report parameters and run the report
  4. Export the result to PDF with Auto page size
  5. Verify the Fiscal Number values in the PDF match the on-screen report
  6. Verify no data is lost or altered due to column width adjustment

**Reproduction Result(s):**
  1. Report runs without error
  2. PDF exports but column values may be partially hidden

**Fix Result(s):**
  1. Report runs successfully
  2. PDF export completes successfully
  3. All Fiscal Number values are fully visible and match the on-screen report
  4. No data loss or alteration occurred during the layout fix
  5. No errors displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
