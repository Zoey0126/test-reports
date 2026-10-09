# BirPreprocessShell Data Migration and Old Task Cleanup Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | BirPreprocessShell Data Migration and Old Task Cleanup |
| Code | Action 6 |
| JIRA Ticket | HERO-68290 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| Component | BirPreprocessShell |
| Database | MySQL & MSSQL |

## Issue Summary

Update `BirPreprocessShell` to support historical data migration from old dynamic BIR tables to new standard tables, plus cleanup of legacy scheduled jobs (Epic HERO-68279 Action 6).

### Scope
- Migration status file (`bir_migration.txt`) tracks `in_progress` / `completed` and last migrated date for resume; missing status file starts migration on the next run.
- Migrate one calendar month per scheduled run: `rpt_dyn_pos_bir_business_days` → `pos_bir_station_summaries`; `rpt_dyn_pos_bir_check_payments` → `pos_bir_check_payments`; build `pos_bir_station_monthly_summaries` for migrated months.
- During `in_progress`: dual-write legacy and standard tables; when status is `completed`: standard tables only (ensure all historical months have monthly summaries before marking completed).
- When migration completed, report list automatically routes six summary BIR reports to the new rptdesign (`bir_ph`, `bir_sales_summary`, `bir_revenue_slip`, `bir_ph_by_running_number`, `bir_sfe_ph`, `bir_sfe_detail_ph`).
- After cutover, HQ scheduled preprocess no longer fills missing month/day; it runs migration (if still needed) and expired-data cleanup only. Local daily close generates day-to-day BIR data and syncs to HQ.

### Key Changes
- **Migration Status File**: JSON status file tracks `in_progress` / `completed` and last migrated date for resume.
- **Monthly Migration**: Migrate one calendar month per scheduled run from legacy dynamic tables to standard tables.
- **Dual-Write During Migration**: Legacy and standard tables are written simultaneously during `in_progress` status.
- **Automatic Report Routing**: After migration completes, six summary BIR reports are automatically routed to the new rptdesign.
- **HQ Preprocess Simplification**: After cutover, HQ scheduled preprocess runs migration (if needed) and cleanup only; no more fill-missing-month / fill-missing-day.
- **No Special UI Configuration**: Migration is driven by `BirPreprocessShell` scheduled runs and the `bir_migration.txt` status file.
- **No Field Definition Changes**: This case does not involve field definition.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Migration Status | (system-managed) The status of the BIR data migration, tracked in the `bir_migration.txt` status file. | in_progress, completed | in_progress |
| Auto-cleanup On-Demand Jobs | (Yes/No) Soft-deletes the temporary preset and related one-time scheduled tasks after the on-demand report is sent. Settings: Go to Admin Console > System Configuration > Report Module | Yes, No | No |
| HQ Scheduled Preprocess Mode | (system-managed) After cutover, the HQ scheduled preprocess runs migration (if needed) and cleanup only; no more fill-missing-month / fill-missing-day. | Migration + Cleanup, Fill-Missing + Cleanup | Fill-Missing + Cleanup |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Migration Status | The current status of the BIR data migration (`in_progress` or `completed`), read from `bir_migration.txt`. | - |
| Last Migrated Date | The last migrated calendar month date, read from `bir_migration.txt`. | - |
| Migrated Data (pos_bir_station_summaries) | Migrated station summary data from `rpt_dyn_pos_bir_business_days`. | Migrated from `rpt_dyn_pos_bir_business_days` |
| Migrated Data (pos_bir_check_payments) | Migrated check payment data from `rpt_dyn_pos_bir_check_payments`. | Migrated from `rpt_dyn_pos_bir_check_payments` |
| Monthly Summaries (pos_bir_station_monthly_summaries) | Built monthly summaries for migrated months. | Built from `pos_bir_station_summaries` |
| Report Routing | After migration completes, the report list automatically routes six summary BIR reports to the new rptdesign. | - |
| HQ Scheduled Task | After cutover, HQ scheduled preprocess runs cleanup only (no fill-missing-month / fill-missing-day). | - |

## Report Data Accuracy Verification

### 1. All Default Parameters — Migration In Progress (Initial State)
**Prerequisite(s):**
  1. Ensure the `bir_migration.txt` status file does not exist or is set to `in_progress`.
  2. Ensure legacy dynamic tables (`rpt_dyn_pos_bir_business_days`, `rpt_dyn_pos_bir_check_payments`) contain historical data.
  3. Go to Platform: System Management > System Configuration > Report > Format Configuration.

**Step(s):**
  1. Trigger the `BirPreprocessShell` scheduled run.
  2. Verify the migration starts and the `bir_migration.txt` status file is created with status `in_progress`.
  3. Verify one calendar month of data is migrated:
    3.1. `rpt_dyn_pos_bir_business_days` → `pos_bir_station_summaries`.
    3.2. `rpt_dyn_pos_bir_check_payments` → `pos_bir_check_payments`.
    3.3. `pos_bir_station_monthly_summaries` is built for the migrated month.
  4. Verify the last migrated date is updated in `bir_migration.txt`.
  5. Verify dual-write is active (both legacy and standard tables are written).

**Test Result(s):**
  1. Migration starts successfully and `bir_migration.txt` is created with status `in_progress`.
  2. One calendar month of data is migrated to standard tables.
  3. Monthly summaries are built for the migrated month.
  4. Last migrated date is updated.
  5. Dual-write is active during `in_progress` status.
  6. No errors displayed.

### 2. Migration Resume After Interruption
**Prerequisite(s):**
  1. Ensure the `bir_migration.txt` status file exists with status `in_progress` and a last migrated date.
  2. Ensure migration was interrupted before completion.

**Step(s):**
  1. Trigger the `BirPreprocessShell` scheduled run.
  2. Verify the migration resumes from the last migrated date.
  3. Verify the next calendar month of data is migrated.
  4. Verify the last migrated date is updated in `bir_migration.txt`.
  5. Repeat until all historical months are migrated.

**Test Result(s):**
  1. Migration resumes from the last migrated date.
  2. The next calendar month of data is migrated.
  3. Last migrated date is updated.
  4. No errors displayed.

### 3. Migration Completion
**Prerequisite(s):**
  1. Ensure all historical months have been migrated.
  2. Ensure all historical months have monthly summaries.

**Step(s):**
  1. Trigger the `BirPreprocessShell` scheduled run.
  2. Verify the migration status is changed to `completed` in `bir_migration.txt`.
  3. Verify all historical months have monthly summaries before marking completed.
  4. Verify dual-write is disabled (standard tables only).
  5. Verify the report list automatically routes six summary BIR reports to the new rptdesign:
    5.1. `bir_ph`
    5.2. `bir_sales_summary`
    5.3. `bir_revenue_slip`
    5.4. `bir_ph_by_running_number`
    5.5. `bir_sfe_ph`
    5.6. `bir_sfe_detail_ph`

**Test Result(s):**
  1. Migration status is changed to `completed`.
  2. All historical months have monthly summaries.
  3. Dual-write is disabled (standard tables only).
  4. Six summary BIR reports are automatically routed to the new rptdesign.
  5. No errors displayed.

### 4. Missing Status File Starts Migration
**Prerequisite(s):**
  1. Ensure the `bir_migration.txt` status file does not exist.

**Step(s):**
  1. Trigger the `BirPreprocessShell` scheduled run.
  2. Verify the migration starts on the next run (missing status file starts migration).
  3. Verify the `bir_migration.txt` status file is created with status `in_progress`.

**Test Result(s):**
  1. Missing status file starts migration on the next run.
  2. `bir_migration.txt` is created with status `in_progress`.
  3. No errors displayed.

## Report Functional Verification

### 1. On-Demand Job Creation with Migration Infrastructure
**Prerequisite(s):**
  1. Ensure migration/on-demand infrastructure is in place.
  2. Ensure migration status is `completed`.

**Step(s):**
  1. Open POS282 BIR Sales Summary Report from POS Module > Desktop Format.
  2. Set Date type to "Specified as Below" with a range older than the last 3 months (e.g., 2026-01-01 ~ 2026-03-31).
  3. Verify the report engine shows "Background generation required" and lists monthly segments for on-demand generation.
  4. Click "Add to generation queue" to open the On-Demand Job form (report, date range, email interface, recipients, format).
  5. Submit the form.
  6. Verify the job is queued and the result page shows the created Preset Parameters id and Scheduled Task ids.
  7. Go to Preset Parameters listing and verify the on-demand preset is displayed (e.g., "On-demand POS282 2026-01-01 2026-03-31").
  8. Go to Report Management > Scheduled Tasks and verify the one-time BirPreprocessShell tasks are listed for each month segment.

**Test Result(s):**
  1. On-demand routing is triggered for date range older than 3 months.
  2. On-Demand Job form is opened with report, date range, email interface, recipients, and format fields.
  3. Preset Parameters id and Scheduled Task ids are created successfully.
  4. On-demand preset is displayed in the Preset Parameters listing.
  5. BirPreprocessShell tasks are listed in Scheduled Tasks for each month segment.
  6. No errors displayed.

### 2. HQ Scheduled Preprocess After Cutover (Cleanup Only)
**Prerequisite(s):**
  1. Ensure migration status is `completed`.
  2. Ensure cutover has occurred.

**Step(s):**
  1. Trigger the HQ scheduled preprocess task.
  2. Verify the HQ scheduled task performs cleanup only (no fill-missing-month / fill-missing-day).
  3. Verify expired-data cleanup is performed.
  4. Verify no fill-missing-month or fill-missing-day operations are executed.

**Test Result(s):**
  1. HQ scheduled task performs cleanup only after cutover.
  2. No fill-missing-month / fill-missing-day operations are executed.
  3. Expired-data cleanup is performed.
  4. No errors displayed.

### 3. HQ Scheduled Preprocess Before Cutover (Migration + Cleanup)
**Prerequisite(s):**
  1. Ensure migration status is `in_progress` (not yet completed).

**Step(s):**
  1. Trigger the HQ scheduled preprocess task.
  2. Verify the HQ scheduled task runs migration (if still needed) and cleanup.
  3. Verify one calendar month of data is migrated per scheduled run.
  4. Verify expired-data cleanup is performed.

**Test Result(s):**
  1. HQ scheduled task runs migration and cleanup before cutover.
  2. One calendar month of data is migrated per scheduled run.
  3. Expired-data cleanup is performed.
  4. No errors displayed.

### 4. Local Daily Close Generates BIR Data and Syncs to HQ
**Prerequisite(s):**
  1. Ensure migration status is `completed`.
  2. Ensure local daily close is configured.

**Step(s):**
  1. Perform local daily close at a POS terminal.
  2. Verify the day-to-day BIR data is generated in standard tables.
  3. Verify the BIR data is synced to HQ.
  4. Verify the HQ receives the synced BIR data.

**Test Result(s):**
  1. Local daily close generates day-to-day BIR data in standard tables.
  2. BIR data is synced to HQ.
  3. HQ receives the synced BIR data.
  4. No errors displayed.

### 5. Auto-Cleanup On-Demand Jobs
**Prerequisite(s):**
  1. Go to Admin Console > System Configuration > Report Module.
  2. Set "Auto-cleanup On-Demand jobs" to "Yes".

**Step(s):**
  1. Create an on-demand job for a BIR report with a date range older than 3 months.
  2. Wait for the on-demand report to be sent.
  3. Verify the temporary preset and related one-time scheduled tasks are soft-deleted after the on-demand report is sent.
  4. Set "Auto-cleanup On-Demand jobs" to "No" and repeat the process.
  5. Verify the temporary preset and related one-time scheduled tasks are NOT soft-deleted.

**Test Result(s):**
  1. When "Auto-cleanup On-Demand jobs" is "Yes", the temporary preset and related one-time scheduled tasks are soft-deleted after the on-demand report is sent.
  2. When "Auto-cleanup On-Demand jobs" is "No", the temporary preset and related one-time scheduled tasks are NOT soft-deleted.
  3. No errors displayed.

### 6. Automatic Report Routing After Migration Completes
**Prerequisite(s):**
  1. Ensure migration status is `completed`.

**Step(s):**
  1. Open each of the six summary BIR reports from POS Module > Desktop Format:
    1.1. `bir_ph` (POS060 BIR Report Philippines)
    1.2. `bir_sales_summary` (POS282 BIR Sales Summary Report)
    1.3. `bir_revenue_slip` (POS063 Revenue Report Philippines)
    1.4. `bir_ph_by_running_number` (CUS012 BIR Report Philippines By Running Number)
    1.5. `bir_sfe_ph` (CUS313 BIR-EIS Sales Summary Report)
    1.6. `bir_sfe_detail_ph` (CUS314 BIR-EIS BIR Detail Report Philippines)
  2. For each report, set Date type to "Current Business Day".
  3. Click "Run" button.
  4. Verify each report uses the new rptdesign (formal design reading `pos_bir_*` tables).

**Test Result(s):**
  1. All six summary BIR reports are automatically routed to the new rptdesign after migration completes.
  2. Each report uses the formal design reading `pos_bir_*` tables.
  3. No errors displayed.

### 7. Multi-Language Support for Report Parameter and Data
**Step(s):**
  1. Select "Language" from the menu.
  2. Open a BIR report (e.g., POS282 BIR Sales Summary Report).
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
  2. Ensure migration status is `completed`.

**Step(s):**
  1. Open a BIR report (e.g., POS282 BIR Sales Summary Report) in Google Chrome.
  2. Select "default" from all parameters.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Repeat steps 1-4 in Mozilla Firefox and Microsoft Edge.

**Test Result(s):**
  1. Report loads successfully in all three browsers.
  2. No errors displayed in any browser.

### 2. Support Config Zone Compatibility
**Step(s):**
  1. Open a BIR report (e.g., POS282 BIR Sales Summary Report).
  2. Select parameters for each Config Zone hierarchy level:
    2.1. Current hierarchy level.
    2.2. Lower hierarchy level.
    2.3. Upper hierarchy level.
    2.4. All hierarchy levels including lower levels.
  3. Verify the report loads successfully for each level.

**Test Result(s):**
  1. Report loads successfully for each Config Zone hierarchy level.
  2. Data displayed is correct for each hierarchy level.
  3. No errors displayed.

### 3. Support Data Service Compatibility - MySQL
**Prerequisite(s):**
  1. Ensure the database service is MySQL.
  2. Ensure migration status is `completed`.

**Step(s):**
  1. Open a BIR report (e.g., POS282 BIR Sales Summary Report).
  2. Set date range to less than 7 days.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from MySQL.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data is retrieved from MySQL.

### 4. Support Data Service Compatibility - TiDB
**Prerequisite(s):**
  1. Ensure the database service supports both MySQL and TiDB.
  2. Ensure migration status is `completed`.

**Step(s):**
  1. Open a BIR report (e.g., POS282 BIR Sales Summary Report).
  2. Set date range to more than 7 days.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from TiDB.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data is retrieved from TiDB.

### 5. Support Data Service Compatibility - MSSQL
**Prerequisite(s):**
  1. Ensure the database service is MSSQL (SQL Server).
  2. Ensure migration status is `completed`.

**Step(s):**
  1. Open a BIR report (e.g., POS282 BIR Sales Summary Report).
  2. Select default parameters.
  3. Click "Run" button.
  4. Verify the report loads successfully.
  5. Verify data is retrieved from MSSQL.

**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data is retrieved from MSSQL.

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
| Report Sprint 20260921-20261002 | HERO-68290 |
| Epic | HERO-68279 |
