# HERO-68873 [Tech] TMS2.0 - TMS - Fix error.log with array key "start_time" and "end_time" for TMS Module - V114 Test Report

## Functional Testing

### 1. Error Log Fix Verification - No Undefined Array Key Warnings

#### 1.1. Verify No "Undefined array key start_time" Error When Config by Date Has Advanced Allotment Settings

**Prerequisite(s):**
  1. TMS 2.0 is installed and configured
  2. Go to Table Management System > Basic Setup > Config by Date page
  3. Add settings to the "Advanced Allotment Settings" field in the "Reservation Setting" tab
  4. Switch to the "Period Segment Allotment" tab and add the "Period Segment Allotment (Online)" setting
  5. Go to the outlet during this time period

**Step(s):**
  1. Navigate to Table Management System > Basic Setup > Config by Date
  2. Select an outlet and date
  3. Open the "Reservation Setting" tab
  4. Add or verify "Advanced Allotment Settings" values (including start_time and end_time)
  5. Switch to "Period Segment Allotment" tab
  6. Add "Period Segment Allotment (Online)" setting
  7. Save the configuration
  8. Go to the TMS Operation module
  9. Select the outlet and date configured above
  10. Open New Reservation form
  11. Check the error.log file at the TMS module log location

**Test Result(s):**
  1. No "Undefined array key start_time" warning appears in error.log
  2. No "Undefined array key end_time" warning appears in error.log
  3. The Config by Date settings are saved successfully
  4. The New Reservation form loads without errors
  5. No errors are displayed

#### 1.2. Verify No Error Log Warnings When Creating Online Reservation with Period Segment Allotment

**Prerequisite(s):**
  1. Config by Date is set up with Advanced Allotment Settings and Period Segment Allotment (Online) as per 1.1

**Step(s):**
  1. Access the online reservation portal for the configured outlet and date
  2. Create a new online reservation
  3. Complete the reservation
  4. Check the error.log file

**Test Result(s):**
  1. No "Undefined array key start_time" warning appears in error.log
  2. No "Undefined array key end_time" warning appears in error.log
  3. The online reservation is created successfully
  4. No other unexpected warnings or errors in the log
  5. No errors are displayed

### 2. Regression - Config by Date Functionality

#### 2.1. Verify Config by Date Reservation Settings Work Correctly After Fix

**Prerequisite(s):**
  1. Config by Date is configured with Advanced Allotment Settings including start_time and end_time values

**Step(s):**
  1. Navigate to Table Management System > Basic Setup > Config by Date
  2. Select an outlet and date
  3. Open the "Reservation Setting" tab
  4. Verify the "Advanced Allotment Settings" are displayed correctly
  5. Verify start_time and end_time values are correct
  6. Switch to "Period Segment Allotment" tab
  7. Verify the "Period Segment Allotment (Online)" settings are displayed correctly
  8. Modify a value and save
  9. Reopen the configuration and verify the saved values

**Test Result(s):**
  1. All Config by Date settings display correctly
  2. start_time and end_time values are saved and retrieved correctly
  3. Period Segment Allotment (Online) settings work as expected
  4. No data loss or corruption
  5. No errors are displayed

#### 2.2. Verify Reservation Quota Calculation Works with Period Segment Allotment

**Prerequisite(s):**
  1. Config by Date is configured with Period Segment Allotment settings

**Step(s):**
  1. Go to TMS Operation
  2. Select the configured outlet and date
  3. Open New Reservation form
  4. Verify the reservation quota is calculated based on Period Segment Allotment settings
  5. Create a reservation and verify quota is deducted correctly

**Test Result(s):**
  1. Reservation quota is calculated correctly based on Period Segment Allotment settings
  2. start_time and end_time are properly used in quota calculation
  3. Quota is deducted correctly after reservation creation
  4. No errors are displayed

### 3. Error Log Cleanliness

#### 3.1. Verify No New Error Log Entries After Fix

**Prerequisite(s):**
  1. The TMS module error.log is accessible

**Step(s):**
  1. Clear or note the current error.log content
  2. Perform various TMS operations (view Config by Date, create online reservation, view Operation form)
  3. Check the error.log after operations

**Test Result(s):**
  1. No "Undefined array key start_time" entries in error.log
  2. No "Undefined array key end_time" entries in error.log
  3. No new unexpected error or warning entries
  4. The error.log remains clean after all operations
  5. No errors are displayed

## Test Environment
- **Version**: v1.2.116.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment
