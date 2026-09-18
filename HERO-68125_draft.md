# TMS020 Waiting List Report Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Waiting List Report |
| Code | TMS020 |
| Issue Key | HERO-68125 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Outlet | Filter report by outlet scope | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Date | Select date or date range | Today, Yesterday, Custom | Today |
| Race | Filter by race | All Races, Specific Race | All Races |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New indicator for syndicate ownership status | Sourced from DB field hkjc_resv_members: rmem_syndicate_owner_indicator; Y, N, or blank |
| Syndicate Owner | Existing syndicate owner identifier | Existing field, unchanged |
| Reservation Number | Reservation reference | Sourced from reservation record |
| Wait List Position | Position in waiting list | Sourced from waiting list module |

## Report Data Accuracy Verification

#### 1. New Column Position - LEFT of Syndicate Owner

**Prerequisite(s):**
1. Access to TMS020 report
2. Test data with varied syndicate owner indicator values

**Step(s):**
1. Open TMS020 Waiting List Report
2. Select default parameters and run
3. Locate "Syndicate Owner" column
4. Verify "Syndicate Owner Indicator" appears immediately to the LEFT

**Test Result(s):**
1. Report generates successfully
2. New column labeled "Syndicate Owner Indicator" is positioned LEFT of "Syndicate Owner"
3. Column width accommodates single-character values

#### 2. Indicator Value Display

**Prerequisite(s):**
1. Reservations with indicator value Y, N, and blank
2. DB access for value verification

**Step(s):**
1. Run TMS020 with full parameter scope
2. Verify display for each known indicator state:
   - Value Y → should show "Y"
   - Value N → should show "N"
   - Empty value (offline profile) → should show blank

**Test Result(s):**
1. All three indicator states render correctly
2. Values match source DB data exactly
3. No fallback to default values or unexpected characters

#### 3. On-Screen, Printed, and Exported Output

**Prerequisite(s):**
1. TMS020 runs successfully with new column
2. Print and export functions available

**Step(s):**
1. View on-screen report - verify column present and positioned correctly
2. Print the report - verify column in correct position with correct values
3. Export to PDF - verify column included
4. Export to Excel/CSV - open file, verify column header and values

**Test Result(s):**
1. All output formats include the new column
2. Column position (LEFT of Syndicate Owner) is consistent across formats
3. Exported values match on-screen values exactly
4. Empty result set export still includes column header

#### 4. Layout Reflow Without Data Loss

**Prerequisite(s):**
1. TMS020 with default page setup
2. Sufficient data to exercise layout reflow

**Step(s):**
1. Run TMS020 with full data scope
2. Inspect on-screen layout for column overflow
3. Print the report and check for truncation
4. Export to Excel and verify no data loss

**Test Result(s):**
1. Layout reflows freely to accommodate the new column
2. No existing column data is truncated or hidden
3. If page width is insufficient for print, indicator value remains fully visible
4. Excel export contains all column data complete without any truncation

## Report Functional Verification

#### 5. Various Parameter Combinations

**Prerequisite(s):**
1. TMS020 with multiple parameter options
2. Test data across different outlets and dates

**Step(s):**
1. Run with All Outlets + Multiple Races
2. Run with Single Outlet + Custom Date Range
3. Run with Multiple Outlets + Specific Race
4. Run with date range producing empty result

**Test Result(s):**
1. All parameter combinations run without errors
2. New column present in every output variation
3. Empty result case shows column headers but no data rows
4. Report performance not degraded vs pre-enhancement baseline
