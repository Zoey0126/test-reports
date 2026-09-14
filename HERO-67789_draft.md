# TMS015 report - Support to show Generate Type of SMS and destination Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Pre-Race Booking List |
| Code | TMS015 |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Shops and outlets included in the report (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business date range of the bookings (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Yesterday, Today, Specified as Below | Yesterday |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Generate Type | New column indicating how the SMS was generated: System for auto-sent SMS, Manual for manually-sent SMS, from the TMS booking records | - |
| Destination Phone Number | Shows the actual receiving phone number of the SMS instead of the reservation record phone number when the SMS is triggered manually, referring to the destination in the message record | - |
| Phone Number | Shows the reservation record phone number for auto-sent SMS records | - |

## Report Data Accuracy Verification

### 1.Generate Type Display

#### 1.1.Verify Generate Type displays System for auto-sent SMS records
**Prerequisite(s):**
  1. At least one booking record with a system auto-sent SMS exists in the selected date range.
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Locate the auto-sent SMS record and check the Generate Type column.
**Test Result(s):**
  1. The Generate Type column displays "System" for the auto-sent SMS record.
  2. The report loads successfully with no errors displayed.

#### 1.2.Verify Generate Type displays Manual for manually-sent SMS records
**Prerequisite(s):**
  1. At least one manual SMS was sent against an existing booking record (the user manually entered a phone number and sent the SMS).
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Locate the manually-sent SMS record and check the Generate Type column.
**Test Result(s):**
  1. The Generate Type column displays "Manual" for the manually-sent SMS record.
  2. The report loads successfully with no errors displayed.

### 2.Destination Phone Number Display

#### 2.1.Verify the actual destination phone number is displayed for manually triggered SMS
**Prerequisite(s):**
  1. A manual SMS was sent to a phone number different from the reservation record phone number, and the destination is stored in the message record.
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Locate the record of the manual SMS and check the destination phone number.
**Test Result(s):**
  1. The report shows the actual receiving phone number used for the manual SMS (the destination in the message record) instead of the reservation record phone number.
  2. The Generate Type column displays "Manual" for the same record.
  3. The report loads successfully with no errors displayed.

#### 2.2.Verify the reservation record phone number is displayed for auto-sent SMS
**Prerequisite(s):**
  1. At least one booking record with a system auto-sent SMS exists in the selected date range.
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Generate the report with default parameters.
  3. Locate the auto-sent SMS record and check the phone number.
**Test Result(s):**
  1. The report shows the reservation record phone number for the auto-sent SMS record.
  2. The Generate Type column displays "System" for the same record.
  3. The report loads successfully with no errors displayed.

## Report Functional Verification

### 1.Verify the new columns appear in exported and printed report outputs
**Prerequisite(s):**
  1. Both auto-sent and manually-sent SMS records exist in the selected date range.
**Step(s):**
  1. Open TMS015 Pre-Race Booking List and generate the report with default parameters.
  2. Select "Export" from the menu and export the report to PDF.
  3. Select "Export" from the menu and export the report to Excel.
  4. Select "Print" from the menu and print the report.
  5. Check the Generate Type and destination phone number columns in each output.
**Test Result(s):**
  1. The Generate Type and destination phone number columns appear in the exported PDF and Excel files with correct values.
  2. The Generate Type and destination phone number columns appear in the printed report with correct values.
  3. All outputs are generated successfully with no errors displayed.

## Report Compatibility Verification

### 1.Environment Compatibility

#### 1.1.Support Config Zone Compatibility
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Run the report with parameters covering the current config zone hierarchy level.
  3. Repeat for the sub-hierarchy level, the parent hierarchy level, and all hierarchies including sub-hierarchies.
  4. Verify the Generate Type and destination phone number columns at each level.
**Test Result(s):**
  1. The report loads successfully at every config zone hierarchy level.
  2. The Generate Type and destination phone number columns are displayed with correct values at every level.
  3. No errors displayed.

#### 1.2.Support Data Service Compatibility - MySQL and TiDB
**Step(s):**
  1. Open TMS015 Pre-Race Booking List.
  2. Select a business date range of less than 7 days and generate the report.
  3. Verify the Generate Type and destination phone number columns.
  4. Select a business date range of more than 7 days and generate the report.
  5. Verify the Generate Type and destination phone number columns.
**Test Result(s):**
  1. The report loads successfully for both date ranges.
  2. The Generate Type and destination phone number columns are displayed with correct values in both cases.
  3. No errors displayed.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
