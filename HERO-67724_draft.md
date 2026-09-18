# SUN Cover Export v2 - TXT Export Format Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | SUN Cover Account v2 Export |
| Code | sun_cover_account_v2_export |
| Issue Key | HERO-67724 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Export File Format | File format for the export output | NDF, TXT | NDF |
| Business Date | Date for export data | Today, Yesterday, Custom | Today |
| Outlet | Scope of export data | All Outlets, Single Outlet | All Outlets |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Export File Format Selection | New parameter controlling output format | User-selectable from NDF or TXT; defaults to NDF |
| Account Number | SUN account identifier | Sourced from POS payment records |
| Cover Amount | Cover payment amount | Sourced from POS transaction data |
| Transaction Date | Business date of transaction | Sourced from POS transaction timestamp |

## Report Data Accuracy Verification

#### 1. Export File Format Parameter Visibility and Default Value

**Prerequisite(s):**
1. Access to SUN POS Export Program configuration
2. System has export function configured

**Step(s):**
1. Open the SUN POS Export Program configuration screen
2. Navigate to the "Export File Setup" section
3. Locate the new "Export File Format" option
4. Check the available selections
5. Verify the default value

**Test Result(s):**
1. A new "Export File Format" option is present in the Export File Setup section
2. Available selections are "NDF" and "TXT"
3. The default value is "NDF"
4. Existing configuration is not affected by adding this parameter

#### 2. NDF Format Selection - Existing Behavior Preserved

**Prerequisite(s):**
1. SUN POS Export Program configured with Export File Format = NDF (default)
2. Sample POS transaction data exists for the export date
3. Baseline NDF export file from before the enhancement is available for comparison

**Step(s):**
1. Open the SUN POS Export Program
2. Verify Export File Format is set to "NDF"
3. Select default parameters (Today, All Outlets)
4. Run the export program
5. Locate the generated NDF file
6. Compare the new NDF output with the baseline NDF file

**Test Result(s):**
1. Export runs successfully without errors
2. Output file is generated in NDF format (correct file structure and encoding)
3. File content is identical to the baseline output (no regressions)
4. File extension and naming convention remain unchanged

#### 3. TXT Format Selection - New TXT Format Output

**Prerequisite(s):**
1. SUN POS Export Program configured
2. Sample POS transaction data exists for the export date

**Step(s):**
1. Open the SUN POS Export Program configuration
2. Change Export File Format to "TXT"
3. Save the configuration
4. Run the export program with default parameters (Today, All Outlets)
5. Locate the generated export file
6. Open and inspect the file content

**Test Result(s):**
1. Export runs successfully without errors
2. Output file is generated in TXT format (plain text, appropriate line endings)
3. File contains all expected SUN cover account data
4. File encoding is appropriate for the target system (Swire Hotels)
5. File naming follows expected convention for TXT exports

#### 4. TXT File Content Validation

**Prerequisite(s):**
1. A TXT export file has been successfully generated
2. Source POS transaction data is available for cross-reference

**Step(s):**
1. Open the generated TXT export file
2. Verify file structure and record layout
3. Cross-check account numbers in the TXT file against POS source data
4. Cross-check cover amounts in the TXT file against POS source data
5. Verify transaction dates match between TXT file and POS data
6. Check that all records are present (no missing or extra records)

**Test Result(s):**
1. TXT file has correct structure with proper record separators
2. All account numbers match the source POS data exactly
3. All cover amounts match the source POS data exactly (correct precision/rounding)
4. Transaction dates are correctly populated
5. Record count in TXT file matches expected count from POS data
6. No duplicate or corrupted records present

## Report Functional Verification

#### 5. Switching Between Formats Does Not Break Existing Configuration

**Prerequisite(s):**
1. SUN POS Export Program with valid configuration
2. Export File Format parameter available

**Step(s):**
1. Set Export File Format to "NDF" and save
2. Run export successfully with NDF output
3. Change Export File Format to "TXT" and save
4. Run export successfully with TXT output
5. Change back to "NDF" and save
6. Run export again to confirm NDF still works

**Test Result(s):**
1. Both format selections work correctly after saving
2. No configuration corruption occurs when switching formats
3. NDF export still produces valid NDF output after toggling
4. All other export settings remain intact after format switch

#### 6. Different Outlet and Date Parameter Combinations

**Prerequisite(s):**
1. Export File Format set to TXT
2. Multiple outlets with different cover transaction data

**Step(s):**
1. Run TXT export with Single Outlet + Custom Date Range
2. Run TXT export with Multiple Outlets + Yesterday
3. Run TXT export with All Outlets + Today
4. For each run, verify output file is valid TXT format with correct data

**Test Result(s):**
1. All parameter combinations produce valid TXT files
2. Each file contains correct data for the selected scope
3. No format degradation or corruption across different parameter sets
4. Export performance is consistent with NDF export baseline

#### 7. Error Handling for Missing Data

**Prerequisite(s):**
1. Export File Format set to TXT
2. A date range known to have no SUN cover transactions

**Step(s):**
1. Run TXT export with a date range containing no relevant transactions
2. Observe the export behavior
3. Check if any output file is generated

**Test Result(s):**
1. Export handles the no-data case gracefully
2. Either no file is generated (expected) or an empty TXT file with appropriate headers is generated
3. No application crash or error message is displayed
4. User is informed appropriately about the empty result
