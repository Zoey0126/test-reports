# TMS024 Waiting List Extraction Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Waiting List Extraction |
| Code | TMS024 |
| Issue Key | HERO-68127 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Outlet | Filter report by outlet scope | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Date | Select the business date or race date | Today, Yesterday, Custom Date | Today |
| Race | Filter by race identifier | All Races, Specific Race | All Races |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New indicator showing whether the reservation has a syndicate owner assigned | Sourced from DB field hkjc_resv_members: rmem_syndicate_owner_indicator; Displays Y (Yes), N (No), or blank (offline profile) |
| Syndicate Owner | Existing syndicate owner name/identifier | Existing field, unchanged behavior |
| Reservation Number | Unique reservation identifier | Sourced from reservation record |
| Extraction Status | Waiting list extraction state | Sourced from extraction module |

## Report Data Accuracy Verification

#### 1. New Column Position - Left of Syndicate Owner

**Prerequisite(s):**
1. Access to TMS report module with permission to run TMS024
2. Test environment with sample reservation data containing various syndicate owner indicator values

**Step(s):**
1. Open TMS024 Waiting List Extraction report
2. Select default parameters and click "Run"
3. Locate the existing "Syndicate Owner" column
4. Verify a new column labeled "Syndicate Owner Indicator" appears immediately to the LEFT

**Test Result(s):**
1. Report generates successfully without errors
2. "Syndicate Owner Indicator" column is positioned to the LEFT of "Syndicate Owner"
3. No other existing columns are displaced or repositioned

#### 2. Indicator Values Rendered Correctly (Y, N, Blank)

**Prerequisite(s):**
1. TMS024 report data with known rmem_syndicate_owner_indicator values
2. Database access to verify source values

**Step(s):**
1. Run TMS024 with parameters that include test reservations
2. For reservations with indicator value Y, N, and blank, compare displayed value with DB source
3. Verify each row's indicator matches the underlying data

**Test Result(s):**
1. Value Y displays "Y" in the column
2. Value N displays "N" in the column
3. Empty/null value (offline profile) displays blank
4. Any unexpected database value is rendered without causing a crash

#### 3. All Output Formats Include New Column

**Prerequisite(s):**
1. TMS024 runs successfully with the new column
2. Print and export functions are available

**Step(s):**
1. View the on-screen preview - verify column present
2. Print the report - verify column in correct position
3. Export to PDF - verify column present
4. Export to Excel/CSV - open file and verify column included with values

**Test Result(s):**
1. On-screen preview: column present and correctly positioned
2. Printed output: column present with correct indicator values
3. PDF export: column included with proper formatting
4. Excel/CSV export: column header present, all values match on-screen display

#### 4. Existing Sorting and Grouping Preserved

**Prerequisite(s):**
1. TMS024 with default or configured sorting/grouping behavior
2. Baseline output available for comparison

**Step(s):**
1. Run TMS024 with the same parameters as the baseline run
2. Compare row ordering and grouping structure between baseline and enhanced report
3. Verify no additional sorting/grouping is introduced by the new column

**Test Result(s):**
1. Row ordering is identical to baseline
2. Grouping structure is identical to baseline
3. No unexpected sorting by the new column is observed

## Report Functional Verification

#### 5. Export with Empty Result Set Still Includes Column Header

**Prerequisite(s):**
1. TMS024 configured with export function
2. A date range known to produce no reservation results

**Step(s):**
1. Run TMS024 with a date range containing no waiting list reservations
2. Export the empty result to CSV or Excel
3. Open the exported file

**Test Result(s):**
1. Report shows empty result set on screen
2. Exported file includes all column headers including "Syndicate Owner Indicator"
3. No data rows are present
4. File opens correctly without errors
