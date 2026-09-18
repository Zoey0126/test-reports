# HERO-62973 POS Report - Create new custom report from CUS340 and restore previous version Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Daily OC/ENT Detail Report (Fixed Layout) |
| Code | CUS340 / CUS380 |
| Type | Improvement |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| N/A | No special configuration or parameter is required | N/A | N/A |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| N/A | This case does not involve field definition | N/A |

## Report Data Accuracy Verification

### 1. Verify Report Console Lists Both CUS340 and CUS380

**Step(s):**
  1. Log in to POS system with appropriate user credentials
  2. Navigate to Report Console
  3. Search for "CUS340" in the report list
  4. Search for "CUS380" in the report list

**Test Result(s):**
  1. CUS340 (Daily OC/ENT Detail Report - restored previous layout) is listed in Report Console
  2. CUS380 (Daily OC/ENT Detail Report - the previous latest CUS340 layout for AMC/Club) is listed in Report Console
  3. Both reports are accessible and can be opened
  4. No errors are displayed

### 2. Verify CUS380 Contains the Previous Latest CUS340 Layout (for AMC/Club)

**Prerequisite(s):**
  1. CUS380 report is available in Report Console

**Step(s):**
  1. Open CUS380 report from Report Console
  2. Run the report with default parameters
  3. Verify the report layout matches the previous latest CUS340 layout (with Rounding and Payment Remark)
  4. Export the report and verify data correctness

**Test Result(s):**
  1. CUS380 report opens successfully
  2. The report layout contains Rounding and Payment Remark fields (the previous latest CUS340 design)
  3. Report data is displayed correctly
  4. Report is applicable for AMC/Club hotels

### 3. Verify CUS340 is Restored to Previous Layout (for ISL/KSL/Kerry HK)

**Prerequisite(s):**
  1. CUS340 report is available in Report Console

**Step(s):**
  1. Open CUS340 report from Report Console
  2. Run the report with default parameters
  3. Verify the report layout matches the previous version (before Rounding and Payment Remark were added)
  4. Export the report and verify data correctness

**Test Result(s):**
  1. CUS340 report opens successfully
  2. The report layout is restored to the previous version (without Rounding and Payment Remark that were added later)
  3. Report data is displayed correctly
  4. Report is applicable for ISL/KSL/Kerry HK hotels

### 4. Verify CUS340 and CUS380 Produce Correct Data Independently

**Step(s):**
  1. Open CUS340 report and run with same business date and outlet parameters
  2. Record the output data
  3. Open CUS380 report and run with same business date and outlet parameters
  4. Record the output data
  5. Compare the data output of both reports

**Test Result(s):**
  1. Both reports run successfully with the same parameters
  2. CUS340 displays data in the restored previous layout
  3. CUS380 displays data in the AMC/Club layout
  4. Data values are consistent between the two reports where applicable
  5. Each report uses its own correct layout template

## Report Functional Verification

### 1. Verify Report Export Functionality for CUS340 and CUS380

**Step(s):**
  1. Open CUS340 report
  2. Export to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx) formats
  3. Open CUS380 report
  4. Export to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx) formats

**Test Result(s):**
  1. CUS340 exports successfully to all supported formats
  2. CUS380 exports successfully to all supported formats
  3. Exported files contain the expected data and correct layout
  4. No errors are displayed during export

### 2. Verify Report Print Functionality

**Step(s):**
  1. Open CUS340 report
  2. Print to HTML and PDF
  3. Open CUS380 report
  4. Print to HTML and PDF

**Test Result(s):**
  1. CUS340 prints successfully to HTML and PDF
  2. CUS380 prints successfully to HTML and PDF
  3. Printed output matches the expected layout for each report
  4. No errors are displayed

### 3. Verify Multi-language Support

**Step(s):**
  1. Set POS language to a non-English language (e.g., Chinese)
  2. Open CUS340 report and verify parameter names and data display
  3. Open CUS380 report and verify parameter names and data display

**Test Result(s):**
  1. CUS340 parameter names and data are displayed correctly in the selected language
  2. CUS380 parameter names and data are displayed correctly in the selected language
  3. No garbled text or translation errors are displayed

## Report Compatibility Verification

### 1. Support Browser Compatibility

**Step(s):**
  1. Open CUS340 and CUS380 reports in Google Chrome
  2. Open CUS340 and CUS380 reports in Mozilla Firefox
  3. Open CUS340 and CUS380 reports in Microsoft Edge

**Test Result(s):**
  1. Both reports load successfully in all three browsers
  2. Report layout is consistent across browsers
  3. No errors are displayed

### 2. Support Data Service Compatibility

**Step(s):**
  1. Run CUS340 and CUS380 with date range less than 7 days (MySQL)
  2. Run CUS340 and CUS380 with date range more than 7 days (TiDB, if applicable)

**Test Result(s):**
  1. Both reports retrieve data correctly from MySQL for short date ranges
  2. Both reports retrieve data correctly from TiDB for long date ranges (if applicable)
  3. No data source errors are displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
