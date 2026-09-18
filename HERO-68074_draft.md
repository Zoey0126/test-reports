# Many errors about "Undefined array keys" and "Undefined variables" -V113 Test Report

## Functional Testing

### 1.SunCoverAccountExportShell Undefined Array Key Errors

#### 1.1.Walk-in custom type code undefined key

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. The SunCoverAccountExportShell export job is configured and a SunCover account export is available to run
3. The error.log is cleared before testing

**Step(s):**
1. Trigger the SunCoverAccountExportShell export job with a configured SunCover account that does not set walk_in_custom_type_code
2. Wait for the export job to complete
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entry "Undefined array key "walk_in_custom_type_code"" from SunCoverAccountExportShell
2. The export job may still complete but the log is polluted with warnings

**Fix Result(s):**
1. The SunCoverAccountExportShell export job runs without generating any "Undefined array key" warning
2. error.log contains no entry for "Undefined array key "walk_in_custom_type_code"" from SunCoverAccountExportShell
3. The exported data is generated correctly and completely

#### 1.2.In-house account code undefined key

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. The SunCoverAccountExportShell export job is configured and a SunCover account export is available to run
3. The error.log is cleared before testing

**Step(s):**
1. Trigger the SunCoverAccountExportShell export job with a configured SunCover account that does not set in_house_account_code
2. Wait for the export job to complete
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entry "Undefined array key "in_house_account_code"" from SunCoverAccountExportShell

**Fix Result(s):**
1. The export job runs without generating the "Undefined array key "in_house_account_code"" warning
2. error.log contains no related warning from SunCoverAccountExportShell

#### 1.3.Export file naming and type setting undefined key

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. The SunCoverAccountExportShell export job is configured without export_file_naming_and_type_setting

**Step(s):**
1. Trigger the SunCoverAccountExportShell export job
2. Wait for the export job to complete
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entry "Undefined array key "export_file_naming_and_type_setting"" from SunCoverAccountExportShell

**Fix Result(s):**
1. The export job runs without generating the "Undefined array key "export_file_naming_and_type_setting"" warning
2. error.log contains no related warning from SunCoverAccountExportShell

#### 1.4.Custom type code and account code undefined keys

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. The SunCoverAccountExportShell export job is configured with a SunCover account that does not set custom_type_code or account_code

**Step(s):**
1. Trigger the SunCoverAccountExportShell export job
2. Wait for the export job to complete
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entries "Undefined array key "custom_type_code"" and "Undefined array key "account_code"" from SunCoverAccountExportShell

**Fix Result(s):**
1. The export job runs without generating the "Undefined array key "custom_type_code"" or "Undefined array key "account_code"" warnings
2. error.log contains no related warnings from SunCoverAccountExportShell

### 2.GeneralSalesV2ExportShell Undefined Array Key Errors

#### 2.1.Date mode undefined key

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. The GeneralSalesV2ExportShell export job is configured and available to run
3. The error.log is cleared before testing

**Step(s):**
1. Trigger the GeneralSalesV2ExportShell export job without setting dateMode in the export configuration
2. Wait for the export job to complete
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entry "Undefined array key "dateMode"" from GeneralSalesV2ExportShell

**Fix Result(s):**
1. The GeneralSalesV2ExportShell export job runs without generating the "Undefined array key "dateMode"" warning
2. error.log contains no related warning from GeneralSalesV2ExportShell
3. The exported sales data is generated correctly

### 3.PosPrintChecksComponent Undefined Array Key Errors

#### 3.1.Page bottom margin undefined key

**Prerequisite(s):**
1. The build containing the HERO-68074 fix is deployed to the test POS environment
2. A print format is configured that does not explicitly set page_bottom_margin
3. The error.log is cleared before testing

**Step(s):**
1. Open a check that uses the configured print format
2. Trigger any check printing or receipt printing that calls PosPrintChecksComponent (line 6654 path)
3. Open tmp/log/error.log and search for "Undefined array key"

**Reproduction Result(s):**
1. error.log contains the entry "Undefined array key "page_bottom_margin"" from ../Plugin/Pos/Controller/Component/PosPrintChecksComponent.php line 6654
2. The print output may render but with warnings recorded

**Fix Result(s):**
1. The printing flow runs without generating the "Undefined array key "page_bottom_margin"" warning
2. error.log contains no related warning from PosPrintChecksComponent
3. The printed check or receipt renders correctly with the expected bottom margin behaviour

### 4.Error Log Verification After Full Run

#### 4.1.No undefined array key or undefined variable warnings in error.log

**Prerequisite(s):**
1. All previous scenarios (1.1 to 1.4, 2.1 and 3.1) have been executed in the fixed build
2. tmp/log/error.log is the same file used across all scenarios

**Step(s):**
1. Open tmp/log/error.log after executing all scenarios above
2. Search for the previously reported warning strings: "Undefined array key "walk_in_custom_type_code"", "Undefined array key "in_house_account_code"", "Undefined array key "export_file_naming_and_type_setting"", "Undefined array key "custom_type_code"", "Undefined array key "account_code"", "Undefined array key "dateMode"", "Undefined array key "page_bottom_margin"" and "Undefined variable $langPath"
3. Confirm none of these warning strings appear in error.log originating from SunCoverAccountExportShell, GeneralSalesV2ExportShell or PosPrintChecksComponent

**Reproduction Result(s):**
1. error.log contains one or more of the previously reported Undefined array key and Undefined variable warnings from the affected modules

**Fix Result(s):**
1. error.log contains none of the previously reported Undefined array key or Undefined variable warnings from SunCoverAccountExportShell, GeneralSalesV2ExportShell or PosPrintChecksComponent
2. No new Undefined array key or Undefined variable warnings are introduced by the fix

### 5.Regression Scope

The following related modules and scenarios must be retested after the fix:

- SunCoverAccountExportShell export workflow end to end
- GeneralSalesV2ExportShell export workflow end to end
- PosPrintChecksComponent print flows for guest check, receipt and reprint
- Any PHP 8 deprecation notices or warnings in error.log for the affected paths
- Print formats that use page_bottom_margin and other previously undefined keys
