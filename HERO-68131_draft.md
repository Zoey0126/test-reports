# Report - TMS037 Master Pre-Race Booking List with Contact Information - Add Syndicate Owner Indicator field Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Master Pre-Race Booking List with Contact Information |
| Report Code | TMS037 |
| JIRA Issue | HERO-68131 |
| Component | REPORTS |
| Issue Type | Improvement |
| Data Source Note | Syndicate Owner Indicator stored on reservation member record hkjc_resv_members: rmem_syndicate_owner_indicator |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Race Meeting Dates | Race meeting date range of the bookings (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Today, Yesterday, Specified as Below | Specified as Below |
| Begin Date / End Date | Race meeting date range boundaries, enabled when Race Meeting Dates = 'Specified as Below' | User Defined | - |
| Shops/Outlets | Shops and outlets to list bookings for (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Reserve Channel | Reservation channel filter (multi-select) | All, Single, Multiple | All |
| Reserve Type | Reservation type filter (multi-select) | All, Single, Multiple | All |
| Reserve Method | Reservation method filter (multi-select) | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Syndicate Owner Indicator | New display-only column placed immediately to the left of the Syndicate Owner column (between Horse Owner and Syndicate Owner). Display values: Y = Yes, N = No, blank = no value (older or legacy reservations and offline profiles with no indicator). No sorting, grouping or filtering behaviour. Visible to all users who can access the report | Source: hkjc_resv_members.rmem_syndicate_owner_indicator |
| Horse Owner | Existing column displayed to the left of the new Syndicate Owner Indicator column | - |
| Syndicate Owner | Existing column; the new indicator column is placed immediately to its left | - |

## Report Data Accuracy Verification

### 1.Indicator Column Display Verification

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. Reservations with Syndicate Owner Indicator values exist for the selected race meeting date range.
**Step(s):**
  1. Open Reports and run TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select "default" from all parameters.
    2.1. Select Specified as Below from Race Meeting Dates and enter the prepared date range.
    2.2. Select All Outlets from Shops/Outlets.
    2.3. Select All from Reserve Channel.
    2.4. Select All from Reserve Type.
    2.5. Select All from Reserve Method.
  3. Click "Run" button.
  4. Verify the new Syndicate Owner Indicator column is displayed immediately to the left of the Syndicate Owner column.
  5. Verify the indicator values are populated from the reservation syndicate owner indicator data.
**Test Result(s):**
  1. Report loads successfully.
  2. The Syndicate Owner Indicator column appears between Horse Owner and Syndicate Owner.
  3. Indicator values match the reservation member record data for each reservation.
  4. No errors displayed.

#### 1.2.[A Single Outlet, Specified Race Meeting Dates] Indicator Values Y and N

**Prerequisite(s):**
  1. For one outlet and a specified race meeting date range, reservations with indicator value Y and reservations with indicator value N exist.
**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select only that single outlet.
  3. Set Race Meeting Dates to Specified as Below and enter the prepared date range.
  4. Run the report.
  5. Compare the indicator shown for each reservation with the rmem_syndicate_owner_indicator value on the reservation member record.
**Test Result(s):**
  1. Reservations with indicator Y show Y in the Syndicate Owner Indicator column.
  2. Reservations with indicator N show N in the Syndicate Owner Indicator column.
  3. Indicator values match the reservation member record for every listed reservation.
  4. No errors displayed.

#### 1.3.Legacy Reservations Without Indicator Value

**Prerequisite(s):**
  1. Older or legacy reservations with no rmem_syndicate_owner_indicator value exist within the selected date range.
**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select the date range that includes the legacy reservations without indicator value.
  3. Run the report.
  4. Verify the Syndicate Owner Indicator cell for the legacy reservations.
**Test Result(s):**
  1. The Syndicate Owner Indicator cell is left blank for reservations with no indicator value.
  2. The rest of the row (Syndicate Owner and other columns) still displays correctly.
  3. No error values such as 0 or null text are shown in the indicator cell.
  4. No errors displayed.

#### 1.4.Mixed Indicator Values in One Report

**Prerequisite(s):**
  1. The selected date range contains reservations with indicator Y, indicator N and reservations without indicator value.
**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select the date range covering all three kinds of reservations.
  3. Run the report.
  4. Verify that Y, N and blank indicator cells are all displayed correctly in the same report output.
**Test Result(s):**
  1. Y rows, N rows and blank rows are all displayed with their correct indicator values.
  2. Column position of the indicator stays immediately left of Syndicate Owner for all rows.
  3. Report completes without errors.

### 2.Column Position Between Horse Owner and Syndicate Owner

**Prerequisite(s):**
  1. Reservations with and without indicator values exist for the selected parameters.
**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select the usual parameters and generate the report.
  3. Verify the column order in the report header.
**Test Result(s):**
  1. The Syndicate Owner Indicator column is displayed immediately to the left of the Syndicate Owner column and to the right of Horse Owner.
  2. No existing column is removed or renamed.
  3. No errors displayed.

## Report Functional Verification

### 1.Export and Print Verification

#### 1.1.Export to Excel Shows Indicator Column

**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Select the parameters that return reservations with indicator values.
  3. Run the report.
  4. Select "Export" from the menu and choose Excel (.xlsx).
  5. Open the exported file and verify the Syndicate Owner Indicator column.
**Test Result(s):**
  1. Exported file is successful.
  2. The Syndicate Owner Indicator column appears in the exported file at the same position as on screen.
  3. Indicator values Y, N and blank match the on-screen report.
  4. No errors displayed.

#### 1.2.Print and On-Screen Preview Show Indicator Column

**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information.
  2. Run the report with the prepared parameters.
  3. Verify the on-screen preview shows the Syndicate Owner Indicator column.
  4. Select "Print" from the menu and print the report to PDF.
  5. Verify the printed output shows the Syndicate Owner Indicator column.
**Test Result(s):**
  1. On-screen preview shows the Syndicate Owner Indicator column with correct values.
  2. Printed copy shows the Syndicate Owner Indicator column with correct values.
  3. Blank indicator cells stay blank in both formats.
  4. No errors displayed.

### 2.Display-Only Behaviour and Role Visibility

**Prerequisite(s):**
  1. A user account with normal report access (no admin role) can run TMS037.
**Step(s):**
  1. Run TMS037 with the prepared parameters.
  2. Try to sort, group and filter by the Syndicate Owner Indicator column.
  3. Log in with the normal report user and run TMS037 again.
**Test Result(s):**
  1. The Syndicate Owner Indicator field is display-only with no sorting, grouping or filtering behaviour applied.
  2. The column is visible to all users who can access the report with no role-based restrictions.
  3. Existing behaviour of the other columns is unchanged.
  4. No errors displayed.

### 3.Previous Preset Parameters Still Run

**Prerequisite(s):**
  1. A preset parameter for TMS037 created before this enhancement exists.
**Step(s):**
  1. Go to Preset Parameters and select the previous TMS037 preset.
  2. Run the preset parameters.
  3. Verify the report output.
**Test Result(s):**
  1. Previous preset parameters run successfully without migration or error.
  2. The Syndicate Owner Indicator column appears in the output of the previous preset.
  3. Data displayed is correct for the preset parameters.
  4. No errors displayed.

### 4.Multi-Language Support

**Step(s):**
  1. Select "Language" and switch the user language to another supported language.
  2. Open TMS037 Master Pre-Race Booking List with Contact Information.
  3. Run the report with the prepared parameters.
  4. Verify the report header, labels and data display in the selected language.
**Test Result(s):**
  1. Language is set successfully.
  2. The Syndicate Owner Indicator column header and data display correctly in the selected language.
  3. No garbled characters are shown.
  4. No errors displayed.

## Report Compatibility Verification

### 1.Data Service Compatibility - MsSQL

**Step(s):**
  1. Open TMS037 Master Pre-Race Booking List with Contact Information on the SQL Server environment.
  2. Select parameters from the Report Data Accuracy Verification cases and set the race meeting date range to cover reservations with Y, N and blank indicator values.
  3. Run the report and verify the indicator column.
**Test Result(s):**
  1. Report loads successfully on the SQL Server data service.
  2. Syndicate Owner Indicator values are correct for all listed reservations.
  3. No errors displayed.

## Test Environment

- **Version**: Sprint build containing the HERO-68131 enhancement
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (SQL Server), ER Test Environment
