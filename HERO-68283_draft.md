# Update Report Module & Engine (Support Routing by Date Range) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Update Report Module & Engine (Support Routing by Date Range) |
| Code | HERO-68283 |
| JIRA Ticket | HERO-68283 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| QC Tester | Mike Lin |
| QC Reviewer | Eric Fang |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Range (Routing Input) | The user-selected date range that drives routing: within last 3 months → direct render; any date older → on-demand flow. | Within Retention (≤3 months), Beyond Retention (>3 months) | Within Retention (≤3 months) |
| Shop (On-Demand Form) | Optional shop filter collected by the On-Demand Job form. | All, Single | All |
| Date Range (On-Demand Form) | Date range collected by the On-Demand Job form; validates overlap; max range 12 months (365 days). | - | - |
| Email Interface (On-Demand Form) | Email interface selection for on-demand report delivery. | - | - |
| Recipients (On-Demand Form) | Email recipients for on-demand report delivery. | - | - |
| Format (On-Demand Form) | Report format for on-demand delivery. | CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx) | PDF |
| Auto-Cleanup On-Demand Jobs | Admin Console setting to soft-delete temporary preset and one-time scheduled tasks after on-demand report is sent. | Yes, No | No |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Routing Decision | The Report Module determines whether to render directly or route to on-demand based on date range. | If entire range within last 3 months → direct; if any date older → on-demand |
| On-Demand Job Form | Form collecting shop (optional), date range, email interface, recipients, format. | - |
| Preset Parameters ID | ID of the created on-demand preset parameter. | - |
| Scheduled Task IDs | IDs of the one-time BirPreprocessShell scheduled tasks for each month segment. | - |
| Monthly Segments | The report engine lists monthly segments for on-demand generation when range exceeds retention. | - |
| Background Generation Required Message | Message shown when on-demand flow is triggered. | - |
| Temporary Tables (Summary) | pos_bir_station_summaries_temporary populated during on-demand flow. | - |
| Temporary Tables (Detail) | pos_bir_check_payments_temporary populated during on-demand flow. | - |
| UNION Query | Shared summary query using UNION of daily, monthly, and temporary summary sources. | pos_bir_station_summaries ∪ pos_bir_station_monthly_summaries ∪ pos_bir_station_summaries_temporary |

## Report Data Accuracy Verification

### 1. Direct Render - Date Range Within Last 3 Months (Retention)
**Prerequisite(s):**
  1. Ensure BIR on-demand-enabled report is available (e.g., POS282 BIR Sales Summary Report).
  2. Ensure BirPreprocessShell has been run for the target date range.
**Step(s):**
  1. Open POS282 BIR Sales Summary Report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range entirely within the last 3 months (e.g., 2026-07-01 ~ 2026-09-30).
  3. Click "Run" button.
  4. Verify the viewer renders directly from daily tables (pos_bir_station_summaries / pos_bir_check_payments).
  5. Verify no on-demand routing is triggered.
**Test Result(s):**
  1. Report renders directly from daily tables.
  2. No on-demand routing is triggered.
  3. Data displayed is correct.
  4. No errors displayed.

### 2. On-Demand Flow - Date Range Beyond Last 3 Months
**Prerequisite(s):**
  1. Ensure BIR on-demand-enabled report is available (e.g., POS282 BIR Sales Summary Report).
  2. Ensure the transitional _legacy.rptdesign is present and the report is listed for on-demand.
**Step(s):**
  1. Open POS282 BIR Sales Summary Report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range older than the last 3 months (e.g., 2026-01-01 ~ 2026-03-31).
  3. Verify the report engine shows "Background generation required" and lists monthly segments for on-demand generation.
  4. Click "Add to generation queue" to open the On-Demand Job form.
  5. Verify the On-Demand Job form collects shop (optional), date range, email interface, and recipients.
  6. Select format (e.g., PDF).
  7. Submit the on-demand job.
  8. Verify the result page shows the created Preset Parameters id and Scheduled Task ids.
  9. Verify Preset Parameters listing shows the on-demand preset (e.g. "On-demand POS282 2026-01-01 2026-03-31").
  10. Verify Report Management > Scheduled Tasks lists the one-time BirPreprocessShell tasks for each month segment.
**Test Result(s):**
  1. Report engine correctly shows "Background generation required" for dates beyond retention.
  2. Monthly segments are listed for on-demand generation.
  3. On-Demand Job form opens with shop (optional), date range, email interface, recipients, and format fields.
  4. Preset Parameters id and Scheduled Task ids are created successfully.
  5. On-demand preset appears in Preset Parameters listing.
  6. BirPreprocessShell tasks are listed in Scheduled Tasks for each month segment.
  7. No errors displayed.

### 3. Full-Range On-Demand Flow (No Range Splitting)
**Prerequisite(s):**
  1. Ensure the Report Module does not split the range for on-demand flow.
**Step(s):**
  1. Open a BIR on-demand-enabled report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range older than 3 months (e.g., 2026-01-01 ~ 2026-06-30).
  3. Trigger the on-demand flow.
  4. Verify the Report Module uses the full-range on-demand flow (does not split the range).
  5. Verify temporary data is written to pos_bir_station_summaries_temporary / pos_bir_check_payments_temporary for the full range.
**Test Result(s):**
  1. Report Module uses the full-range on-demand flow without splitting the range.
  2. Temporary data is written to pos_bir_*_temporary tables for the full range.
  3. No errors displayed.

### 4. Monthly Speed-Up - Reuse Monthly Summaries for Complete Months
**Prerequisite(s):**
  1. Ensure pos_bir_station_monthly_summaries has data for complete months within the range.
**Step(s):**
  1. Open a BIR on-demand-enabled report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a long range spanning multiple calendar months (e.g., 2026-01-01 ~ 2026-06-15).
  3. Trigger the on-demand flow.
  4. Verify the Report Module reuses pos_bir_station_monthly_summaries for complete months (e.g., February, March, April, May).
  5. Verify only incomplete first/last month segments are recomputed (e.g., January 1-31 if complete, June 1-15 incomplete).
**Test Result(s):**
  1. Report Module reuses pos_bir_station_monthly_summaries for complete months.
  2. Only incomplete first/last month segments are recomputed.
  3. No errors displayed.

### 5. On-Demand Job Form - Overlap Validation
**Step(s):**
  1. Open a BIR on-demand-enabled report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range older than 3 months.
  3. Open the On-Demand Job form.
  4. Enter a date range that overlaps with an existing on-demand job.
  5. Submit the form.
  6. Verify the system validates overlap and rejects or warns about the overlapping range.
**Test Result(s):**
  1. System validates overlap for on-demand job date ranges.
  2. Overlapping ranges are rejected or warned about.
  3. No errors displayed.

### 6. On-Demand Job Form - Max Range Validation (12 Months / 365 Days)
**Step(s):**
  1. Open a BIR on-demand-enabled report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range older than 3 months.
  3. Open the On-Demand Job form.
  4. Enter a date range exceeding 12 months (365 days).
  5. Submit the form.
  6. Verify the system rejects the range exceeding the 12-month maximum.
**Test Result(s):**
  1. System enforces max range of 12 months (365 days) for on-demand jobs.
  2. Ranges exceeding 12 months are rejected.
  3. No errors displayed.

### 7. Detail Reports - Max 3 Months Range
**Prerequisite(s):**
  1. Use a detail report (e.g., POS081 BIR Detail Report (Philippines) / bir_detail_ph).
**Step(s):**
  1. Open POS081 BIR Detail Report from POS Module > Desktop Format.
  2. Set Date type to "Specified as below" with a range beyond retention but within 3 months.
  3. Trigger the on-demand flow.
  4. Verify on-demand writes pos_bir_check_payments_temporary for the full range (no monthly segment reuse).
  5. Set a range over 3 months.
  6. Verify the range is rejected.
**Test Result(s):**
  1. Detail reports support on-demand for ranges within 3 months.
  2. On-demand writes pos_bir_check_payments_temporary for the full range without monthly segment reuse.
  3. Ranges over 3 months are rejected for detail reports.
  4. No errors displayed.

### 8. Detail Reports - No Monthly Summaries, No Since-Opening
**Prerequisite(s):**
  1. Use a detail report (e.g., POS081 BIR Detail Report (Philippines) / bir_detail_ph).
**Step(s):**
  1. Open POS081 BIR Detail Report from POS Module > Desktop Format.
  2. Verify the detail report does not use monthly summaries (pos_bir_station_monthly_summaries).
  3. Verify the detail report does not support "since opening".
  4. Verify the detail report reads pos_bir_check_payments (within retention) or pos_bir_check_payments_temporary (on-demand), one row per payment / running number.
**Test Result(s):**
  1. Detail reports do not use monthly summaries.
  2. Detail reports do not support "since opening".
  3. Detail reports read pos_bir_check_payments or pos_bir_check_payments_temporary, one row per payment / running number.
  4. No errors displayed.

### 9. Shared Summary Query - UNION of Daily, Monthly, and Temporary Sources
**Prerequisite(s):**
  1. Ensure pos_bir_station_summaries, pos_bir_station_monthly_summaries, and pos_bir_station_summaries_temporary have test data.
  2. Ensure pos_bir_daily_infos has running numbers and cumulative fields.
**Step(s):**
  1. Open a BIR summary report (e.g., POS282 BIR Sales Summary Report).
  2. Set Date type to "Specified as below" with a range spanning retention and on-demand periods.
  3. Click "Run" button.
  4. Verify the shared summary query uses UNION of:
    4.1. Daily summary source (pos_bir_station_summaries).
    4.2. Monthly summary source (pos_bir_station_monthly_summaries).
    4.3. Temporary summary source (pos_bir_station_summaries_temporary).
  5. Verify the query joins pos_bir_daily_infos for running numbers and cumulative fields.
**Test Result(s):**
  1. Shared summary query correctly uses UNION of daily, monthly, and temporary summary sources.
  2. pos_bir_daily_infos is joined for running numbers and cumulative fields.
  3. No errors displayed.

### 10. "Since Opening" - Summary Only
**Step(s):**
  1. Open a BIR summary report (e.g., POS282 BIR Sales Summary Report).
  2. Set Date type to "Specified as below" with a "since opening" range.
  3. Verify the on-demand flow supports "since opening" for summary reports.
  4. Open a detail report (e.g., POS081 BIR Detail Report).
  5. Verify "since opening" is not available/supported for detail reports.
**Test Result(s):**
  1. "Since opening" is supported for summary reports via on-demand flow.
  2. "Since opening" is not supported for detail reports.
  3. No errors displayed.

## Report Functional Verification

### 1. Verify BIR Report List (Desktop Format) Routing Entry
**Step(s):**
  1. Go to POS Module > Desktop Format.
  2. Verify the BIR report list includes on-demand-enabled reports.
  3. Open an on-demand-enabled BIR report.
  4. Set a date range requiring on-demand.
  5. Verify the viewer routes to the On-Demand job form.
**Test Result(s):**
  1. BIR report list includes on-demand-enabled reports in Desktop Format.
  2. Viewer routes to On-Demand job form when range requires on-demand.
  3. No errors displayed.

### 2. Verify Auto-Cleanup On-Demand Jobs Configuration
**Prerequisite(s):**
  1. Go to Admin Console > System Configuration > Report Module.
**Step(s):**
  1. Set "Auto-cleanup On-Demand jobs" to "Yes".
  2. Create and run an on-demand job.
  3. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks are soft-deleted.
  4. Set "Auto-cleanup On-Demand jobs" to "No".
  5. Create and run another on-demand job.
  6. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks are NOT soft-deleted.
**Test Result(s):**
  1. When "Auto-cleanup On-Demand jobs" = Yes, temporary preset and one-time scheduled tasks are soft-deleted after on-demand report is sent.
  2. When "Auto-cleanup On-Demand jobs" = No, temporary preset and one-time scheduled tasks are NOT soft-deleted.
  3. No errors displayed.

### 3. Verify On-Demand UI Enabled Only When Transitional Legacy Design Is Present
**Prerequisite(s):**
  1. Ensure a report has transitional _legacy.rptdesign present and is listed for on-demand.
  2. Ensure another report does NOT have transitional _legacy.rptdesign.
**Step(s):**
  1. Open the report with transitional _legacy.rptdesign.
  2. Set a date range requiring on-demand.
  3. Verify on-demand UI is enabled.
  4. Open the report without transitional _legacy.rptdesign.
  5. Set a date range requiring on-demand.
  6. Verify on-demand UI is NOT enabled.
**Test Result(s):**
  1. On-demand UI is enabled when transitional legacy design is present and report is listed for on-demand.
  2. On-demand UI is NOT enabled when transitional legacy design is absent.
  3. No errors displayed.

### 4. Verify On-Demand Report Delivery via Email
**Prerequisite(s):**
  1. Create an on-demand job with email interface and recipients.
**Step(s):**
  1. Open a BIR on-demand-enabled report.
  2. Set a date range requiring on-demand.
  3. Open the On-Demand Job form.
  4. Select email interface, enter recipients, select format.
  5. Submit the job.
  6. Wait for preprocess and AutoReport to complete.
  7. Verify the emailed report is produced from the formal design reading pos_bir_* union temporary tables.
**Test Result(s):**
  1. On-demand report is delivered via email successfully.
  2. Emailed report is produced from the formal design reading pos_bir_* union temporary tables.
  3. No errors displayed.

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Step(s):**
  1. Open a BIR on-demand-enabled report in Google Chrome.
  2. Set a date range requiring on-demand.
  3. Verify the on-demand flow works correctly.
  4. Repeat steps 1-3 in Mozilla Firefox.
  5. Repeat steps 1-3 in Microsoft Edge.
**Test Result(s):**
  1. On-demand flow works correctly in Google Chrome.
  2. On-demand flow works correctly in Mozilla Firefox.
  3. On-demand flow works correctly in Microsoft Edge.
  4. No errors displayed.

### 2. Support Config Zone Compatibility
**Step(s):**
  1. Open a BIR on-demand-enabled report at different Config Zone hierarchy levels:
    1.1. Current hierarchy level.
    1.2. Lower hierarchy level.
    1.3. Upper hierarchy level.
    1.4. All hierarchy levels including lower levels.
  2. Set a date range requiring on-demand at each level.
  3. Verify the on-demand flow works correctly at each level.
**Test Result(s):**
  1. On-demand flow works correctly at current hierarchy level.
  2. On-demand flow works correctly at lower hierarchy level.
  3. On-demand flow works correctly at upper hierarchy level.
  4. On-demand flow works correctly at all hierarchy levels including lower levels.
  5. No errors displayed.

### 3. Support Data Service Compatibility - MySQL
**Step(s):**
  1. Open a BIR on-demand-enabled report on a MySQL environment.
  2. Set a date range within retention (less than 7 days).
  3. Verify the report renders directly from MySQL.
  4. Set a date range beyond retention.
  5. Verify the on-demand flow works correctly with MySQL.
**Test Result(s):**
  1. Report renders directly from MySQL for ranges within retention.
  2. On-demand flow works correctly with MySQL.
  3. No errors displayed.

### 4. Support Data Service Compatibility - TiDB
**Step(s):**
  1. Open a BIR on-demand-enabled report on a TiDB environment.
  2. Set a date range within retention (more than 7 days but within 3 months).
  3. Verify the report renders directly from TiDB.
  4. Set a date range beyond retention.
  5. Verify the on-demand flow works correctly with TiDB.
**Test Result(s):**
  1. Report renders directly from TiDB for ranges within retention (more than 7 days).
  2. On-demand flow works correctly with TiDB.
  3. No errors displayed.

### 5. Support Data Service Compatibility - SQL Server (MSSQL)
**Step(s):**
  1. Open a BIR on-demand-enabled report on an MSSQL environment.
  2. Set a date range within retention.
  3. Verify the report renders directly from MSSQL.
  4. Set a date range beyond retention.
  5. Verify the on-demand flow works correctly with MSSQL.
**Test Result(s):**
  1. Report renders directly from MSSQL for ranges within retention.
  2. On-demand flow works correctly with MSSQL.
  3. No errors displayed.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
