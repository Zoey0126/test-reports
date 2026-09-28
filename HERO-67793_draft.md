# POS Report - New CUS Report "Detail Check Listing Report (with APC)" Test Report

## Functional Testing

### 1.New Custom Report Availability

#### 1.1.New CUS report "Detail Check Listing Report (with APC)" appears in the report list

**Prerequisite(s):**
1. User has access to run reports in the POS Platform.
2. The base report POS017 - Detail Check Listing Report is available in the system.
3. The custom report build containing this report has been deployed.

**Step(s):**
1. Log in to the POS Platform with a user account that has report access.
2. Navigate to the Report List / Report Module.
3. Search for "Detail Check Listing Report (with APC)".
4. Verify the original POS017 "Detail Check Listing Report" is also still listed.
5. Open both reports and compare their column structures.

**Test Result(s):**
1. The new custom report "Detail Check Listing Report (with APC)" is available in the report list.
2. The original POS017 "Detail Check Listing Report" remains unchanged and available.
3. The new report shares the same base structure as POS017 with additional dynamic APC columns.

### 2.Dynamic Item Department APC Columns

#### 2.1.APC columns are generated dynamically based on Item Departments present in the report data

**Prerequisite(s):**
1. The custom report "Detail Check Listing Report (with APC)" is deployed.
2. Multiple outlets exist with sales data covering different Item Departments (e.g., Food, Soft Beverage, Alcohol).
3. Report run covers a business date range with check-level data.

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Select Outlet 1 which has Food and Soft Beverage sales only.
3. Click "Run" to generate the report.
4. Verify the report columns at the check total and grand total levels.
5. Repeat with Outlet 2 which has Food, Soft Beverage, and Alcohol sales.
6. Verify the report columns at the check total and grand total levels.

**Test Result(s):**
1. Outlet 1 report run displays Food APC and Soft Beverage APC columns only (no Alcohol APC column).
2. Outlet 2 report run displays Food APC, Soft Beverage APC, and Alcohol APC columns.
3. No APC column appears for Item Departments that have no sales in the report data.
4. APC columns appear at the check total and grand total levels only (not on individual line items).
5. If a department has sales in one outlet but not in another, the APC column appears only in the outlet where the department has sales.

### 3.APC Calculation Logic

#### 3.1.Department APC is calculated as Department Sales divided by Cover Count

**Prerequisite(s):**
1. The custom report is deployed.
2. A report run contains at least one check with sales in multiple Item Departments and a cover count greater than zero.

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Run the report for a single outlet with a business date that has multi-department check data.
3. Identify a check with cover count > 0.
4. Verify the Department Sales, Cover Count, and Department APC values for each department on that check.
5. Manually compute Department Sales ÷ Cover Count for each department and compare with the reported APC value.

**Test Result(s):**
1. Each department's APC equals the corresponding Department Sales divided by the check's Cover Count.
2. Each department's APC uses the same total Cover Count of the check.
3. If a department has no sales on a check, the APC cell is blank or omitted rather than zero.

#### 3.2.APC handles zero cover count without division error

**Prerequisite(s):**
1. The custom report is deployed.
2. A report run contains at least one check with Cover Count = 0.

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Run the report for an outlet/period that includes a check with Cover Count = 0.
3. Locate the check with Cover Count = 0 in the report output.
4. Verify the Department APC cells on that check.
5. Confirm the report generation completes without errors.

**Test Result(s):**
1. APC cells display a dash (—) or "N/A" for departments on checks with Cover Count = 0.
2. The report generation completes without division errors or runtime failures.

### 4.APC Totals

#### 4.1.APC totals are displayed for each APC column at the grand total level

**Prerequisite(s):**
1. The custom report is deployed.
2. A report run produces multiple checks with non-zero cover counts and department sales.

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
3. Run the report for an outlet and a date range with multiple checks.
4. Locate the grand total section.
5. Verify the APC totals for each Department APC column.
6. Manually sum the individual per-check APC values and compare with the reported APC totals.

**Test Result(s):**
1. A corresponding APC total is displayed for each Department APC column at the grand total level.
2. The APC total equals the sum of the individual department APC values across all included checks.
3. Checks with APC values of dash or "N/A" due to zero cover count are excluded from the APC total sum.

### 5.Export Formats Containing APC Columns

#### 5.1.APC columns and totals are included in on-screen view and all export formats

**Prerequisite(s):**
1. The custom report is deployed.
2. A report run has been generated with APC columns and totals.

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Run the report with at least two Item Departments present.
3. Verify the on-screen report view contains APC columns and APC totals.
4. Export the report to Excel.
5. Export the report to CSV.
6. Open the exported files and verify the APC columns and totals.

**Test Result(s):**
1. The on-screen report view displays APC columns and APC totals.
2. The Excel export contains all APC columns and totals.
3. The CSV export contains all APC columns and totals.
4. No column data is lost or misaligned between on-screen view and exported files.

### 6.Related Functional Verification

#### 6.1.New custom report supports Format Configuration and parameter presets

**Prerequisite(s):**
1. The custom report "Detail Check Listing Report (with APC)" is deployed.
2. Format Configuration has been set under System Management > System Configuration > Report.

**Step(s):**
1. Go to System Management > System Configuration > Report > Format Configuration and set the desired date format.
2. Open "Detail Check Listing Report (with APC)".
3. Run the report with default parameters.
4. Save current parameters as a preset parameter.
5. Open the saved preset from the Preset Parameters list and run it.

**Test Result(s):**
1. The report renders using the configured date format.
2. The preset parameter is saved successfully and can be reopened.
3. The preset parameter run returns the expected data using the same parameters.

## Report Compatibility Verification

### 1.Browser Compatibility

#### 1.1.Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge

**Step(s):**
1. Open "Detail Check Listing Report (with APC)" using Google Chrome, Mozilla Firefox, and Microsoft Edge.
2. Select the default values for all parameters.
3. Click "Run" to generate the report.
4. Verify the report loads successfully and APC columns are displayed.

**Test Result(s):**
1. Report loads successfully in Chrome, Firefox, and Edge.
2. APC columns and totals render correctly in all three browsers.
3. No errors are displayed.

### 2.Config Zone Compatibility

#### 2.1.Support Config Zone Compatibility

**Step(s):**
1. Open "Detail Check Listing Report (with APC)" with users from different Config Zone levels (本层级, 下一层级, 上层级, 所有层级包括下层级).
2. Run the report using parameters from Report Data Accuracy Verification.
3. Verify the report loads successfully and the data is consistent with the user's Config Zone scope.

**Test Result(s):**
1. Report loads successfully across all Config Zone levels.
2. Data displayed matches the expected Config Zone scope.
3. No errors are displayed.

### 3.Data Service Compatibility

#### 3.1.Support Data Service Compatibility - MySQL

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Set the date range to less than 7 days so data is retrieved from MySQL.
3. Run the report and verify the data and APC columns.

**Test Result(s):**
1. Report loads successfully with MySQL data source.
2. APC columns are calculated correctly.
3. No errors are displayed.

#### 3.2.Support Data Service Compatibility - TiDB

**Step(s):**
1. Open "Detail Check Listing Report (with APC)".
2. Set the date range to more than 7 days so data is retrieved from TiDB.
3. Run the report and verify the data and APC columns.

**Test Result(s):**
1. Report loads successfully with TiDB data source.
2. APC columns are calculated correctly.
3. No errors are displayed.

## Report Information

| Field | Value |
| --- | --- |
| Name | Detail Check Listing Report (with APC) |
| Code | CUS - Detail Check Listing Report (with APC) |
| Base Report | POS017 - Detail Check Listing Report |
| APC Formula | Department Sales / Cover Count |
| APC Placement | Check total and grand total levels only |

## Test Environment

- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## Appendix

### Acceptance Criteria (from JIRA)

- New custom report created based on POS017 - Detail Check Listing Report.
- Existing POS017 structure, columns, and reporting logic remain unchanged.
- APC columns are generated dynamically based on the Item Departments available in the report data.
- Each Item Department has a corresponding APC column.
- APC is calculated as: Department Sales ÷ Cover Count.
- Departments with no sales in the report are not displayed as APC columns.
- APC totals are displayed in the report.
- APC total is the sum of individual APC values.
- The APC calculation correctly handles cases where the cover count is zero to avoid calculation/division errors.
- APC columns appear only at the check total and grand total levels (not on each line item).
- Exports: APC columns and totals appear in on-screen view and all export formats.
- Access: available to all users who can run any report.