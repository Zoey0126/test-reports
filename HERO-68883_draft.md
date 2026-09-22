# HERO-68883 Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V114 Test Report

## Overview

| Field | Value |
| --- | --- |
| Issue Key | HERO-68883 |
| Type | Bug |
| Sprint | Report - 20260921-20261002 |
| Summary | Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V114 |

## Test Scope

This bug fix addresses PHP warnings (Undefined array keys, Trying to access array offset on false/null, and MissingControllerException) written to error.log in the Report Module. The fix guards missing array keys, missing report/user records, and missing TMS report properties files so warnings are no longer logged while existing behavior is preserved.

## Test Cases

### 1. Auto Report Preset List - Missing Report Record and Missing User

**Prerequisite(s):**
  1. Login to Platform with an account that has access to Report module.
  2. Prepare an auto report preset whose linked report record has been removed from the database.
  3. Prepare an auto report preset whose linked user record has been removed from the database.

**Step(s):**
  1. Navigate to System Management > Report > Auto Report Configuration.
  2. Open the preset list that references the missing report record.
  3. Observe the displayed report name column.
  4. Open the preset list that references the missing user record.
  5. Observe the displayed user name column.
  6. Open the application error.log and check for new warnings.

**Reproduction Result(s):**
  1. Before the fix, when the linked report record was missing, PHP warning "Undefined array key 1" was written to error.log at ReportExportExceptionDelayInSettlementExportComponent.php line 1675.
  2. Before the fix, when the linked user record was missing, the preset list attempted to read a non-existent user name and logged related warnings.

**Fix Result(s):**
  1. After the fix, the preset list reads the report name only when the report record exists; when the record is missing, the displayed text stays empty. No "Undefined array key 1" warning is written to error.log.
  2. After the fix, the user name is read only when the user record exists; when the user is not found, the displayed text stays empty. No warnings are written to error.log.

### 2. Galaxy Avero Export - Item-Discount Rows Without Matched Check Item

**Prerequisite(s):**
  1. Login to Platform with an account that has access to Galaxy Avero export.
  2. Prepare a check that contains item-discount rows without a matched check item (e.g., a discount applied at check level that has no corresponding item).

**Step(s):**
  1. Run the Galaxy Avero export (GalaxyAveroExportShell) for the prepared check.
  2. Check the export output for the item-discount rows.
  3. Open the application error.log and check for new warnings related to "citm_info".

**Reproduction Result(s):**
  1. Before the fix, when processing item-discount rows without a matched check item, PHP warnings "Undefined array key \"citm_info\"" and "Trying to access array offset on null" were written to error.log at GalaxyAveroExportShell.php line 1264.

**Fix Result(s):**
  1. After the fix, item-discount rows without a matched check item are skipped (existing behavior preserved), and the "citm_info" array key is only accessed when present. No "Undefined array key \"citm_info\"" or "Trying to access array offset on null" warnings are written to error.log.

### 3. Missing TMS Report Properties File Returns HTTP 404

**Prerequisite(s):**
  1. Login to Platform as an account with access to TMS reports.
  2. Identify a TMS report and request a properties file that does not exist on disk (e.g., resources_en_US.properties for a report that only has other locale files).

**Step(s):**
  1. Navigate to the TMS reports area.
  2. Request the URL `/accounts/<account>/tms/reports/<report>/resources_en_US.properties` where the file does not exist on disk.
  3. Observe the HTTP response status.
  4. Open the application error.log and check for MissingControllerException.

**Reproduction Result(s):**
  1. Before the fix, requesting a missing TMS report properties file raised `[MissingControllerException] Controller class ReportsController could not be found` and the exception was logged in error.log.

**Fix Result(s):**
  1. After the fix, a missing TMS report properties file returns HTTP 404. Files that exist on disk are still served as before. No MissingControllerException is written to error.log.

### 4. Delay-in-Settlement Array Key Already Guarded

**Prerequisite(s):**
  1. Login to Platform with an account that has access to the Delay-in-Settlement export.

**Step(s):**
  1. Run the Delay-in-Settlement export with various settlement data (including edge cases where array index 1 might be absent).
  2. Check the export output.
  3. Open the application error.log and verify no new "Undefined array key 1" warning appears for this export.

**Reproduction Result(s):**
  1. The array key 1 access in Delay-in-Settlement export was already guarded in an earlier fix. Confirm the guard is still in place and no regression.

**Fix Result(s):**
  1. The export runs successfully without writing "Undefined array key 1" warnings to error.log. Existing output behavior is unchanged.

## Regression Test Scope

The following areas are affected by this fix and should be regression tested:
- Auto Report preset list display (report name and user name columns)
- Galaxy Avero export with check-level discounts
- TMS report properties file serving (existing locale files must still load correctly)
- Delay-in-Settlement export

## Test Environment

- **Version**: V114
- **Browser**: Chrome, Firefox, Edge
- **Environment**: HQ Test Environment
