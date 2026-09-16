# POS028 Monthly Revenue Report - optimize loading performance and fix last/current month timezone Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Monthly Revenue Report |
| Report Code | POS028 |
| JIRA Issue | HERO-68275 |
| Component | REPORTS |
| Issue Type | Bug |
| Related Ticket | Ticket #1924232 (CS \| JUMBO GROUP \| REPORT \| Auto Report generating incorrect data) |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Outlets whose monthly revenue is listed (multi-select) | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Month Type | Month range type of the monthly revenue listing (single-select) | Current Month, Last Month, Specified | Last Month |
| Begin Month / End Month | Month range boundaries, enabled when Month Type = 'Specified' | User Defined | - |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Header Month | Month shown in the report header; must follow the hotel local calendar via the shop time offset (iTimeOffset) | localNow = BirtDateTime.addMinute(now, iTimeOffset - JVM offset); Last Month = addMonth(localNow, -1) |
| Revenue Rows Month | Revenue month shown on the rows; filtered by the hotel local month with UTC time bounds | Local current month = DATE_FORMAT(DATE_ADD(NOW(), INTERVAL iTimeOffset MINUTE), '%Y-%m'); UTC bounds via DATE_SUB(..., iTimeOffset) |

## Test Verification

### 1.Last Month and Current Month Timezone Verification on UTC HQ

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. Monthly revenue data exists for the selected outlets and months.
**Step(s):**
  1. Open POS028 Monthly Revenue Report.
  2. Select default values for all parameters.
    2.1. Select All Outlets from Shops/Outlets.
    2.2. Select Last Month from Month Type.
  3. Click "Run" button.
  4. Verify the report header month and the row months.
**Reproduction Result(s):**
  1. On a UTC HQ, when the scheduled task executes on September 1st, the report header and row months are shifted one extra month earlier (July) instead of the hotel local last month (August).
**Fix Result(s):**
  1. The report header month and row months follow the hotel local calendar for Last Month.
  2. Data displayed matches the monthly revenue of the hotel local last month.
  3. No errors displayed.

#### 1.2.Single Outlet, Last Month Auto Report Sent on the 1st of the Month on UTC HQ

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. HQ database and report server run on UTC; hotel local time is UTC+8 with shop time offset iTimeOffset = 480.
  3. POS028 Auto Report is configured with Month Type = Last Month and sends on the 1st day of each month at about 06:00 hotel local time.
**Step(s):**
  1. Let the Auto Report send on September 1st in the hotel local morning, before UTC has crossed midnight into September.
  2. Open the attached POS028 Excel / PDF from the auto report mail.
  3. Verify the report header month and the row months against the hotel local last month (August).
**Reproduction Result(s):**
  1. Mail sent on 2026-09-01 local morning shows report header and rows dated 2026-07 instead of hotel local last month 2026-08, because bare CURRENT_MONTH() on UTC picks the previous UTC calendar month when the JVM date is still in August.
**Fix Result(s):**
  1. The report header month and row months show hotel local last month (2026-08) via the iTimeOffset-adjusted local month.
  2. The UTC time window of the queried revenue rows matches that local month.
  3. No errors displayed.

#### 1.3.Single Outlet, Current Month Follows Hotel Local Calendar

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. HQ runs on UTC and the hotel local time is ahead of UTC (for example UTC+8).
  3. Monthly revenue data exists for the hotel local current month.
**Step(s):**
  1. Open POS028 Monthly Revenue Report in the hotel local morning of the 1st day of the month.
  2. Select the single outlet and set Month Type = Current Month.
  3. Run the report.
  4. Verify the header month and the row months against the hotel local current month.
**Reproduction Result(s):**
  1. On UTC HQ, Current Month resolves to the UTC calendar month, so on the morning of the 1st the report shows the previous month and the new month's revenue rows are missing or dated incorrectly.
**Fix Result(s):**
  1. Current Month follows the hotel local calendar (local current month from DATE_FORMAT(DATE_ADD(NOW(), INTERVAL iTimeOffset MINUTE), '%Y-%m')).
  2. Header month and row months match the hotel local current month.
  3. No errors displayed.

#### 1.4.Single Outlet, Specified Month Range Unchanged

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. Monthly revenue data exists within a known month range.
**Step(s):**
  1. Open POS028 Monthly Revenue Report.
  2. Select the single outlet and set Month Type = Specified.
  3. Enter the known Begin Month and End Month.
  4. Run the report and verify the returned rows.
**Reproduction Result(s):**
  1. Specified month range results were correct before the fix; confirm the timezone fix does not alter this behaviour.
**Fix Result(s):**
  1. The report returns exactly the revenue rows within the specified local month range.
  2. Header month matches the selected range handling.
  3. No errors displayed.

#### 1.5.Different Shop Time Offsets

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. Two shops are available: one with time offset UTC+8 (iTimeOffset = 480) and one with offset 0 (UTC).
**Step(s):**
  1. Run POS028 Auto Report with Month Type = Last Month for the UTC+8 shop on the 1st day of the month in its local morning.
  2. Verify the report month equals the shop local last month.
  3. Run POS028 Auto Report for the offset 0 shop.
  4. Verify the report month equals the UTC calendar month for that shop.
**Reproduction Result(s):**
  1. The UTC+8 shop report shows one extra month earlier (July instead of August) because the offset is not applied to pick the local calendar month first.
**Fix Result(s):**
  1. The UTC+8 shop shows the shop local last month in header and rows.
  2. The offset 0 shop follows the UTC calendar as configured, as documented in the release note.
  3. Preset keeps a correct iTimeOffset for each shop.
  4. No errors displayed.

### 2.Report Loading Performance Verification

#### 2.1.Single Month and Single Outlet on Large HQ Database

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. A large HQ database with very large revenue / pos_check_extra_infos volumes is available.
**Step(s):**
  1. Open POS028 Monthly Revenue Report.
  2. Select one month range and one outlet.
  3. Run the report.
  4. Verify the report finishes loading and displays the revenue listing.
**Reproduction Result(s):**
  1. The main revenue SQL is slow and the report appears to hang; no result is returned in a reasonable time even for one month and one shop.
**Fix Result(s):**
  1. The one-month POS028 report loads and returns the listing in a reasonable time.
  2. The query drives from the time-indexed revenue table and joins pos_check_extra_infos via pre-aggregation.
  3. No errors displayed.

#### 2.2.Single Month and Multiple Outlets Loading

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. The large HQ database contains revenue data of multiple outlets for the selected month.
**Step(s):**
  1. Open POS028 Monthly Revenue Report.
  2. Select one month range and multiple outlets.
  3. Run the report.
  4. Verify the report completes loading and displays the listing for all selected outlets.
**Reproduction Result(s):**
  1. The optimizer drives from out_outlets and applies revenue_time as a residual filter, so the multi-outlet one-month report runs extremely long or hangs.
**Fix Result(s):**
  1. The report completes loading for the multi-outlet one-month selection.
  2. Rows of all selected outlets within the month are displayed.
  3. No errors displayed.

#### 2.3.Data Correctness After Loading Optimization

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. Expected revenue rows for the selected month and outlets are known from the source data or a baseline export.
**Step(s):**
  1. Run POS028 with the same one-month selection as in the previous cases.
  2. Compare the returned rows with the expected revenue data row by row.
**Reproduction Result(s):**
  1. No complete result can be obtained before the fix to compare against.
**Fix Result(s):**
  1. The optimized time-first query returns exactly the expected revenue rows.
  2. No rows are missing or duplicated by the join order change.
  3. No errors displayed.

#### 2.4.Full Year and Multiple Outlets Loading

**Prerequisite(s):**
  1. A build with the HERO-68275 fix is deployed.
  2. The large HQ database contains revenue data of multiple outlets for a full year.
**Step(s):**
  1. Open POS028 Monthly Revenue Report.
  2. Select a full year range (12 months) and multiple outlets.
  3. Run the report.
  4. Verify the report completes loading and displays the listing for all selected outlets.
**Reproduction Result(s):**
  1. The full-year multi-outlet report runs for an unacceptable duration or times out before the fix.
**Fix Result(s):**
  1. The report completes loading for the full-year multi-outlet selection within an acceptable time.
  2. All revenue rows of all selected outlets within the year are displayed.
  3. No errors displayed.

### 3.Regression Scope

- POS028 all Month Types (Current Month / Last Month / Specified) on both UTC HQ and non-UTC HQ environments.
- POS028 Auto Report mail attachments (Excel / PDF): header month and row months must match the shop local calendar.
- Revenue UI and revenue-related queries that force the time index remain unaffected.
- SQL Server design of POS028: join order from the time-indexed revenue table returns the same rows without MySQL FORCE INDEX.
- Multi-month and multi-outlet selections of POS028 to confirm loading time and data correctness after the optimization.

## Test Environment

- **Version**: Sprint build containing the HERO-68275 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (UTC database), ER Test Environment
