# HERO-68126 Report - TMS022 Master Pre-Race Booking List - Add Syndicate Owner Indicator field Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Master Pre-Race Booking List Report |
| Code | TMS022 |
| Type | Improvement |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Business Date | Select the business date for the report | [Date Picker] | Current Date |
| Shops | Filter by shop(s) | All Shops, Single Shop, Multiple Shops | All Shops |
| Outlets | Filter by outlet(s) | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Meal Period | Filter by meal period | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | Syndicate Owner Indicator from DB field hkjc_resv_members.rmem_syndicate_owner_indicator. Displays "Y", "N", or blank. | Direct display from DB |
| Syndicate Owner | Syndicate Owner name | Direct display |

## Report Data Accuracy Verification

### 1. Verify Syndicate Owner Indicator Column is Displayed

#### 1.1. All Default Parameters

**Step(s):**
  1. Open TMS022 Master Pre-Race Booking List Report
  2. Select "default" from all parameters
    2.1. Select current business date from Business Date
    2.2. Select All Shops from Shops
    2.3. Select All Outlets from Outlets
    2.4. Select All from Meal Period
  3. Click "Run" button
  4. Verify the "Syndicate Owner Indicator" column is displayed immediately to the left of the "Syndicate Owner" column

**Test Result(s):**
  1. Report loads successfully
  2. The "Syndicate Owner Indicator" column is displayed immediately to the left of the "Syndicate Owner" column
  3. No errors are displayed

### 2. Verify Indicator Values Display Correctly

#### 2.1. Verify "Y" Value Display

**Prerequisite(s):**
  1. At least one reservation exists with rmem_syndicate_owner_indicator = "Y"

**Step(s):**
  1. Open TMS022 report
  2. Run with parameters that include the reservation with "Y" value
  3. Locate the reservation in the report output
  4. Verify the Syndicate Owner Indicator cell

**Test Result(s):**
  1. The cell displays "Y" for reservations where the DB value is Y
  2. The value is displayed as stored in the DB record
  3. No errors are displayed

#### 2.2. Verify "N" Value Display

**Prerequisite(s):**
  1. At least one reservation exists with rmem_syndicate_owner_indicator = "N"

**Step(s):**
  1. Open TMS022 report
  2. Run with parameters that include the reservation with "N" value
  3. Locate the reservation in the report output
  4. Verify the Syndicate Owner Indicator cell

**Test Result(s):**
  1. The cell displays "N" for reservations where the DB value is N
  2. The value is displayed as stored in the DB record
  3. No errors are displayed

#### 2.3. Verify Blank Value Display for Empty/Offline Profile

**Prerequisite(s):**
  1. At least one reservation exists with rmem_syndicate_owner_indicator = empty (created by Offline Profile)

**Step(s):**
  1. Open TMS022 report
  2. Run with parameters that include the reservation with empty value
  3. Locate the reservation in the report output
  4. Verify the Syndicate Owner Indicator cell

**Test Result(s):**
  1. The cell is blank for reservations where the DB value is empty
  2. No error or unexpected characters are displayed
  3. No errors are displayed

### 3. Verify Column Position

#### 3.1. Verify Syndicate Owner Indicator is Immediately Left of Syndicate Owner

**Step(s):**
  1. Open TMS022 report
  2. Run the report with default parameters
  3. Examine the column headers and their order
  4. Locate "Syndicate Owner Indicator" and "Syndicate Owner" columns
  5. Verify the position relationship

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column is immediately to the left of the "Syndicate Owner" column
  2. No other column is placed between them
  3. No errors are displayed

## Report Functional Verification

### 1. Verify Field Appears in All Exported Formats

**Step(s):**
  1. Open TMS022 report and run with default parameters
  2. Export to Excel, Excel (.xlsx), PDF, CSV
  3. Open each exported file
  4. Verify the "Syndicate Owner Indicator" column appears in all formats
  5. Verify the column is positioned immediately left of "Syndicate Owner" in all formats
  6. Verify indicator values (Y, N, blank) are correct in all formats

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column appears in all exported formats
  2. The column position is correct in all formats
  3. The indicator values match the on-screen display across all formats
  4. No errors are displayed

### 2. Verify Field Appears in Printed Formats

**Step(s):**
  1. Open TMS022 report and run with default parameters
  2. Print to HTML
  3. Print to PDF
  4. Verify the "Syndicate Owner Indicator" column appears in printed output
  5. Verify the column position and indicator values

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column appears in both HTML and PDF printed output
  2. The column position is correct
  3. The indicator values match the on-screen display
  4. No errors are displayed

### 3. Verify No Access Restrictions on New Field

**Prerequisite(s):**
  1. Users with different roles who can currently view TMS022 are available

**Step(s):**
  1. Log in as a user with standard access to TMS022
  2. Open and run TMS022 report
  3. Verify the "Syndicate Owner Indicator" column is visible
  4. Log in as another user with access to TMS022
  5. Verify the column is visible

**Test Result(s):**
  1. All users who can view TMS022 can see the "Syndicate Owner Indicator" column
  2. No additional permission restrictions are applied to the new field
  3. No errors are displayed

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge

**Step(s):**
  1. Open TMS022 report in Google Chrome, run with default parameters
  2. Verify the column is displayed correctly
  3. Repeat in Mozilla Firefox
  4. Repeat in Microsoft Edge

**Test Result(s):**
  1. Report loads successfully in all three browsers
  2. The "Syndicate Owner Indicator" column is displayed correctly in all browsers
  3. No rendering issues across browsers
  4. No errors are displayed

### 2. Support Data Service Compatibility - MySQL and TiDB

**Step(s):**
  1. Open TMS022 report
  2. Set date range to less than 7 days (MySQL) and run
  3. Verify the column and data are correct
  4. Set date range to more than 7 days (TiDB) and run
  5. Verify the column and data are correct

**Test Result(s):**
  1. Report retrieves data correctly from MySQL for short date ranges
  2. Report retrieves data correctly from TiDB for long date ranges
  3. The "Syndicate Owner Indicator" column displays correctly in both data sources
  4. No errors are displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
