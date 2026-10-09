# CUS314 BIR-EIS BIR Detail Report (Philippines) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | BIR-EIS BIR Detail Report (Philippines) |
| Code | CUS314 |
| JIRA Ticket | HERO-68289 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| Report Design | bir_sfe_detail_ph |
| Database | MySQL & MSSQL |

## Issue Summary

Refactor CUS314 BIR-EIS BIR Detail Report (Philippines) (`bir_sfe_detail_ph`) so that the formal rptdesign file reads the new standard `pos_bir_*` tables instead of legacy `rpt_dyn_pos_bir_*` dynamic tables.

### Scope
- Refactor `bir_sfe_detail_ph` (MySQL + SQL Server).
- Detail layer reads `pos_bir_check_payments` or `pos_bir_check_payments_temporary` (one row per payment / running number).
- Align with Action 4 on-demand routing for ranges beyond 3 months.

### Key Changes
- **Data Source Switch**: Detail report reads from `pos_bir_check_payments` (within retention) or `pos_bir_check_payments_temporary` (on-demand) instead of legacy dynamic tables.
- **On-Demand Routing**: When the selected date range is older than retention, the viewer routes to the On-Demand job form; ranges over 3 months are rejected.
- **Transitional Legacy Design**: A `_legacy.rptdesign` file is delivered alongside the formal design. Until migration completes, the Report Module routes to the legacy file; after migration completes, it uses the formal file.
- **No New Parameters**: Existing date range and filter parameters are unchanged.
- **No Field Definition Changes**: Layout columns keep the existing `dpbb_*` / `dpbp_*` aliases; only the SQL data source changes.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | (single-select) Selects the business date range for the report. Detail report date range is limited to at most 3 months; since-opening is not supported. Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days | Current Business Day, Last Business Day, Specified as Below | Current Business Day |
| Shops | (multi-select) Select shops to filter report data. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Outlets | (multi-select) Select outlets to filter report data. | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Title | Show corresponding report title by selection [Shops / Outlets / Date type]. Show report name 'BIR-EIS BIR Detail Report (Philippines)'. If select a single shop show shop name, else no show. If select a single outlet show outlet name, else no show. If select one day show target date, if select range show target range date. | - |
| Station / Outlet | Show outlet name resolved from POS master data at runtime. | - |
| Running Number | Show BIR-EIS running number from `pos_bir_daily_infos`. | - |
| Payment Method | Show payment method name resolved from POS master data at runtime. | - |
| Payment-level Amounts | Payment-level precomputed amounts from `pos_bir_check_payments` / `pos_bir_check_payments_temporary` table columns. | - |
| Check Number | Show check number. | - |
| Business Date | Show business date. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format | - |
| ** END OF REPORT ** | End of report marker. | - |
| Printed At | Show printed at time. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Time Format | - |
| Printed By | Show operator last name and first name. | - |
| Report Code | Show report code "CUS314" (System Management > System Configuration > Report Module > Show Footer Report Code: Yes = show; No = hide). | - |
| Page Number | Show page number (P.). | - |

> **Note**: Layout columns keep the existing `dpbb_*` / `dpbp_*` aliases; only the SQL data source changes from legacy `rpt_dyn_*` to standard `pos_bir_*` tables.

## Report Data Accuracy Verification

### 1. All Default Parameters
**Prerequisite(s):**
  1. Migration status is completed (formal design is active) OR transitional `_legacy.rptdesign` is present.
  2. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  3. Set the desired date format, time format, rounding, decimal places, currency sign, and thousands separator.

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Select "default" from all parameters:
    2.1. Select "Current Business Day" from Date Type.
    2.2. Select "All" from Shops.
    2.3. Select "All" from Outlets.
  3. Click "Run" button.
  4. Verify report displays data for all default parameter combinations.
  5. Verify the report reads from `pos_bir_check_payments` (within retention).

**Test Result(s):**
  1. Report loads successfully.
  2. Data displayed is correct for expected parameter combinations.
  3. Data source is `pos_bir_check_payments` (formal design) or `rpt_dyn_*` (legacy design).
  4. One row per payment / running number is displayed.
  5. No errors displayed.

### 2. [A Single Outlet, Last Business Day, A Single Shop]
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Last Business Day" from Date Type.
    2.2. Select "Single" from Shops (select one specific shop).
    2.3. Select "Single" from Outlets (select one specific outlet).
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify the report title shows the single shop name and single outlet name.

**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Report title shows the selected shop name and outlet name.
  4. One row per payment / running number is displayed.
  5. No errors displayed.

### 3. [Multiple Outlets, Specified as Below Business Day, Multiple Shops]
**Prerequisite(s):**
  1. Ensure the specified date range is within retention (within 3 months).

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Specified as Below" from Date Type and set a date range within 3 months (e.g., 2026-09-01 ~ 2026-09-15).
    2.2. Select "Multiple" from Shops (select multiple shops).
    2.3. Select "Multiple" from Outlets (select multiple outlets).
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify the report reads from `pos_bir_check_payments` (within retention).

**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Report title shows the date range.
  4. One row per payment / running number is displayed.
  5. No errors displayed.

## Report Functional Verification

### 1. Verify Report Information with Format Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Set the desired date format, time format, rounding, decimal places, currency sign, thousands separator, show footer report code, replace execution user, show shop short name.

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select "Language" from the menu.
  3. Select the desired language from the dropdown menu.
  4. Verify the language is set successfully.
  5. Verify the report displays data in the selected language.
  6. Verify the date format, time format, rounding, decimal places, currency sign, and thousands separator are applied correctly.
  7. Verify the footer report code (CUS314) is shown or hidden based on the configuration.
  8. Verify the shop short name is shown or hidden based on the configuration.

**Test Result(s):**
  1. Language is set successfully.
  2. Report displays data in the selected language.
  3. Date format, time format, rounding, decimal places, currency sign, and thousands separator are applied correctly.
  4. Footer report code is shown or hidden based on the configuration.
  5. Shop short name is shown or hidden based on the configuration.
  6. No errors displayed.

### 2. On-Demand Routing for Date Range Beyond Retention (Within 3 Months)
**Prerequisite(s):**
  1. Ensure the transitional `_legacy.rptdesign` is present and the report is listed for on-demand.
  2. Ensure migration status is completed (formal design is active).

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Set Date type to "Specified as Below" with a range older than the last 3 months but within 3 months (e.g., 2026-07-01 ~ 2026-07-31).
  3. Verify the report engine shows "Background generation required" and lists segments for on-demand generation.
  4. Click "Add to generation queue" to open the On-Demand Job form.
  5. Fill in the email interface, recipients, and format.
  6. Submit the form.
  7. Verify the result page shows the created Preset Parameters id and Scheduled Task ids.
  8. Go to Preset Parameters listing and verify the on-demand preset is displayed (e.g., "On-demand CUS314 2026-07-01 2026-07-31").
  9. Go to Report Management > Scheduled Tasks and verify the one-time BirPreprocessShell tasks are listed.
  10. After preprocess and AutoReport complete, verify the emailed report is produced from the formal design reading `pos_bir_check_payments_temporary`.

**Test Result(s):**
  1. On-demand routing is triggered for date range older than retention.
  2. On-Demand Job form is opened with email interface, recipients, and format fields.
  3. Preset Parameters id and Scheduled Task ids are created successfully.
  4. On-demand preset is displayed in the Preset Parameters listing.
  5. BirPreprocessShell tasks are listed in Scheduled Tasks.
  6. Emailed report is produced from the formal design reading `pos_bir_check_payments_temporary`.
  7. `pos_bir_check_payments_temporary` is written for the full range (no monthly segment reuse).
  8. No errors displayed.

### 3. Reject Date Range Over 3 Months (Detail Report Limit)
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Set Date type to "Specified as Below" with a range over 3 months (e.g., 2026-01-01 ~ 2026-06-30).
  3. Click "Run" button.
  4. Verify the report engine rejects the range and displays an appropriate error message.

**Test Result(s):**
  1. Date range over 3 months is rejected.
  2. Appropriate error message is displayed.
  3. No data is loaded for the invalid range.
  4. No errors displayed beyond the validation message.

### 4. Since-Opening Is Not Supported
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Verify that the "Since Opening" option is not available in the Date Type parameter.
  3. Attempt to set a since-opening date range if the option is present.
  4. Verify the report does not support since-opening.

**Test Result(s):**
  1. "Since Opening" option is not available in the Date Type parameter.
  2. Detail report does not support since-opening.
  3. No errors displayed.

### 5. Transitional Legacy Design Before Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is NOT completed (in_progress or not started).

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify the report uses the transitional `_legacy.rptdesign` against `rpt_dyn_*` tables.
  5. Verify the report data is correct and consistent with the legacy behavior.

**Test Result(s):**
  1. Report uses the transitional `_legacy.rptdesign` against `rpt_dyn_*` tables.
  2. Report data is correct and consistent with the legacy behavior.
  3. No errors displayed.

### 6. Formal Design After Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is completed.

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) from POS Module > Desktop Format.
  2. Set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify the report uses the formal design against `pos_bir_*` tables.
  5. Verify the report data is correct and consistent with the formal design.

**Test Result(s):**
  1. Report uses the formal design against `pos_bir_*` tables.
  2. Report data is correct and consistent with the formal design.
  3. No errors displayed.

### 7. Export Files to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select "Export" from the menu.
  3. Select the desired format (CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)) from the dropdown menu.
  4. Click "OK" button.
  5. Verify the exported file is saved to the specified location.
  6. Repeat for each export format.

**Test Result(s):**
  1. Exported file is successful for each format.
  2. Exported file is saved to the specified location.
  3. Exported file format matches the selected format.
  4. Exported file contains the expected data with correct `dpbb_*` / `dpbp_*` aliases.

### 8. Print Report to HTML, PDF
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select "Print" from the menu.
  3. Verify the printed report is saved to the specified location.
  4. Repeat for HTML and PDF formats.

**Test Result(s):**
  1. Printed report is successful.
  2. Printed report is saved to the specified location.
  3. Printed report format matches the selected format.
  4. Printed report contains the expected data.

### 9. Create Preset Parameters from Current Parameters
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select desired parameters (Date Type, Shops, Outlets).
  3. Select "Preset Parameters" from the menu.
  4. Click "OK" button.
  5. Input the relevant information for the preset parameters.
  6. Click "Save" button.
  7. Verify the preset parameters are set successfully.
  8. Verify the preset parameters are displayed in the report.
  9. Open the preset parameters' report from the list.
  10. Verify the report displays data for the preset parameters.

**Test Result(s):**
  1. Preset parameters are set successfully.
  2. Report filters by the preset parameters.
  3. Data displayed is correct for the preset parameters.
  4. No errors displayed.

### 10. Run Preset Parameters
**Step(s):**
  1. Open the preset parameters' report from the list.
  2. Verify the report displays data for the preset parameters.

**Test Result(s):**
  1. Data displayed is correct for the preset parameters.
  2. No errors displayed.

### 11. Auto Run Preset Parameters to Email, FTP, or SFTP with Auto Report Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Auto Report Configuration.
  2. Set the desired auto report configuration (Auto Report Employee, Restrict access to preset parameter of auto reports outside the current hierarchy, Attachment Expiration Days).

**Step(s):**
  1. Add new or edit auto report schedule.
  2. Select the preset parameters to run.
  3. Select the desired destination (Email, FTP, or SFTP) for the report.
  4. Verify the auto report schedule is set successfully.
  5. Wait for the scheduled run to complete.
  6. Verify the report is sent to the specified destination.

**Test Result(s):**
  1. Auto report schedule is set successfully.
  2. Report is sent successfully to the specified destination, time, format and other settings.
  3. Data is correct for the preset parameters.
  4. No errors displayed.

### 12. Auto-Cleanup On-Demand Jobs
**Prerequisite(s):**
  1. Go to Admin Console > System Configuration > Report Module.
  2. Set "Auto-cleanup On-Demand jobs" to "Yes".

**Step(s):**
  1. Create an on-demand job for the BIR-EIS BIR Detail Report (Philippines) with a date range beyond retention.
  2. Wait for the on-demand report to be sent.
  3. Verify the temporary preset and related one-time scheduled tasks are soft-deleted after the on-demand report is sent.
  4. Set "Auto-cleanup On-Demand jobs" to "No" and repeat the process.
  5. Verify the temporary preset and related one-time scheduled tasks are NOT soft-deleted.

**Test Result(s):**
  1. When "Auto-cleanup On-Demand jobs" is "Yes", the temporary preset and related one-time scheduled tasks are soft-deleted after the on-demand report is sent.
  2. When "Auto-cleanup On-Demand jobs" is "No", the temporary preset and related one-time scheduled tasks are NOT soft-deleted.
  3. No errors displayed.

### 13. Multi-Language Support for Report Parameter and Data
**Step(s):**
  1. Select "Language" from the menu.
  2. Open BIR-EIS BIR Detail Report (Philippines).
  3. Verify the language is set successfully.
  4. Verify the report displays data in the selected language.
  5. Repeat for each supported language.

**Test Result(s):**
  1. Language is set successfully.
  2. Report displays data in the selected language.
  3. No errors displayed.

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Prerequisite(s):**
  1. Install Google Chrome, Mozilla Firefox, and Microsoft Edge browsers.

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines) in Google Chrome.
  2. Select "default" from all parameters:
    2.1. Select "Current Business Day" from Date Type.
    2.2. Select "All" from Shops.
    2.3. Select "All" from Outlets.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Repeat steps 1-4 in Mozilla Firefox and Microsoft Edge.

**Test Result(s):**
  1. Report loads successfully in all three browsers.
  2. No errors displayed in any browser.

### 2. Support Config Zone Compatibility
**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification (All Default Parameters, A Single Outlet, Multiple Outlets).
  3. Verify the report loads successfully for each Config Zone hierarchy level:
    3.1. Current hierarchy level.
    3.2. Lower hierarchy level.
    3.3. Upper hierarchy level.
    3.4. All hierarchy levels including lower levels.

**Test Result(s):**
  1. Report loads successfully for each Config Zone hierarchy level.
  2. Data displayed is correct for each hierarchy level.
  3. No errors displayed.

### 3. Support Data Service Compatibility - MySQL
**Prerequisite(s):**
  1. Ensure the database service is MySQL.
  2. Ensure the selected date range is less than 7 days.

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification and set date range to less than 7 days.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from MySQL.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from MySQL.

### 4. Support Data Service Compatibility - TiDB
**Prerequisite(s):**
  1. Ensure the database service supports both MySQL and TiDB.
  2. Ensure the selected date range is more than 7 days (but within 3 months for detail report).

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification and set date range to more than 7 days (within 3 months).
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from TiDB.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from TiDB.

### 5. Support Data Service Compatibility - MSSQL
**Prerequisite(s):**
  1. Ensure the database service is MSSQL (SQL Server).

**Step(s):**
  1. Open BIR-EIS BIR Detail Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from MSSQL.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from MSSQL.

## Test Environment

- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
- **Database**: MySQL, TiDB, MSSQL

## QC Information

| Role | Name | Email |
| --- | --- | --- |
| Tester | Mike Lin | mike.lin@shijigroup.com |
| Reviewer | Eric Fang | eric.fang@shijigroup.com |

## Requirements Traceability

| Fix Version | JIRA Ticket |
| --- | --- |
| Report Sprint 20260921-20261002 | HERO-68289 |
| Epic | HERO-68279 |
