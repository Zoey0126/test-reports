# TMS035 Waiting List Report (Owner Box) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Waiting List Report (Owner Box) |
| Code | TMS035 |
| Issue Key | HERO-68130 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Outlet | Filter report by outlet selection | All Outlets, Single Outlet | All Outlets |
| Business Date | Select the business date or date range | Today, Yesterday, Custom Date | Today |
| Race | Filter by race | All Races, Specific Race | All Races |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New indicator showing whether the reservation has a syndicate owner assigned | Sourced from DB field hkjc_resv_members: rmem_syndicate_owner_indicator; Displays Y (Yes), N (No), or blank (offline profile) |
| Syndicate Owner | Existing syndicate owner name/identifier | Existing field, unchanged behavior |
| Reservation Number | Unique reservation identifier | Sourced from reservation record |
| Wait List Status | Current waiting list position/status | Sourced from waiting list module |

## Report Data Accuracy Verification

#### 1. New Column Position Verification

**Prerequisite(s):**
1. Access to TMS report module with permission to run TMS035
2. Test environment with sample reservation data containing various syndicate owner indicator values (Y, N, blank)

**Step(s):**
1. Open TMS035 Waiting List Report (Owner Box)
2. Select default parameters
3. Click "Run" to generate the report
4. Locate the existing "Syndicate Owner" column in the report output
5. Verify the new column immediately to the LEFT of "Syndicate Owner" is labeled "Syndicate Owner Indicator"

**Test Result(s):**
1. Report generates successfully with no errors
2. "Syndicate Owner Indicator" column appears immediately to the LEFT of "Syndicate Owner" column
3. Column header text is exactly "Syndicate Owner Indicator"
4. Column width is sufficient to display the indicator value clearly

#### 2. Indicator Value Display - Y, N, and Blank

**Prerequisite(s):**
1. TMS035 report with sample data including reservations with indicator value Y, value N, and blank (offline profile)
2. Direct database access or reservation record viewer to confirm source values

**Step(s):**
1. Run TMS035 with parameters that include all test reservations
2. Identify reservation rows with known indicator values
3. Compare the displayed Syndicate Owner Indicator value against the source DB value
4. Verify rows with indicator value Y show "Y"
5. Verify rows with indicator value N show "N"
6. Verify rows with empty indicator value (offline profile) show blank

**Test Result(s):**
1. Reservations with rmem_syndicate_owner_indicator = Y display "Y" in the new column
2. Reservations with rmem_syndicate_owner_indicator = N display "N" in the new column
3. Reservations with empty/null rmem_syndicate_owner_indicator display blank (empty cell)
4. All displayed values match the source database values exactly

#### 3. All Output Formats Include New Column

**Prerequisite(s):**
1. TMS035 report runs successfully with the new column
2. Access to print and export functions

**Step(s):**
1. Run TMS035 and view the on-screen preview
2. Verify "Syndicate Owner Indicator" column is present in the preview
3. Print the report and verify the printed output includes the new column in the correct position
4. Export the report to Excel and open the exported file
5. Export the report to PDF and verify the PDF includes the new column

**Test Result(s):**
1. On-screen preview displays the new column correctly
2. Printed output includes "Syndicate Owner Indicator" column to the LEFT of "Syndicate Owner"
3. Excel export file contains the new column header and all indicator values
4. PDF export includes the new column with correct values and positioning
5. Column position is consistent across all output formats

#### 4. Existing Sorting and Grouping Behavior Preserved

**Prerequisite(s):**
1. TMS035 report with existing sorting or grouping configuration by Syndicate Owner
2. Baseline report output from before this enhancement for comparison

**Step(s):**
1. Run TMS035 with existing sorting/grouping by Syndicate Owner
2. Compare the row ordering and grouping in the enhanced report against the baseline
3. Verify the new column does not introduce any new sorting or grouping

**Test Result(s):**
1. Existing sorting by Syndicate Owner remains unchanged
2. Existing grouping behavior is preserved exactly as before
3. The new column does not cause any reordering of rows
4. No additional sorting/grouping is introduced by the enhancement

#### 5. Layout Reflow Does Not Truncate Data

**Prerequisite(s):**
1. TMS035 report with a wide set of columns that may cause horizontal overflow
2. Report configuration with default page setup

**Step(s):**
1. Run TMS035 with all default parameters
2. View the on-screen report and check all columns are visible
3. Print the report and inspect printed output for truncation
4. Export to Excel and verify all column data is complete

**Test Result(s):**
1. Report layout reflows to accommodate the new column
2. No existing column data is truncated or hidden
3. Exported Excel file contains complete data for all columns including the new indicator
4. If print layout width is limited, the Syndicate Owner Indicator value is still fully visible or wrapped appropriately

## Report Functional Verification

#### 6. Report Runs Without Errors Under Various Parameter Combinations

**Prerequisite(s):**
1. TMS035 report configured with multiple parameter options
2. Sufficient test data to cover different parameter combinations

**Step(s):**
1. Run TMS035 with All Outlets + Today's Race
2. Run TMS035 with Single Outlet + Custom Date Range
3. Run TMS035 with Multiple Races selected
4. Run TMS035 with date range containing no reservations (empty result set)

**Test Result(s):**
1. All parameter combinations run without errors
2. New column is present in all output variations
3. Empty result set still shows the Syndicate Owner Indicator column header
4. Report performance is not degraded compared to baseline run times
