# Update BirPreprocessShell (Old/New Write, Daily/Monthly, Cleanup, On-Demand) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Update BirPreprocessShell (Old/New Write, Daily/Monthly, Cleanup, On-Demand) |
| Code | HERO-68282 |
| JIRA Ticket | HERO-68282 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| QC Tester | Mike Lin |
| QC Reviewer | Eric Fang |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| BirPreprocessShell Date Mode | (single-select) Specify date mode for the preprocess shell: current day, last day, custom date, or date range. | Current Day (-m d), Last Day (-m l), Custom (-m c), Date Range (-m r) | Current Day (-m d) |
| Custom Date | Specify business date when Date Mode = Custom (-m c -d YYYY-MM-DD). | - | - |
| Date Range | Specify date range when Date Mode = Date Range (-m r -d YYYY-MM-DD,YYYY-MM-DD). | - | - |
| Migration Status | Determines whether dual-write or new-tables-only write is performed. | Migration In Progress (Dual-Write), Migration Completed (New Tables Only) | Migration In Progress (Dual-Write) |
| Auto-Cleanup On-Demand Jobs | Admin Console setting to soft-delete temporary preset and one-time scheduled tasks after on-demand report is sent. | Yes, No | No |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| pos_bir_check_payments | New standard payment-level table written by BirPreprocessShell alongside existing dynamic payment table during migration. | - |
| pos_bir_station_summaries | New standard station daily summary table written alongside existing dynamic business-day table during migration. | - |
| pos_bir_station_monthly_summaries | New standard monthly summary table aggregated from POS source data at Local daily close. | - |
| pos_bir_station_summaries_temporary | Temporary table populated during on-demand date-range generation. | - |
| pos_bir_check_payments_temporary | Temporary table populated during on-demand date-range generation for detail reports. | - |
| rpt_dyn_pos_bir_* | Legacy dynamic tables written during dual-write (migration in progress); not written after migration completes. | - |
| Daily Retention | Purge daily rows in pos_bir_station_summaries / pos_bir_check_payments older than 3 months. | - |
| Monthly/Daily-Info Retention | Apply 10-year policy on long-term tables (pos_bir_station_monthly_summaries / pos_bir_daily_infos). | - |

## Report Data Accuracy Verification

### 1. Verify Dual-Write During Migration (Old + New Tables)
**Prerequisite(s):**
  1. Ensure migration status is in progress.
  2. Ensure BirPreprocessShell is invoked from Local daily close via DailyPreprocessShell.
**Step(s):**
  1. Trigger BirPreprocessShell from Local daily close (DailyPreprocessShell).
  2. Verify BirPreprocessShell performs dual-write:
    2.1. Writes legacy rpt_dyn_pos_bir_* tables (dynamic payment table, dynamic business-day table).
    2.2. Writes new pos_bir_* tables (pos_bir_check_payments, pos_bir_station_summaries).
  3. Verify payment-level preprocess writes pos_bir_check_payments alongside existing dynamic payment table.
  4. Verify station daily summary writes pos_bir_station_summaries alongside existing dynamic business-day table.
  5. Verify monthly summary aggregates into pos_bir_station_monthly_summaries at Local daily close.
**Test Result(s):**
  1. BirPreprocessShell performs dual-write during migration.
  2. Legacy rpt_dyn_pos_bir_* tables are written.
  3. New pos_bir_* tables (pos_bir_check_payments, pos_bir_station_summaries) are written.
  4. pos_bir_station_monthly_summaries is aggregated at Local daily close.
  5. No errors displayed.

### 2. Verify New-Tables-Only Write After Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is completed.
**Step(s):**
  1. Trigger BirPreprocessShell from Local daily close (DailyPreprocessShell).
  2. Verify BirPreprocessShell writes new standard tables only:
    2.1. Writes pos_bir_check_payments (payment-level preprocess).
    2.2. Writes pos_bir_station_summaries (station daily summary).
    2.3. Aggregates into pos_bir_station_monthly_summaries (monthly summary).
  3. Verify legacy rpt_dyn_pos_bir_* tables are NOT written.
**Test Result(s):**
  1. BirPreprocessShell writes new standard tables only after migration completes.
  2. pos_bir_check_payments is written.
  3. pos_bir_station_summaries is written.
  4. pos_bir_station_monthly_summaries is aggregated.
  5. Legacy rpt_dyn_pos_bir_* tables are NOT written.
  6. No errors displayed.

### 3. Verify Payment-Level Preprocess (pos_bir_check_payments)
**Prerequisite(s):**
  1. Ensure POS check payment data exists for the target business date.
**Step(s):**
  1. Trigger BirPreprocessShell with date mode = current day (-m d).
  2. Verify payment-level preprocess writes pos_bir_check_payments.
  3. Verify each row in pos_bir_check_payments corresponds to one payment / running number.
  4. Verify the data matches POS check payment source data.
**Test Result(s):**
  1. Payment-level preprocess writes pos_bir_check_payments successfully.
  2. Each row corresponds to one payment / running number.
  3. Data matches POS check payment source data.
  4. No errors displayed.

### 4. Verify Station Daily Summary (pos_bir_station_summaries)
**Prerequisite(s):**
  1. Ensure POS station data exists for the target business date.
**Step(s):**
  1. Trigger BirPreprocessShell with date mode = current day (-m d).
  2. Verify station daily summary writes pos_bir_station_summaries.
  3. Verify the summary data is correctly aggregated from POS source data.
  4. Verify station display names can be resolved from POS master data at report runtime.
**Test Result(s):**
  1. Station daily summary writes pos_bir_station_summaries successfully.
  2. Summary data is correctly aggregated from POS source data.
  3. Station display names can be resolved from POS master data at runtime.
  4. No errors displayed.

### 5. Verify Monthly Summary (pos_bir_station_monthly_summaries)
**Prerequisite(s):**
  1. Ensure POS source data exists for the target month.
  2. Ensure BirPreprocessShell is invoked at Local daily close.
**Step(s):**
  1. Trigger BirPreprocessShell at Local daily close.
  2. Verify monthly summary aggregates from POS source data into pos_bir_station_monthly_summaries.
  3. Verify the monthly summary is correctly computed for the complete month.
  4. Verify monthly summaries can be reused for long-range report queries.
**Test Result(s):**
  1. Monthly summary aggregates into pos_bir_station_monthly_summaries at Local daily close.
  2. Monthly summary is correctly computed for the complete month.
  3. Monthly summaries can be reused for long-range report queries.
  4. No errors displayed.

### 6. Verify Expired-Data Purge (3-Month Daily Retention)
**Prerequisite(s):**
  1. Ensure pos_bir_station_summaries and pos_bir_check_payments have rows older than 3 months.
**Step(s):**
  1. Trigger BirPreprocessShell purge process.
  2. Verify daily rows in pos_bir_station_summaries older than 3 months are purged.
  3. Verify daily rows in pos_bir_check_payments older than 3 months are purged.
  4. Verify rows within the 3-month retention period are NOT purged.
**Test Result(s):**
  1. Daily rows in pos_bir_station_summaries older than 3 months are purged.
  2. Daily rows in pos_bir_check_payments older than 3 months are purged.
  3. Rows within the 3-month retention period are NOT purged.
  4. No errors displayed.

### 7. Verify Long-Term Retention (10-Year Policy)
**Prerequisite(s):**
  1. Ensure pos_bir_station_monthly_summaries and pos_bir_daily_infos have rows older than 10 years.
**Step(s):**
  1. Trigger BirPreprocessShell purge process.
  2. Verify 10-year retention policy is applied on pos_bir_station_monthly_summaries.
  3. Verify 10-year retention policy is applied on pos_bir_daily_infos.
  4. Verify rows within the 10-year retention period are NOT purged.
**Test Result(s):**
  1. 10-year retention policy is applied on pos_bir_station_monthly_summaries.
  2. 10-year retention policy is applied on pos_bir_daily_infos.
  3. Rows within the 10-year retention period are NOT purged.
  4. No errors displayed.

### 8. Verify Temporary Table Cleanup / DROP
**Prerequisite(s):**
  1. Ensure pos_bir_station_summaries_temporary and pos_bir_check_payments_temporary have data from completed on-demand jobs.
**Step(s):**
  1. Trigger temporary table cleanup process.
  2. Verify pos_bir_station_summaries_temporary is cleaned up or dropped when empty.
  3. Verify pos_bir_check_payments_temporary is cleaned up or dropped when empty.
  4. Verify temporary tables with active on-demand jobs are NOT dropped.
**Test Result(s):**
  1. pos_bir_station_summaries_temporary is cleaned up or dropped when empty.
  2. pos_bir_check_payments_temporary is cleaned up or dropped when empty.
  3. Temporary tables with active on-demand jobs are NOT dropped.
  4. No errors displayed.

### 9. Verify On-Demand Date-Range Generation (pos_bir_station_summaries_temporary)
**Prerequisite(s):**
  1. Ensure an on-demand job is created for a BIR summary report with a date range beyond retention.
**Step(s):**
  1. Trigger the on-demand BirPreprocessShell for the specified date range.
  2. Verify pos_bir_station_summaries_temporary is populated for the on-demand date range.
  3. Verify the temporary data is correctly generated from POS source data.
  4. Verify the temporary data can be read by the formal rptdesign in the 3-table UNION.
**Test Result(s):**
  1. On-demand date-range generation populates pos_bir_station_summaries_temporary.
  2. Temporary data is correctly generated from POS source data.
  3. Temporary data can be read by the formal rptdesign in the 3-table UNION.
  4. No errors displayed.

### 10. Verify On-Demand Date-Range Generation (pos_bir_check_payments_temporary)
**Prerequisite(s):**
  1. Ensure an on-demand job is created for a BIR detail report with a date range beyond retention but within 3 months.
**Step(s):**
  1. Trigger the on-demand BirPreprocessShell for the specified date range.
  2. Verify pos_bir_check_payments_temporary is populated for the on-demand date range.
  3. Verify each row corresponds to one payment / running number.
  4. Verify the temporary data can be read by the formal rptdesign.
**Test Result(s):**
  1. On-demand date-range generation populates pos_bir_check_payments_temporary.
  2. Each row corresponds to one payment / running number.
  3. Temporary data can be read by the formal rptdesign.
  4. No errors displayed.

### 11. Verify Post-Migration HQ/LS Scheduled Preprocess (Cleanup Only)
**Prerequisite(s):**
  1. Ensure migration status is completed.
**Step(s):**
  1. Trigger the HQ/LS scheduled preprocess task.
  2. Verify the HQ/LS scheduled preprocess performs cleanup only.
  3. Verify no HQ daily generation is performed.
  4. Verify no backfill of missing days/months is performed.
**Test Result(s):**
  1. HQ/LS scheduled preprocess performs cleanup only after migration.
  2. No HQ daily generation is performed.
  3. No backfill of missing days/months is performed.
  4. No errors displayed.

### 12. Verify BirPreprocessShell Date Modes
**Step(s):**
  1. Run BirPreprocessShell with date mode = current day (-m d).
  2. Verify preprocess runs for the current business day.
  3. Run BirPreprocessShell with date mode = last day (-m l).
  4. Verify preprocess runs for the last business day.
  5. Run BirPreprocessShell with date mode = custom (-m c -d 2026-09-01).
  6. Verify preprocess runs for the specified custom date.
  7. Run BirPreprocessShell with date mode = date range (-m r -d 2026-09-01,2026-09-30).
  8. Verify preprocess runs for the specified date range.
**Test Result(s):**
  1. BirPreprocessShell runs correctly for current day mode (-m d).
  2. BirPreprocessShell runs correctly for last day mode (-m l).
  3. BirPreprocessShell runs correctly for custom date mode (-m c -d).
  4. BirPreprocessShell runs correctly for date range mode (-m r -d).
  5. No errors displayed.

## Report Functional Verification

### 1. Verify BirPreprocessShell Invocation from Local Daily Close
**Prerequisite(s):**
  1. Ensure Local daily close process triggers DailyPreprocessShell.
**Step(s):**
  1. Perform Local daily close.
  2. Verify DailyPreprocessShell invokes BirPreprocessShell.
  3. Verify BirPreprocessShell runs successfully.
  4. Verify new standard tables are populated.
**Test Result(s):**
  1. Local daily close triggers DailyPreprocessShell.
  2. DailyPreprocessShell invokes BirPreprocessShell.
  3. BirPreprocessShell runs successfully.
  4. New standard tables are populated.
  5. No errors displayed.

### 2. Verify BirPreprocessShell Scheduled Task in Report Management
**Step(s):**
  1. Go to Platform > Report Management > Scheduled Tasks.
  2. Verify BirPreprocessShell is listed as a scheduled task.
  3. Verify the scheduled task can be manually triggered.
  4. Verify the scheduled task runs successfully when triggered.
**Test Result(s):**
  1. BirPreprocessShell is listed in Report Management > Scheduled Tasks.
  2. Scheduled task can be manually triggered.
  3. Scheduled task runs successfully when triggered.
  4. No errors displayed.

### 3. Verify On-Demand BirPreprocessShell Tasks
**Prerequisite(s):**
  1. Create an on-demand job for a BIR report with a date range beyond retention.
**Step(s):**
  1. Go to Report Management > Scheduled Tasks.
  2. Verify one-time BirPreprocessShell tasks are listed for each month segment of the on-demand job.
  3. Verify the task names indicate "BIR on-demand" and the date range.
  4. Verify the tasks run successfully and populate temporary tables.
**Test Result(s):**
  1. One-time BirPreprocessShell tasks are listed for each month segment.
  2. Task names indicate "BIR on-demand" and the date range.
  3. Tasks run successfully and populate temporary tables.
  4. No errors displayed.

### 4. Verify Auto-Cleanup On-Demand Jobs Configuration
**Prerequisite(s):**
  1. Go to Admin Console > System Configuration > Report Module.
**Step(s):**
  1. Set "Auto-cleanup On-Demand jobs" to "Yes".
  2. Create and run an on-demand job.
  3. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks (BirPreprocessShell) are soft-deleted.
  4. Set "Auto-cleanup On-Demand jobs" to "No".
  5. Create and run another on-demand job.
  6. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks are NOT soft-deleted.
**Test Result(s):**
  1. When "Auto-cleanup On-Demand jobs" = Yes, temporary preset and one-time BirPreprocessShell scheduled tasks are soft-deleted after on-demand report is sent.
  2. When "Auto-cleanup On-Demand jobs" = No, temporary preset and one-time BirPreprocessShell scheduled tasks are NOT soft-deleted.
  3. No errors displayed.

### 5. Verify On-Demand Job Result Page
**Prerequisite(s):**
  1. Create and submit an on-demand job for a BIR report.
**Step(s):**
  1. Open POS282 BIR Sales Summary Report.
  2. Set Date type to "Specified as below" with a range older than 3 months (e.g., 2026-01-01 ~ 2026-03-31).
  3. Click "Add to generation queue".
  4. Fill in the On-Demand Job form and submit.
  5. Verify the result page shows the created Preset Parameters id.
  6. Verify the result page shows the created Scheduled Task ids.
  7. Verify Preset Parameters listing shows the on-demand preset (e.g. "On-demand POS282 2026-01-01 2026-03-31").
  8. Verify Report Management > Scheduled Tasks lists the one-time BirPreprocessShell tasks for each month segment.
**Test Result(s):**
  1. Result page shows the created Preset Parameters id.
  2. Result page shows the created Scheduled Task ids.
  3. Preset Parameters listing shows the on-demand preset.
  4. Scheduled Tasks lists one-time BirPreprocessShell tasks for each month segment.
  5. No errors displayed.

## Report Compatibility Verification

### 1. Support Data Service Compatibility - MySQL
**Step(s):**
  1. Run BirPreprocessShell on a MySQL environment.
  2. Verify BirPreprocessShell writes to pos_bir_* tables on MySQL.
  3. Verify purge process works correctly on MySQL.
  4. Verify on-demand temporary table generation works on MySQL.
**Test Result(s):**
  1. BirPreprocessShell writes to pos_bir_* tables on MySQL.
  2. Purge process works correctly on MySQL.
  3. On-demand temporary table generation works on MySQL.
  4. No errors displayed.

### 2. Support Data Service Compatibility - TiDB
**Step(s):**
  1. Run BirPreprocessShell on a TiDB environment.
  2. Verify BirPreprocessShell writes to pos_bir_* tables on TiDB.
  3. Verify purge process works correctly on TiDB.
  4. Verify on-demand temporary table generation works on TiDB.
**Test Result(s):**
  1. BirPreprocessShell writes to pos_bir_* tables on TiDB.
  2. Purge process works correctly on TiDB.
  3. On-demand temporary table generation works on TiDB.
  4. No errors displayed.

### 3. Support Data Service Compatibility - SQL Server (MSSQL)
**Step(s):**
  1. Run BirPreprocessShell on an MSSQL environment.
  2. Verify BirPreprocessShell writes to pos_bir_* tables on MSSQL.
  3. Verify purge process works correctly on MSSQL.
  4. Verify on-demand temporary table generation works on MSSQL.
**Test Result(s):**
  1. BirPreprocessShell writes to pos_bir_* tables on MSSQL.
  2. Purge process works correctly on MSSQL.
  3. On-demand temporary table generation works on MSSQL.
  4. No errors displayed.

### 4. Support Config Zone Compatibility
**Step(s):**
  1. Run BirPreprocessShell at different Config Zone hierarchy levels:
    1.1. Current hierarchy level.
    1.2. Lower hierarchy level.
    1.3. Upper hierarchy level.
    1.4. All hierarchy levels including lower levels.
  2. Verify BirPreprocessShell processes data correctly at each level.
  3. Verify the data hierarchy is respected (current, lower, upper, all including lower).
**Test Result(s):**
  1. BirPreprocessShell processes data correctly at current hierarchy level.
  2. BirPreprocessShell processes data correctly at lower hierarchy level.
  3. BirPreprocessShell processes data correctly at upper hierarchy level.
  4. BirPreprocessShell processes data correctly at all hierarchy levels including lower levels.
  5. No errors displayed.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
