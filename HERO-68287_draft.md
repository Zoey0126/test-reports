# CUS012 BIR Report (Philippines) (By Running Number) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | BIR Report (Philippines) (By Running Number) |
| Code | CUS012 |
| JIRA Ticket | HERO-68287 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| Report Design | bir_ph_by_running_number |
| Database | MySQL only |

## Issue Summary

Refactor CUS012 BIR Report (Philippines) (By Running Number) (`bir_ph_by_running_number`) so that the formal rptdesign file reads the new standard `pos_bir_*` tables instead of legacy `rpt_dyn_pos_bir_*` dynamic tables.

### Scope
- Refactor `bir_ph_by_running_number` (MySQL + SQL Server).
- Filter `sum_type='r'`; use `pos_bir_daily_infos` for start/end running numbers.
- 3-table UNION + long-range on-demand support.

### Key Changes
- **Data Source Switch**: Summary report reads from the shared three-table UNION (`pos_bir_station_summaries` + `pos_bir_station_monthly_summaries` + `pos_bir_station_summaries_temporary`) instead of legacy `rpt_dyn_*` dynamic tables.
- **Running Number Filter**: Filters `sum_type='r'` to show data by running number.
- **Running Number Resolution**: Uses `pos_bir_daily_infos` for start/end running numbers.
- **Runtime Display Name Resolution**: Display names (station, payment) resolve from POS master data at runtime.
- **Long-Range On-Demand Support**: Supports on-demand generation for long date ranges beyond retention with monthly segments.
- **Transitional Legacy Design**: A `_legacy.rptdesign` file is delivered alongside the formal design. Until migration completes, the Report Module routes to the legacy file; after migration completes, it uses the formal file.
- **No New Parameters**: Existing date range and filter parameters are unchanged.
- **No Field Definition Changes**: Layout columns keep the existing `dpbb_*` / `dpbp_*` aliases; only the SQL data source changes.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | (single-select) Selects the business date range for the report. Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days | Current Business Day, Last Business Day, Specified as Below | Current Business Day |
| Shops | (multi-select) Select shops to filter report data. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Outlets | (multi-select) Select outlets to filter report data. | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Title | Show corresponding report title by selection [Shops / Outlets / Date type]. Show report name 'BIR Report (Philippines) (By Running Number)'. If select a single shop show shop name, else no show. If select a single outlet show outlet name, else no show. If select one day show target date, if select range show target range date. | - |
| Station / Outlet | Show outlet name resolved from POS master data at runtime. | Resolved from POS master data at runtime |
| Start Running Number | Show start running number from `pos_bir_daily_infos`. | From `pos_bir_daily_infos` WHERE `sum_type='r'` |
| End Running Number | Show end running number from `pos_bir_daily_infos`. | From `pos_bir_daily_infos` WHERE `sum_type='r'` |
| Payment Name | Show payment method name resolved from POS master data at runtime. | Resolved from POS master data at runtime |
| Summary Amounts | Precomputed station summary amounts from the 3-table UNION (`pos_bir_station_summaries` + `pos_bir_station_monthly_summaries` + `pos_bir_station_summaries_temporary`), filtered by `sum_type='r'`. | 3-table UNION WHERE `sum_type='r'` |
| ** END OF REPORT ** | End of report marker. | - |
| Printed At | Show printed at time. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Time Format | - |
| Printed By | Show operator last name and first name. | - |
| Report Code | Show report code "CUS012" (System Management > System Configuration > Report Module > Show Footer Report Code: Yes = show; No = hide). | - |
| Page Number | Show page number (P.). | - |

> **Note**: Layout columns keep the existing `dpbb_*` / `dpbp_*` aliases; only the SQL data source changes from legacy `rpt_dyn_*` to standard `pos_bir_*` tables. Data is filtered by `sum_type='r'` to show by running number.

## Report Data Accuracy Verification

### 1. All Default Parameters
**Prerequisite(s):**
  1. Migration status is completed (formal design is active) OR transitional `_legacy.rptdesign` is present.
  2. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  3. Set the desired date format, time format, rounding, decimal places, currency sign, and thousands separator.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Select "default" from all parameters:
    2.1. Select "Current Business Day" from Date Type.
    2.2. Select "All" from Shops.
    2.3. Select "All" from Outlets.
  3. Click "Run" button.
  4. Verify report displays data for all default parameter combinations.
  5. Verify the report reads from the 3-table UNION (`pos_bir_station_summaries` within retention), filtered by `sum_type='r'`.
  6. Verify start/end running numbers are resolved from `pos_bir_daily_infos`.
  7. Verify display names (station, payment) are resolved from POS master data at runtime.

**Test Result(s):**
  1. Report loads successfully.
  2. Data displayed is correct for expected parameter combinations.
  3. Data source is the 3-table UNION filtered by `sum_type='r'` (formal design) or `rpt_dyn_*` (legacy design).
  4. Start/end running numbers are resolved from `pos_bir_daily_infos`.
  5. Display names (station, payment) are resolved from POS master data at runtime.
  6. No errors displayed.

### 2. [A Single Outlet, Last Business Day, A Single Shop]
**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Last Business Day" from Date Type.
    2.2. Select "Single" from Shops (select one specific shop).
    2.3. Select "Single" from Outlets (select one specific outlet).
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify the report title shows the single shop name and single outlet name.
  6. Verify start/end running numbers are correct for the selected outlet and date.

**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Report title shows the selected shop name and outlet name.
  4. Start/end running numbers are resolved from `pos_bir_daily_infos`.
  5. Display names are resolved from POS master data at runtime.
  6. No errors displayed.

### 3. [Multiple Outlets, Specified as Below Business Day, Multiple Shops]
**Prerequisite(s):**
  1. Ensure the specified date range is within retention.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Specified as Below" from Date Type and set a date range within retention (e.g., 2026-09-01 ~ 2026-09-15).
    2.2. Select "Multiple" from Shops (select multiple shops).
    2.3. Select "Multiple" from Outlets (select multiple outlets).
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify the report reads from the 3-table UNION (`pos_bir_station_summaries` within retention), filtered by `sum_type='r'`.

**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Report title shows the date range.
  4. Start/end running numbers are resolved from `pos_bir_daily_infos`.
  5. Display names are resolved from POS master data at runtime.
  6. No errors displayed.

## Report Functional Verification

### 1. Verify Report Information with Format Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Set the desired date format, time format, rounding, decimal places, currency sign, thousands separator, show footer report code, replace execution user, show shop short name.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number).
  2. Select "Language" from the menu.
  3. Select the desired language from the dropdown menu.
  4. Verify the language is set successfully.
  5. Verify the report displays data in the selected language.
  6. Verify the date format, time format, rounding, decimal places, currency sign, and thousands separator are applied correctly.
  7. Verify the footer report code (CUS012) is shown or hidden based on the configuration.
  8. Verify the shop short name is shown or hidden based on the configuration.

**Test Result(s):**
  1. Language is set successfully.
  2. Report displays data in the selected language.
  3. Date format, time format, rounding, decimal places, currency sign, and thousands separator are applied correctly.
  4. Footer report code is shown or hidden based on the configuration.
  5. Shop short name is shown or hidden based on the configuration.
  6. No errors displayed.

### 2. On-Demand Routing for Long Date Range Beyond Retention (Monthly Segments)
**Prerequisite(s):**
  1. Ensure the transitional `_legacy.rptdesign` is present and the report is listed for on-demand.
  2. Ensure migration status is completed (formal design is active).

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Set Date type to "Specified as Below" with a range older than the last 3 months (e.g., 2026-01-01 ~ 2026-03-31).
  3. Verify the report engine shows "Background generation required" and lists monthly segments for on-demand generation.
  4. Click "Add to generation queue" to open the On-Demand Job form.
  5. Fill in the email interface, recipients, and format.
  6. Submit the form.
  7. Verify the result page shows the created Preset Parameters id and Scheduled Task ids.
  8. Go to Preset Parameters listing and verify the on-demand preset is displayed (e.g., "On-demand CUS012 2026-01-01 2026-03-31").
  9. Go to Report Management > Scheduled Tasks and verify the one-time BirPreprocessShell tasks are listed for each month segment.
  10. After preprocess and AutoReport complete, verify the emailed report is produced from the formal design reading `pos_bir_*` ∪ temporary tables.
  11. Verify the report data is filtered by `sum_type='r'` and running numbers are resolved from `pos_bir_daily_infos`.

**Test Result(s):**
  1. On-demand routing is triggered for long date range beyond retention.
  2. Monthly segments are listed for on-demand generation.
  3. On-Demand Job form is opened with email interface, recipients, and format fields.
  4. Preset Parameters id and Scheduled Task ids are created successfully.
  5. On-demand preset is displayed in the Preset Parameters listing.
  6. BirPreprocessShell tasks are listed in Scheduled Tasks for each month segment.
  7. Emailed report is produced from the formal design reading `pos_bir_*` ∪ temporary tables (3-table UNION including `pos_bir_station_summaries_temporary`).
  8. Report data is filtered by `sum_type='r'` and running numbers are resolved from `pos_bir_daily_infos`.
  9. No errors displayed.

### 3. Within Retention Renders Directly Without On-Demand
**Prerequisite(s):**
  1. Ensure migration status is completed (formal design is active).
  2. Ensure the selected date range is within retention.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Set Date type to "Specified as Below" with a range within retention (e.g., 2026-09-01 ~ 2026-09-15).
  3. Click "Run" button.
  4. Verify the report renders directly without on-demand routing.
  5. Verify the report reads from `pos_bir_station_summaries` (within retention, no temporary table needed).

**Test Result(s):**
  1. Report renders directly without on-demand routing for date range within retention.
  2. Report reads from `pos_bir_station_summaries` (within retention).
  3. No on-demand job form is shown.
  4. No errors displayed.

### 4. Transitional Legacy Design Before Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is NOT completed (in_progress or not started).

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify the report uses the transitional `_legacy.rptdesign` against `rpt_dyn_*` tables.
  5. Verify the report data is correct and consistent with the legacy behavior.

**Test Result(s):**
  1. Report uses the transitional `_legacy.rptdesign` against `rpt_dyn_*` tables.
  2. Report data is correct and consistent with the legacy behavior.
  3. No errors displayed.

### 5. Formal Design After Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is completed.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify the report uses the formal design against `pos_bir_*` tables (3-table UNION).
  5. Verify the report data is correct and consistent with the formal design.

**Test Result(s):**
  1. Report uses the formal design against `pos_bir_*` tables (3-table UNION).
  2. Report data is correct and consistent with the formal design.
  3. No errors displayed.

### 6. Running Number Accuracy Verification (sum_type='r' Filter)
**Prerequisite(s):**
  1. Ensure migration status is completed.
  2. Ensure `pos_bir_daily_infos` contains running number data for the selected date range.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number) from POS Module > Desktop Format.
  2. Set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify the report displays data filtered by `sum_type='r'`.
  5. Verify start running numbers are correct for each station/outlet.
  6. Verify end running numbers are correct for each station/outlet.
  7. Verify running numbers are resolved from `pos_bir_daily_infos`.
  8. Cross-check the running numbers with the source data.

**Test Result(s):**
  1. Report displays data filtered by `sum_type='r'`.
  2. Start running numbers are correct for each station/outlet.
  3. End running numbers are correct for each station/outlet.
  4. Running numbers are resolved from `pos_bir_daily_infos`.
  5. No errors displayed.

### 7. Export Files to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)
**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number).
  2. Select "Export" from the menu.
  3. Select the desired format (CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)) from the dropdown menu.
  4. Click "OK" button.
  5. Verify the exported file is saved to the specified location.
  6. Repeat for each export format.

**Test Result(s):**
  1. Exported file is successful for each format.
  2. Exported file is saved to the specified location.
  3. Exported file format matches the selected format.
  4. Exported file contains the expected data with correct `dpbb_*` / `dpbp_*` aliases and running numbers.

### 8. Print Report to HTML, PDF
**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number).
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
  1. Open BIR Report (Philippines) (By Running Number).
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
  1. Create an on-demand job for the BIR Report (Philippines) (By Running Number) with a date range beyond retention.
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
  2. Open BIR Report (Philippines) (By Running Number).
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
  1. Open BIR Report (Philippines) (By Running Number) in Google Chrome.
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
  1. Open BIR Report (Philippines) (By Running Number).
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
  1. Ensure the database service is MySQL (this report is MySQL only per release notes).
  2. Ensure the selected date range is less than 7 days.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number).
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
  2. Ensure the selected date range is more than 7 days.

**Step(s):**
  1. Open BIR Report (Philippines) (By Running Number).
  2. Select parameters from Report Data Accuracy Verification and set date range to more than 7 days.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from TiDB.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from TiDB.

## Test Environment

- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
- **Database**: MySQL, TiDB

## QC Information

| Role | Name | Email |
| --- | --- | --- |
| Tester | Mike Lin | mike.lin@shijigroup.com |
| Reviewer | Eric Fang | eric.fang@shijigroup.com |

## Requirements Traceability

| Fix Version | JIRA Ticket |
| --- | --- |
| Report Sprint 20260921-20261002 | HERO-68287 |
| Epic | HERO-68279 |
