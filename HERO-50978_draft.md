# POS044 Audit Control Listing Report — wrong previous-day date on UTC HQ; optimize loading performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Audit Control Listing Report |
| Report Code | POS044 |
| JIRA Issue | HERO-50978 |
| Component | REPORTS |
| Issue Type | Bug |
| Report Design | Plugin/Pos/webroot/reports/audit_control_listing/audit_control_listing.rptdesign (MySQL; SQL Server design aligned where applicable) |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Outlets whose audit control logs are listed (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Date Type | Date range type of the audit control listing (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Current Day, Last Day, Specified | Last Day |
| Begin Date / End Date | Date range boundaries, enabled when Date Type = 'Specified' | User Defined | - |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Header Date | Date shown in the report header; must follow the hotel local calendar via the shop time offset (iTimeOffset) | localNow = BirtDateTime.addMinute(now, iTimeOffset - JVM offset); Last Day = addDay(localNow, -1) |
| Log Date | Audit log date shown on the rows; filtered by the hotel local day with UTC time bounds | Local today = DATE(DATE_ADD(NOW(), INTERVAL iTimeOffset MINUTE)); UTC bounds via DATE_SUB(..., iTimeOffset) |

## Test Verification

### 1.Previous Day and Current Day Date Verification on UTC HQ

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. Audit control log data exists for the selected outlets and dates.
**Step(s):**
  1. Open POS044 Audit Control Listing Report.
  2. Select "default" from all parameters.
    2.1. Select All Outlets from Shops/Outlets.
    2.2. Select Last Day from Date Type.
  3. Click "Run" button.
  4. Verify the report header date and the row dates.
**Reproduction Result(s):**
  1. On a UTC HQ, the header and row dates are shifted one extra day earlier than the hotel local day.
**Fix Result(s):**
  1. The report header date and row dates follow the hotel local calendar for Last Day.
  2. Data displayed matches the audit logs of the hotel local last day.
  3. No errors displayed.

#### 1.2.[A Single Outlet, Last Day] Auto Report Sent in Local Morning on UTC HQ

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. HQ database and report server run on UTC; hotel local time is UTC+8 with shop time offset iTimeOffset = 480.
  3. POS044 Auto Report is configured with Date Type = Last Day (last_bday) and sends at about 06:00 hotel local time.
**Step(s):**
  1. Let the Auto Report send in the hotel local morning, before UTC has crossed midnight.
  2. Open the attached POS044 Excel / PDF from the auto report mail.
  3. Verify the report header date and the row dates against the hotel local yesterday.
**Reproduction Result(s):**
  1. Mail sent on 2025-01-14 local morning shows report header and rows dated 2025-01-12 instead of hotel local yesterday 2025-01-13, because bare CURRENT_DATE() on UTC picks the previous UTC calendar day.
**Fix Result(s):**
  1. The report header date and row dates show hotel local yesterday (2025-01-13) via the iTimeOffset-adjusted local day.
  2. The UTC time window of the queried audit logs (DATE_SUB(..., iTimeOffset)) matches that local day.
  3. No errors displayed.

#### 1.3.[A Single Outlet, Current Day] Current Day Follows Hotel Local Calendar

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. HQ runs on UTC and the hotel local time is ahead of UTC (for example UTC+8).
  3. Audit logs exist for the hotel local current day.
**Step(s):**
  1. Open POS044 Audit Control Listing Report in the hotel local morning.
  2. Select the single outlet and set Date Type = Current Day.
  3. Run the report.
  4. Verify the header date and the row dates against the hotel local today.
**Reproduction Result(s):**
  1. On UTC HQ, Current Day resolves to the UTC calendar day, so morning logs of the hotel local day are missing or dated incorrectly.
**Fix Result(s):**
  1. Current Day follows the hotel local calendar (local today from DATE(DATE_ADD(NOW(), INTERVAL iTimeOffset MINUTE))).
  2. Header date and row dates match the hotel local today.
  3. No errors displayed.

#### 1.4.[A Single Outlet, Specified Date Range] Specified Dates Unchanged

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. Audit logs exist within a known date range.
**Step(s):**
  1. Open POS044 Audit Control Listing Report.
  2. Select the single outlet and set Date Type = Specified.
  3. Enter the known Begin Date and End Date.
  4. Run the report and verify the returned rows.
**Reproduction Result(s):**
  1. Specified date range results were correct before the fix; confirm the date fix does not alter this behaviour.
**Fix Result(s):**
  1. The report returns exactly the audit logs within the specified local date range.
  2. Header date matches the selected range handling.
  3. No errors displayed.

#### 1.5.Different Shop Time Offsets

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. Two shops are available: one with time offset UTC+8 (iTimeOffset = 480) and one with offset 0 (UTC).
**Step(s):**
  1. Run POS044 Auto Report with Date Type = Last Day for the UTC+8 shop in its local morning.
  2. Verify the report date equals the shop local yesterday.
  3. Run POS044 Auto Report for the offset 0 shop.
  4. Verify the report date equals the UTC calendar day for that shop.
**Reproduction Result(s):**
  1. The UTC+8 shop report shows one extra day earlier because the offset is not applied to pick the local calendar day first.
**Fix Result(s):**
  1. The UTC+8 shop shows the shop local yesterday in header and rows.
  2. The offset 0 shop follows the UTC calendar as configured, as documented in the release note.
  3. Preset keeps a correct iTimeOffset for each shop.
  4. No errors displayed.

### 2.Report Loading Verification

#### 2.1.One Day and Single Outlet on Large HQ Database

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. A large HQ database with very large alog_logs / alog_log_infos volumes is available.
**Step(s):**
  1. Open POS044 Audit Control Listing Report.
  2. Select one day range and one outlet.
  3. Run the report.
  4. Verify the report finishes loading and displays the listing.
**Reproduction Result(s):**
  1. The main audit-log SQL is slow and the report appears to hang; no result is returned even for one day and one shop.
**Fix Result(s):**
  1. The one-day POS044 report loads and returns the listing in a reasonable time.
  2. The query drives from alog_logs with the time_outlet index (FORCE INDEX (time_outlet)) and then joins alog_log_infos.
  3. No errors displayed.

#### 2.2.One Day and Multiple Outlets Loading

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. The large HQ database contains audit logs of multiple outlets for the selected day.
**Step(s):**
  1. Open POS044 Audit Control Listing Report.
  2. Select one day range and multiple outlets.
  3. Run the report.
  4. Verify the report completes loading and displays the listing for all selected outlets.
**Reproduction Result(s):**
  1. The optimizer drives from out_outlets and applies alog_time as a residual filter, so the multi-outlet one-day report runs extremely long or hangs.
**Fix Result(s):**
  1. The report completes loading for the multi-outlet one-day selection.
  2. Rows of all selected outlets within the day are displayed.
  3. No errors displayed.

#### 2.3.Data Correctness After Loading Optimization

**Prerequisite(s):**
  1. A build with the HERO-50978 fix is deployed.
  2. Expected audit log rows for the selected day and outlets are known from the Audit Log UI or source data.
**Step(s):**
  1. Run POS044 with the same one-day selection as in the previous cases.
  2. Compare the returned rows with the expected audit log data row by row.
**Reproduction Result(s):**
  1. No complete result can be obtained before the fix to compare against.
**Fix Result(s):**
  1. The optimized time-first query returns exactly the expected audit log rows.
  2. No rows are missing or duplicated by the join order change.
  3. No errors displayed.

### 3.Regression Scope

- POS044 all Date Types (Current Day / Last Day / Specified) on both UTC HQ and non-UTC HQ environments.
- POS044 Auto Report mail attachments (Excel / PDF): header date and row dates must match the shop local calendar.
- Audit Log UI and AuditLogGeneralComponent queries that force the time_outlet index remain unaffected.
- SQL Server design of POS044: join order from alog_logs returns the same rows without MySQL FORCE INDEX.
- Multi-day and multi-outlet selections of POS044 to confirm loading time and data correctness after the optimization.

## Test Environment

- **Version**: Sprint build containing the HERO-50978 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (UTC database), ER Test Environment
