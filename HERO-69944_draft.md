# HERO-69944 Test Report - SDP Interface AlogLogInfo Export Stops on Orphan Info Row

## Background

During SDP export, when processing the `alog_log_infos` batch, if any row cannot find its parent `alog_logs` record, the whole table export of that run fails and stops. The data process log is not updated (`lastRecId`, `lastRecModTime`), so the next run starts from the same rows and fails again.

Expected behaviour: rows with a missing parent audit log should be skipped so that the rest of the batch can be processed and the bookmark can advance.

## Functional Testing

### 1. Bug Reproduction and Fix Verification - Orphan AlogLogInfo Row Handling

#### 1.1. Verify Orphan Row Is Skipped and Remaining Batch Records Are Processed

**Prerequisite(s):**
1. The background job "HERO SDP Data Export Shell" is turned on
2. The existing SDP data export flow is configured and the `alog_log_infos` table is included in the export
3. Tester can create orphan `alog_log_infos` rows (a row whose `ainf_alog_id` has no matching `alog_id` in `alog_logs`) in the next export window

**Step(s):**
1. Create one orphan `alog_log_infos` row (e.g., `ainf_alog_id = XXX` while `alog_logs` has no row with `alog_id = XXX`) together with normal rows that have valid parent `alog_logs` records, so that all of them fall into the next export window
2. Run the HERO SDP Data Export Shell job (or wait for its scheduled execution) so the batch containing the orphan row is processed
3. Check the export result of the batch containing the orphan row
4. Check the new or updated records on the SDP platform, or in the local SDP log

**Reproduction Result(s):**
1. Before the fix, when the batch reaches the orphan row, the whole table export of that run fails and stops
2. Nothing in the batch is sent, including the normal rows that come after the orphan row

**Fix Result(s):**
1. The orphan row with the missing parent audit log is skipped
2. The remaining records in the batch continue to be processed and are sent successfully
3. The new or updated records can be verified on the SDP platform, or in the local SDP log

#### 1.2. Verify Export Bookmark Advances After a Batch Containing an Orphan Row

**Prerequisite(s):**
1. The background job "HERO SDP Data Export Shell" is turned on
2. An orphan `alog_log_infos` row exists within the export window (same test data as 1.1)
3. Tester can query the data process log of the SDP export for `lastRecId` and `lastRecModTime`

**Step(s):**
1. Record the current `lastRecId` and `lastRecModTime` of the `alog_log_infos` export in the data process log
2. Run the HERO SDP Data Export Shell job so the batch containing the orphan row is processed
3. Check `lastRecId` and `lastRecModTime` in the data process log after the run
4. Trigger the next export run and check which rows are loaded and whether the run completes

**Reproduction Result(s):**
1. Before the fix, the data process log is not updated: `lastRecId` and `lastRecModTime` remain the same after the failed run
2. The next run loads the same rows again and fails on the same orphan row again, so the export never advances

**Fix Result(s):**
1. `lastRecId` and `lastRecModTime` are updated after the run that skipped the orphan row
2. The next run starts from the new bookmark instead of reloading the same rows
3. The next run completes without failing on the previously orphaned row

#### 1.3. Verify Orphan Row Skip Is Recorded in sdp_error.log

**Prerequisite(s):**
1. The background job "HERO SDP Data Export Shell" is turned on
2. Orphan `alog_log_infos` rows are created in the next export window for testing
3. Tester has access to the local SDP log directory `PATHTOHERO\account\{ACOUNT}\tmp\logs\sdp`

**Step(s):**
1. Run the HERO SDP Data Export Shell job so the batch containing the orphan row is processed
2. Navigate to `PATHTOHERO\account\{ACOUNT}\tmp\logs\sdp`
3. Confirm each newly configured table gets its own `sdp_full_log_per_table_XXXXX.log` file
4. Open `sdp_error.log` and look for the message about the orphan `alog_log_infos` row

**Reproduction Result(s):**
1. Before the fix, the export run fails and stops at the orphan row, and no skip-and-continue message is recorded for it

**Fix Result(s):**
1. `sdp_error.log` contains the message indicating that the orphan row is skipped
2. The message shows that the export continues to process the remaining records in the batch
3. Each newly configured table has its own `sdp_full_log_per_table_XXXXX.log` file

### 2. SDP Data Export Normal Flow - Regression After Fix

#### 2.1. Verify Batch Export Completes When No Orphan Row Exists

**Prerequisite(s):**
1. The background job "HERO SDP Data Export Shell" is turned on
2. The `alog_log_infos` rows modified since the last export all have valid parent `alog_logs` records (no orphan row in the export window)

**Step(s):**
1. Create or update several `alog_log_infos` rows that all have valid parent `alog_logs` records
2. Run the HERO SDP Data Export Shell job
3. Check the export result, the bookmark in the data process log, and the records on the SDP platform or in the local SDP log

**Reproduction Result(s):**
1. Before the fix, a batch without orphan rows is exported successfully and the bookmark advances (the bug is not triggered in this scenario)

**Fix Result(s):**
1. The job sends the rows modified after the last export
2. All rows in the batch are exported successfully with no row skipped
3. `lastRecId` and `lastRecModTime` are updated, and the new or updated records can be verified on the SDP platform or in the local SDP log

#### 2.2. Verify alog_logs Parent Table Export Is Not Affected

**Prerequisite(s):**
1. The background job "HERO SDP Data Export Shell" is turned on
2. The `alog_logs` table is configured in the SDP data export flow

**Step(s):**
1. Create or update several `alog_logs` records so they fall into the next export window
2. Run the HERO SDP Data Export Shell job
3. Check the export result of the `alog_logs` table on the SDP platform or in its `sdp_full_log_per_table_XXXXX.log`

**Reproduction Result(s):**
1. Before the fix, the `alog_logs` table export works normally (the bug only affects the `alog_log_infos` batch)

**Fix Result(s):**
1. The `alog_logs` table export behaves exactly as before the fix
2. All modified `alog_logs` records are sent successfully and can be verified on the SDP platform or in the local SDP log

### 3. Regression Scope

The following related modules and functions must be retested after the fix:

- HERO SDP Data Export Shell background job scheduling and execution
- `alog_log_infos` table export, for batches with and without orphan rows
- `alog_logs` parent table export
- Other tables configured in the SDP data export flow (each table's own `sdp_full_log_per_table_XXXXX.log`)
- Data process log bookmark update (`lastRecId`, `lastRecModTime`) and the resume behaviour of the next export run
- Record verification on the SDP platform and in the local SDP log directory

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-69944 |
| Summary | [Bug Fix] SDP Interface - AlogLogInfo export stops on orphan info row |
| Sprint | Platform - 20261019-20261030 |
| Template | infrasys |
| Issue Type | Bug |
| QC Tester | Not assigned in JIRA |
| QC Reviewer | Not assigned in JIRA |
| Test Target | SDP Interface - HERO SDP Data Export Shell (`alog_log_infos` export) |
| Background Job | HERO SDP Data Export Shell (turned on) |
| Local SDP Log Path | `PATHTOHERO\account\{ACOUNT}\tmp\logs\sdp` (`sdp_full_log_per_table_XXXXX.log`, `sdp_error.log`) |
| Verification Points | New or updated records on the SDP platform; orphan-skip message in `sdp_error.log`; data process log bookmark (`lastRecId`, `lastRecModTime`) |
