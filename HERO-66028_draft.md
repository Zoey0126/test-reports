# HERO-66028 Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V111 Test Report

## Overview

| Field | Value |
| --- | --- |
| Issue Key | HERO-66028 |
| Type | Bug |
| Sprint | Report - 20260921-20261002 |
| Summary | Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V111 |

## Test Scope

This bug fix addresses multiple PHP warnings (Undefined variables, Undefined array keys, and Trying to access array offset on null) written to error.log across Report content, Auto report naming, Sun cover custom type, and General sales FTP setup. The fix guards missing array keys and uninitialized variables so warnings are no longer logged while existing empty-result behavior is preserved.

## Test Cases

### 1. Report Contents - Missing Variables and Array Keys

**Prerequisite(s):**
  1. Login to Platform with an account that has access to Report content management.
  2. Open a report content configuration where toFormatParameters, outputFileFormat, and editId may be absent.

**Step(s):**
  1. Navigate to Report content configuration.
  2. Open the report content where toFormatParameters is not set.
  3. Trigger the flow that reads outputFileFormat and editId.
  4. Observe the page-overflow parameter, edit link, and HTML content disposition.
  5. Open the application error.log and check for "Undefined variable $outputFileFormat", "Undefined variable $editId", and "Undefined array key \"toFormatParameters\"" warnings.

**Reproduction Result(s):**
  1. Before the fix, when toFormatParameters was absent, "Undefined array key \"toFormatParameters\"" was written at ReportContentsController.php line 94.
  2. Before the fix, when outputFileFormat and editId were not initialized, "Undefined variable $outputFileFormat" (lines 528-529) and "Undefined variable $editId" (line 436) were written to error.log.

**Fix Result(s):**
  1. After the fix, toFormatParameters is checked before access; missing value results in no page-overflow parameter.
  2. After the fix, outputFileFormat and editId are initialized before use; missing values result in edit link not taken and HTML content disposition unchanged. No warnings are written to error.log.

### 2. Auto Report Shell - Missing Shop, Outlet, and Naming Keys

**Prerequisite(s):**
  1. Login to Platform with an account that has access to auto reports.
  2. Configure an auto report where sShops, sOutlets, OutShop, and index 0 may be absent in the naming/setup data.

**Step(s):**
  1. Run the AutoReportShell for the configured auto report.
  2. Check the generated report file name and shop code in the output.
  3. Open the application error.log and check for "Undefined array key 0", "sShops", "sOutlets", "OutShop", "Trying to access array offset on value of type null", and "Undefined variable $errorKey" warnings.

**Reproduction Result(s):**
  1. Before the fix, the auto report shell read index 0, sShops, sOutlets, OutShop, and errorKey when they were absent, writing the corresponding warnings to error.log at AutoReportShell.php lines 682, 826-828, 844-845, and 1219.

**Fix Result(s):**
  1. After the fix, the keys are checked before access. When absent, the shop code is left blank and the naming follows the previous empty result. No "Undefined array key" or "Undefined variable $errorKey" warnings are written to error.log.

### 3. Sun Cover Account V2 Export - Missing Custom Type Status Keys

**Prerequisite(s):**
  1. Login to Platform with an account that has access to Sun cover account export.
  2. Run the SunCoverAccountV2ExportShell with data where walk_in_custom_type_code_status, in_house_custom_type_code_status, and custom_type_code_status may be absent.

**Step(s):**
  1. Run the SunCoverAccountV2ExportShell.
  2. Check the export output for custom type status values.
  3. Open the application error.log and check for the custom type status warnings.

**Reproduction Result(s):**
  1. Before the fix, the export read custom type status keys when absent, writing "Undefined array key" and "Trying to access array offset on null" warnings to error.log at SunCoverAccountV2ExportShell.php lines 331, 335, 339.

**Fix Result(s):**
  1. After the fix, the custom type status keys are checked before access. When absent, the status defaults to F. No warnings are written to error.log.

### 4. General Sales V2 Export - Missing FTP Setup Keys

**Prerequisite(s):**
  1. Login to Platform with an account that has access to General Sales V2 export.
  2. Run the GeneralSalesV2ExportShell with an interface where type, passive_mode, debug, and MsgInterface may be absent.

**Step(s):**
  1. Run the GeneralSalesV2ExportShell.
  2. Check the export FTP configuration output.
  3. Open the application error.log and check for "Undefined array key \"type\"", "\"passive_mode\"", "\"debug\"", "\"MsgInterface\"", and "Trying to access array offset on null" warnings.

**Reproduction Result(s):**
  1. Before the fix, the export read FTP setup keys when absent, writing the corresponding warnings to error.log at GeneralSalesV2ExportShell.php lines 317, 377-383.

**Fix Result(s):**
  1. After the fix, the FTP setup keys are checked before access. When absent, the FTP type is left empty. No warnings are written to error.log.

### 5. Reports Controller Null Config and CTM SMS Start Time (Already Guarded)

**Prerequisite(s):**
  1. Login to Platform with an account that has access to reports and CTM SMS.

**Step(s):**
  1. Open a report with a null configuration.
  2. Trigger the CTM SMS flow where startTime may be absent.
  3. Open the application error.log and verify no related warnings appear.

**Reproduction Result(s):**
  1. The null config access in ReportsController and the CTM SMS startTime in MessagingSmsComponent were already guarded in an earlier fix. Confirm the guards are still in place and no regression.

**Fix Result(s):**
  1. The report opens without "Trying to access array offset on null" at ReportsController.php lines 813-814, and the CTM SMS flow runs without "Undefined variable $startTime" at MessagingSmsComponent.php line 1545.

## Regression Test Scope

The following areas are affected by this fix and should be regression tested:
- Report content configuration (page-overflow, edit link, HTML content disposition)
- Auto report shell naming and shop code
- Sun cover account V2 export custom type status
- General sales V2 export FTP setup
- Reports controller null config handling
- CTM SMS start time handling

## Test Environment

- **Version**: V111
- **Browser**: Chrome, Firefox, Edge
- **Environment**: HQ Test Environment
