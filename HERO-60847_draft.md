# HERO-60847 [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V105 Test Report

## Report Information

| Field | Value |
| --- | --- |
| Issue Key | HERO-60847 |
| Summary | [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V105 |
| Type | Bug Fix |
| Component | Report Module |
| Version | V105 |
| Status | READY FOR QA |

## Bug Reproduction Summary

| Location | Error |
| --- | --- |
| `../Plugin/Report/Controller/Component/ReportExportExceptionDelayInSettlementExportComponent.php`, line 1675 | `Undefined array key 1` |
| `../Plugin/Report/Controller/ReportContentsController.php`, line 260 | `Undefined array key "iUserId"` |
| `../Plugin/Report/Controller/ReportContentsController.php`, line 271 | `Undefined array key "UserUser"` |
| `../Plugin/Report/View/Configs/admin_listing.ctp`, line 17 | `Undefined array key ""` (empty string) |
| Request URL `/accounts/{account}/menu/reports/common - menu.rptlibrary` etc. | `[MissingControllerException] Controller class ReportsController could not be found` |
| Request URL `/accounts/{account}/eng/report/` | `[MissingControllerException] Controller class ReportController could not be found` |
| Request URL `/accounts/{account}/eng/report/favourites/ws/ws` | `[MissingActionException] Action FavouritesController::ws() could not be found` |

**Root Cause:** Delay-in-settlement time threshold lines without commas were split into one value, then reading index 1 raised `Undefined array key 1`. Report content generation read `iUserId` and `UserUser` without key checks. Report system config page looked up Yes/No labels with an empty stored value. BIRT requests for missing report resources and `/{language}/report` were routed as controllers instead of being handled gracefully. Menu listing referenced `common - menu.rptlibrary` while the file on disk is `common.rptlibrary`.

## Functional Testing

### F1. Error Log Verification - Before Fix

#### F1.1. Reproduce all V105 undefined / MissingController exceptions
**Prerequisite(s):**
  1. Build WITHOUT the HERO-60847 fix deployed
  2. Delay-in-settlement export configured with threshold lines that DO NOT contain a comma
  3. Report contents generation preset parameters may have missing `iUserId`
  4. Session may be missing `UserUser`
  5. Report config page has an empty Yes/No stored value
**Step(s):**
  1. Run Delay In Settlement Exception Report export
  2. Navigate ReportContents controller actions (page that reads preset params and session UserUser)
  3. Open Report System Configuration admin listing page
  4. Attempt the example URLs from the ticket
  5. Inspect error.log after each action
**Reproduction Result(s):**
  1. error.log records `Undefined array key 1` in `ReportExportExceptionDelayInSettlementExportComponent.php`, line 1675
  2. error.log records `Undefined array key "iUserId"` and `Undefined array key "UserUser"` in `ReportContentsController.php`
  3. error.log records `Undefined array key ""` in `Configs/admin_listing.ctp`, line 17
  4. error.log records `MissingControllerException` and `MissingActionException` for the BIRT-resource and malformed report URLs

### F2. Error Log Verification - After Fix

#### F2.1. Verify no V105 undefined / MissingController warnings
**Prerequisite(s):**
  1. Build WITH the HERO-60847 fix deployed
  2. Same data and environment as F1; clear error.log before test
**Step(s):**
  1. Repeat the exact same flows from F1
  2. Inspect error.log
**Fix Result(s):**
  1. error.log does NOT contain any new `Undefined array key 1` entries from Delay In Settlement export
  2. error.log does NOT contain any new `Undefined array key "iUserId"` or `"UserUser"` entries from ReportContentsController
  3. error.log does NOT contain any new `Undefined array key ""` from Configs/admin_listing.ctp
  4. error.log does NOT contain any new `MissingControllerException` / `MissingActionException` for the listed URLs (they now produce graceful 404s or are not routed as controllers)
  5. Delay In Settlement export completes even with threshold lines that have no comma
  6. Report System Configuration page renders even when a Yes/No stored value is empty

### F3. Regression Testing

#### F3.1. Delay In Settlement export with comma-containing thresholds
**Step(s):**
  1. Run Delay In Settlement with threshold lines that DO contain commas (standard case)
**Fix Result(s):**
  1. Export still works correctly with comma-containing lines
  2. Second value after the comma is parsed as expected

#### F3.2. Report content generation with/without preset params
**Step(s):**
  1. Run ReportContents generation both with preset params and without them
  2. Run while session `UserUser` is present and while it is missing
**Fix Result(s):**
  1. All four combinations produce clean output with no warnings

#### F3.3. Menu listing common.rptlibrary (fixes filename reference)
**Step(s):**
  1. Open the Menu Listing by Display Panel report
**Fix Result(s):**
  1. Report loads the correct `common.rptlibrary` file
  2. No longer references the non-existent `common - menu.rptlibrary`

#### F3.4. Config page with valid Yes/No values
**Step(s):**
  1. Open Config admin listing on pages where all Yes/No values are properly stored
**Fix Result(s):**
  1. Page renders as before with correct label resolution

## Compatibility Testing

### Multi-Outlet Report Configurations
**Step(s):**
  1. Test Delay In Settlement, ReportContents, and Config admin pages on different outlet configurations
**Test Result(s):**
  1. All configurations produce clean error.log output
  2. No cross-outlet regressions

### Menu Listing Across Multiple Display Panels
**Step(s):**
  1. Run Menu Listing on multiple Display Panels
**Test Result(s):**
  1. All panels correctly reference `common.rptlibrary`
  2. No warnings

## Test Environment Information

- **Version**: Build containing HERO-60847 fix (Report V105)
- **Environment**: Local Test Environment, HQ Test Environment

## Appendix

### Affected Files
- `../Plugin/Report/Controller/Component/ReportExportExceptionDelayInSettlementExportComponent.php`
- `../Plugin/Report/Controller/ReportContentsController.php`
- `../Plugin/Report/View/Configs/admin_listing.ctp`
- Report / BIRT routing rules (handling missing resource URLs gracefully)
- Menu Listing display panel rptlibrary reference

### Release Notes Summary
- Fixed: Delay-in-settlement export handles threshold lines without commas — no longer reads index 1
- Fixed: Report content generation guards missing `iUserId` and `UserUser` keys
- Fixed: Report System Configuration page guards empty Yes/No stored values before label lookup
- Fixed: BIRT resource URLs and `/{language}/report` URLs are no longer routed as controller actions (graceful 404s)
- Fixed: Menu Listing references the correct file name (`common.rptlibrary`) instead of `common - menu.rptlibrary`
