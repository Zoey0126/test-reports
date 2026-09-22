# HERO-68071 Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V113 Test Report

## Overview

| Field | Value |
| --- | --- |
| Issue Key | HERO-68071 |
| Type | Bug |
| Sprint | Report - 20260921-20261002 |
| Summary | Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V113 |

## Test Scope

This bug fix addresses PHP warnings (Undefined array keys and Trying to access array offset on null) written to error.log in the Report Module. The fix guards missing array keys in preset parameter listing, report export language index, cross-outlet revenue export, and Sun cover account export so warnings are no longer logged while existing behavior is preserved.

## Test Cases

### 1. Preset Parameter Listing - Missing User "suspended" Flag

**Prerequisite(s):**
  1. Login to Platform with an account that has access to Report preset parameters.
  2. Prepare a user record that does not have the "suspended" flag set (UserUser.suspended key absent).

**Step(s):**
  1. Navigate to the preset parameter listing page.
  2. Observe the user name column for the user without the "suspended" flag.
  3. Open the application error.log and check for "Undefined array key \"suspended\"" warnings.

**Reproduction Result(s):**
  1. Before the fix, the preset parameter listing checked UserUser.suspended when the key was absent, writing "Undefined array key \"suspended\"" to error.log at listing.ctp line 196.

**Fix Result(s):**
  1. After the fix, the "suspended" key is checked before access. A user without that flag is still shown in the normal name style. No "Undefined array key \"suspended\"" warning is written to error.log.

### 2. Report Export - Missing Language Index 3

**Prerequisite(s):**
  1. Login to Platform with an account that has access to report export.
  2. Configure a report where language index 3 is not set (only 1-2 languages configured).

**Step(s):**
  1. Open the report and trigger export.
  2. Check the export output for language URL and code fields.
  3. Open the application error.log and check for "Undefined array key 3" and "Trying to access array offset on null" warnings.

**Reproduction Result(s):**
  1. Before the fix, the export handler read language index 3 when that language was not configured, writing "Undefined array key 3" and "Trying to access array offset on null" to error.log at ReportExportHandler.php lines 42-43.

**Fix Result(s):**
  1. After the fix, the language index 3 is read only when configured. When not configured, the language URL and code stay empty. No "Undefined array key 3" or "Trying to access array offset on null" warnings are written to error.log.

### 3. Cross-Outlet Revenue Export - Missing export_file_setup

**Prerequisite(s):**
  1. Login to Platform with an account that has access to cross-outlet revenue export.
  2. Configure an interface that has no export path (export_file_setup key absent).

**Step(s):**
  1. Run the CrossOutletRevenueFixedLayoutExportShell for the interface with no export path.
  2. Observe the shell output message.
  3. Open the application error.log and check for "Undefined array key \"export_file_setup\"" and "Trying to access array offset on null" warnings.

**Reproduction Result(s):**
  1. Before the fix, the cross-outlet revenue export read export_file_setup when the interface had no export path, writing "Undefined array key \"export_file_setup\"" and "Trying to access array offset on null" to error.log at CrossOutletRevenueFixedLayoutExportShell.php line 174.

**Fix Result(s):**
  1. After the fix, the export_file_setup key is checked before access. The shell still stops with the empty export path message. No "Undefined array key \"export_file_setup\"" or "Trying to access array offset on null" warnings are written to error.log.

### 4. Auto Report sOutlets and Sun Cover Custom Type Status (Already Guarded)

**Prerequisite(s):**
  1. Login to Platform with an account that has access to auto reports and Sun cover account export.

**Step(s):**
  1. Run the AutoReportShell with configurations where sOutlets may be absent.
  2. Run the SunCoverAccountV2ExportShell with configurations where walk_in_custom_type_code_status, in_house_custom_type_code_status, and custom_type_code_status may be absent.
  3. Open the application error.log and verify no related warnings appear.

**Reproduction Result(s):**
  1. The sOutlets access in AutoReportShell and the custom type status keys in SunCoverAccountV2ExportShell were already guarded in an earlier fix. Confirm the guards are still in place and no regression.

**Fix Result(s):**
  1. The exports run successfully without writing "Undefined array key \"sOutlets\"" or custom type status warnings to error.log. Existing output behavior is unchanged.

## Regression Test Scope

The following areas are affected by this fix and should be regression tested:
- Preset parameter listing user display
- Report export language handling
- Cross-outlet revenue fixed layout export
- Auto report shell execution
- Sun cover account V2 export

## Test Environment

- **Version**: V113
- **Browser**: Chrome, Firefox, Edge
- **Environment**: HQ Test Environment
