# TMS022 Master Pre-Race Booking List Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Master Pre-Race Booking List |
| Code | TMS022 |
| Issue Key | HERO-68126 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Outlet | Filter report by outlet scope | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Race Date | Select the race date | Today, Yesterday, Custom Date | Today |
| Race Number | Filter by specific race number | All Races, Specific Race Number | All Races |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New indicator showing whether reservation has a syndicate owner | Sourced from DB field hkjc_resv_members: rmem_syndicate_owner_indicator; Y, N, or blank |
| Syndicate Owner | Existing syndicate owner name | Existing field, unchanged |
| Reservation Number | Reservation identifier | Sourced from reservation record |
| Member Name | Horse owner/member name | Sourced from member profile |
| Race Details | Race number, time, venue | Sourced from race schedule |

## Report Data Accuracy Verification

#### 1. New Column Position Verification

**Prerequisite(s):**
1. Access to TMS report module with permission to run TMS022
2. Test data with syndicate owner indicator values Y, N, blank

**Step(s):**
1. Open TMS022 Master Pre-Race Booking List
2. Select default parameters and run report
3. Locate existing "Syndicate Owner" column
4. Check that "Syndicate Owner Indicator" appears immediately to its LEFT

**Test Result(s):**
1. Report generates successfully
2. New column labeled "Syndicate Owner Indicator" is positioned to the LEFT of "Syndicate Owner"
3. Column header text matches exactly

#### 2. Indicator Value Rendering for All Scenarios

**Prerequisite(s):**
1. Test environment with reservations covering all three indicator states
2. DB access to verify source values

**Step(s):**
1. Run TMS022 with parameters including all test reservations
2. Compare displayed indicator values with DB values for each state:
   - Reservations with rmem_syndicate_owner_indicator = Y
   - Reservations with rmem_syndicate_owner_indicator = N
   - Reservations with empty indicator value (offline profile)
3. Test with any unexpected DB value (non Y/N/empty)

**Test Result(s):**
1. Value Y → displays "Y"
2. Value N → displays "N"
3. Empty/null → displays blank
4. Unexpected value → renders without crash (shows raw value or blank depending on implementation)

#### 3. Column Present in Preview, Printed, and Exported Output

**Prerequisite(s):**
1. TMS022 runs successfully with new column
2. Print, PDF export, and Excel/CSV export functions available

**Step(s):**
1. View on-screen preview - check column present
2. Print the report - check column position and values
3. Export to PDF - check column in output
4. Export to Excel/CSV - open file, verify column header and values
5. For each format, confirm column is LEFT of Syndicate Owner

**Test Result(s):**
1. On-screen preview: column present and correctly positioned
2. Printed output: column present in correct position, values match
3. PDF export: column included with proper formatting
4. Excel/CSV export: column header present, all values match screen display
5. Position (LEFT of Syndicate Owner) is consistent across all formats

#### 4. Every Reservation Row Has Indicator Populated

**Prerequisite(s):**
1. TMS022 run with multiple reservations in result set
2. Total reservation count known (from source system)

**Step(s):**
1. Run TMS022 and count total data rows
2. Count rows in exported Excel/CSV file
3. Verify no rows are missing indicator column data
4. Confirm total row count matches between screen and export

**Test Result(s):**
1. Every reservation row in the report has the indicator column populated with Y, N, or blank
2. Total row count is consistent across on-screen, printed, and exported formats
3. No data rows are omitted due to the enhancement

## Report Functional Verification

#### 5. Layout Reflow and Data Integrity

**Prerequisite(s):**
1. TMS022 with many columns that may cause horizontal overflow
2. Default page/print setup

**Step(s):**
1. Run TMS022 with default parameters
2. Check on-screen layout - all columns visible or properly scrollable
3. Print report - check no column is truncated or hidden
4. Export to Excel - verify all data cells complete

**Test Result(s):**
1. Layout reflows to accommodate new column without truncating existing data
2. No existing column data is cut off or hidden
3. Exported Excel file contains complete data for every column including new indicator
4. If print width exceeds page width, column wraps appropriately rather than truncating content
