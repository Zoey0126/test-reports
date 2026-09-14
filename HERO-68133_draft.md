# Report - TMS039 - Add Syndicate Owner Indicator field Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Override Date Limitation for Advance Booking |
| Code | TMS039 |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Shops and outlets included in the report (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business date range of the advance booking reservations (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Yesterday, Today, Specified as Below | Yesterday |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New column displayed to the LEFT of the Syndicate Owner field. Shows the reservation syndicate owner indicator from the TMS reservation record (DB field hkjc_resv_members: rmem_syndicate_owner_indicator): DB value Y displays as Y, DB value N displays as N, empty value (reservation uses an offline profile with no Syndicate Owner Indicator) displays as blank | - |
| Syndicate Owner | Shows the syndicate owner of the reservation | - |

## Report Data Accuracy Verification

### 1.Syndicate Owner Indicator Display

#### 1.1.Verify Syndicate Owner Indicator column is added with default parameters
**Prerequisite(s):**
  1. The TMS039 Override Date Limitation for Advance Booking report is accessible to the test user.
  2. Reservations with different Syndicate Owner Indicator DB values (Y, N, and empty) exist in the test data.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Verify a new column named Syndicate Owner Indicator is displayed.
  5. Verify the new column is positioned to the LEFT of the Syndicate Owner column.
  6. Verify the indicator values in the report match the DB field hkjc_resv_members: rmem_syndicate_owner_indicator for each reservation.
**Test Result(s):**
  1. The Syndicate Owner Indicator column is displayed to the LEFT of the Syndicate Owner column.
  2. Indicator values match the DB values for each reservation.
  3. The report loads successfully with no errors displayed.

#### 1.2.Verify indicator displays Y when the DB value is Y
**Prerequisite(s):**
  1. At least one reservation with Syndicate Owner Indicator = Y exists in the selected date range.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Select parameters that cover the reservation prepared above.
  3. Click "Run" to generate the report.
  4. Locate the reservation row and check the Syndicate Owner Indicator cell.
**Test Result(s):**
  1. The Syndicate Owner Indicator cell displays "Y".
  2. The report loads successfully with no errors displayed.

#### 1.3.Verify indicator displays N when the DB value is N
**Prerequisite(s):**
  1. At least one reservation with Syndicate Owner Indicator = N exists in the selected date range.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Select parameters that cover the reservation prepared above.
  3. Click "Run" to generate the report.
  4. Locate the reservation row and check the Syndicate Owner Indicator cell.
**Test Result(s):**
  1. The Syndicate Owner Indicator cell displays "N".
  2. The report loads successfully with no errors displayed.

#### 1.4.Verify indicator displays blank for offline profile reservations with empty indicator
**Prerequisite(s):**
  1. At least one reservation that uses an offline profile with no Syndicate Owner Indicator exists in the selected date range.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Select parameters that cover the reservation prepared above.
  3. Click "Run" to generate the report.
  4. Locate the reservation row and check the Syndicate Owner Indicator cell.
**Test Result(s):**
  1. The Syndicate Owner Indicator cell is left blank.
  2. The report loads successfully with no errors displayed.

### 2.Verify blank display without error for unexpected DB indicator values
**Prerequisite(s):**
  1. The DB field hkjc_resv_members: rmem_syndicate_owner_indicator contains a value other than Y, N, or empty (e.g., null, whitespace, or an unexpected character) for at least one reservation.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Select parameters that cover the reservation prepared above.
  3. Click "Run" to generate the report.
  4. Check the Syndicate Owner Indicator cell of the affected reservation.
  5. Check the indicator cells of the other reservation rows.
**Test Result(s):**
  1. The Syndicate Owner Indicator cell of the affected reservation is left blank.
  2. The report does not error or crash and completes generation normally.
  3. Other reservation rows are displayed with their correct indicator values.

## Report Functional Verification

### 1.Report Access and Output

#### 1.1.Verify Syndicate Owner Indicator is visible to all users who can access the report
**Prerequisite(s):**
  1. Two or more user accounts with permission to access TMS039 exist (e.g., a manager user and a normal report user).
**Step(s):**
  1. Log in with the first user account and open TMS039 Override Date Limitation for Advance Booking.
  2. Generate the report with default parameters.
  3. Verify the Syndicate Owner Indicator column is displayed with correct values.
  4. Repeat steps 1 to 3 with the other user accounts.
**Test Result(s):**
  1. The Syndicate Owner Indicator column is displayed for every user who can currently access the report.
  2. The report loads successfully with no errors displayed.

#### 1.2.Verify Syndicate Owner Indicator appears in exported files (PDF and Excel)
**Prerequisite(s):**
  1. Reservations with different indicator values (Y, N, and empty) exist in the selected date range.
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Generate the report with default parameters.
  3. Select "Export" from the menu and export the report to PDF.
  4. Select "Export" from the menu and export the report to Excel.
  5. Open the exported files and check the Syndicate Owner Indicator column.
**Test Result(s):**
  1. The exported PDF file contains the Syndicate Owner Indicator column to the LEFT of the Syndicate Owner column with correct values.
  2. The exported Excel file contains the Syndicate Owner Indicator column to the LEFT of the Syndicate Owner column with correct values.
  3. The exported files are saved to the specified location successfully.

#### 1.3.Verify Syndicate Owner Indicator appears in the printed report
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Generate the report with default parameters.
  3. Select "Print" from the menu and print the report.
  4. Check the printed output.
**Test Result(s):**
  1. The printed report contains the Syndicate Owner Indicator column to the LEFT of the Syndicate Owner column with correct values.
  2. The printed report is produced successfully with no errors displayed.

## Report Compatibility Verification

### 1.Environment Compatibility

#### 1.1.Support Config Zone Compatibility
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Run the report with parameters covering the current config zone hierarchy level.
  3. Repeat for the sub-hierarchy level, the parent hierarchy level, and all hierarchies including sub-hierarchies.
  4. Verify the Syndicate Owner Indicator column is displayed with correct values at each level.
**Test Result(s):**
  1. The report loads successfully at every config zone hierarchy level.
  2. The Syndicate Owner Indicator column is displayed with correct values at every level.
  3. No errors displayed.

#### 1.2.Support Data Service Compatibility - MySQL and TiDB
**Step(s):**
  1. Open TMS039 Override Date Limitation for Advance Booking.
  2. Select a business date range of less than 7 days and generate the report.
  3. Verify the Syndicate Owner Indicator column and its values.
  4. Select a business date range of more than 7 days and generate the report.
  5. Verify the Syndicate Owner Indicator column and its values.
**Test Result(s):**
  1. The report loads successfully for both date ranges.
  2. The Syndicate Owner Indicator column is displayed with correct values in both cases.
  3. No errors displayed.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
