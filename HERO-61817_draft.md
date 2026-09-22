# HERO-61817 [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V106 Test Report

## Report Information

| Field | Value |
| --- | --- |
| Issue Key | HERO-61817 |
| Summary | [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V106 |
| Type | Bug Fix |
| Component | Report Module |
| Version | V106 |
| Status | READY FOR QA |

## Bug Reproduction Summary

| Location | Error |
| --- | --- |
| `../Plugin/Report/View/Reports/listing.ctp`, line 145, 164 | `Undefined array key "disable_report_code"` |
| `../Plugin/Report/Controller/Component/ReportExportExceptionReportDelayOpeningExportComponent.php`, line 857, 860 | `Maximum execution time of 360 seconds exceeded` (Fatal Error) |
| Request URL `/accounts/{account}/eng/report/export/export` | `[InternalErrorException] Maximum execution time of 360 seconds exceeded` |

**Root Cause:** Report listing read `disable_report_code` unconditionally, so reports without that flag raised `Undefined array key`. Delay Opening export ran under the default 360-second PHP limit; nested business-day, period, and payment loops exceeded that limit. `generateContent()` did not extend execution time unlike other exception export components.

## Functional Testing

### F1. Error Log Verification - Before Fix

#### F1.1. Reproduce report listing disable_report_code and Delay Opening export timeout
**Prerequisite(s):**
  1. Build WITHOUT the HERO-61817 fix deployed
  2. At least one report record does NOT have the `disable_report_code` flag
  3. Delay Opening export report configured with enough business-day/period/payment data to exceed 360s
**Step(s):**
  1. Navigate to Report listing page (`Reports/listing.ctp`)
  2. Inspect error.log for `Undefined array key "disable_report_code"`
  3. Open Delay Opening Exception Report via `/eng/report/export/export`
  4. Set a date range / outlet scope that produces a large data volume
  5. Click Export; wait up to 6 minutes
**Reproduction Result(s):**
  1. error.log records `Undefined array key "disable_report_code"` at `listing.ctp` line 145 and 164
  2. error.log records `Maximum execution time of 360 seconds exceeded` and a `Fatal Error` from `ReportExportExceptionReportDelayOpeningExportComponent.php`

### F2. Error Log Verification - After Fix

#### F2.1. Verify no disable_report_code warnings and Delay Opening export no longer times out
**Prerequisite(s):**
  1. Build WITH the HERO-61817 fix deployed
  2. Same report data and environment as F1; clear error.log before test
**Step(s):**
  1. Repeat the Report listing page access from F1 (with same report records that lack `disable_report_code`)
  2. Inspect error.log for `Undefined array key "disable_report_code"`
  3. Run Delay Opening export with the SAME large data scope from F1
  4. Measure the export time and verify completion
**Fix Result(s):**
  1. error.log does NOT contain any new `Undefined array key "disable_report_code"` entries
  2. Delay Opening export completes successfully within an extended PHP time limit
  3. error.log does NOT contain `Maximum execution time of 360 seconds exceeded`
  4. Export file is generated and contains correct data

### F3. Regression Testing

#### F3.1. Other report exports still honour disable_report_code correctly
**Step(s):**
  1. Run exports for reports that DO have `disable_report_code = 1`
  2. Verify they are excluded from the export listing
**Fix Result(s):**
  1. Reports with `disable_report_code = 1` are correctly hidden from export actions
  2. Reports without the flag still show up as before

#### F3.2. Other exception export components still work
**Step(s):**
  1. Run exports for other exception reports (not Delay Opening)
**Fix Result(s):**
  1. All other exception exports complete successfully
  2. No new warnings in error.log

#### F3.3. Very large Delay Opening export
**Step(s):**
  1. Run Delay Opening with an even wider date range than F2.1
**Fix Result(s):**
  1. Export completes (may take longer, but does not abort at 360s)
  2. No fatal error

## Compatibility Testing

### Multi-Outlet / Multi-Currency Delay Opening Data
**Step(s):**
  1. Run Delay Opening on accounts with many outlets and currencies
**Test Result(s):**
  1. Export completes without timeout
  2. All rows are present

### Performance Regression Check
**Step(s):**
  1. Run Delay Opening with the SAME test scope as F2.1 repeatedly
  2. Verify each run completes in roughly similar time
**Test Result(s):**
  1. Execution time is consistent across runs
  2. No memory leaks or gradual slowdown

## Test Environment Information

- **Version**: Build containing HERO-61817 fix (Report V106)
- **Environment**: Local Test Environment, HQ Test Environment, shangrilagroup customer environment

## Appendix

### Affected Files
- `../Plugin/Report/View/Reports/listing.ctp`
- `../Plugin/Report/Controller/Component/ReportExportExceptionReportDelayOpeningExportComponent.php`

### Release Notes Summary
- Fixed: Report listing reads `disable_report_code` only when that key exists on the report row
- Fixed: Delay Opening Exception Report export now extends PHP execution time in `generateContent()` (matching other exception export components)
- Fixed: `Maximum execution time of 360 seconds exceeded` fatal error no longer occurs during Delay Opening export
