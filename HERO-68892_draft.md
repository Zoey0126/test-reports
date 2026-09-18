# HERO-68892 HKJC - TMS - Background Job Fail Notification not shown when Command is a supported HKJC export job Test Report

## Functional Testing

### 1. Background Job Add Screen - Fail Notification Visibility

#### 1.1. Verify Fail Notification is Shown for Supported HKJC Command with Custom Key

**Prerequisite(s):**
  1. TMS module is installed and configured
  2. HKJC Table Occupancy export Command is available

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Click "Add New"
  3. Enter any custom Key value (e.g., "CustomExport1")
  4. Select a supported HKJC Command (e.g., Table Occupancy export)
  5. Observe whether the Fail Notification email group field is displayed

**Test Result(s):**
  1. The Fail Notification field is displayed when the Command is a supported HKJC export job
  2. The Fail Notification field is visible regardless of the Key value entered
  3. No errors are displayed

#### 1.2. Verify Fail Notification is Shown for Table Availability Export Command

**Prerequisite(s):**
  1. TMS module is installed and configured
  2. HKJC Table Availability export Command is available

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Click "Add New"
  3. Enter any custom Key value
  4. Select Table Availability export as the Command
  5. Observe whether the Fail Notification field is displayed

**Test Result(s):**
  1. The Fail Notification field is displayed for Table Availability export Command
  2. The field is visible regardless of the Key value
  3. No errors are displayed

#### 1.3. Verify Fail Notification is Shown for Unsuccessful Waitlist Notification Command

**Prerequisite(s):**
  1. TMS module is installed and configured
  2. Unsuccessful Waitlist notification Command is available

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Click "Add New"
  3. Enter any custom Key value
  4. Select Unsuccessful Waitlist notification as the Command
  5. Observe whether the Fail Notification field is displayed

**Test Result(s):**
  1. The Fail Notification field is displayed for Unsuccessful Waitlist notification Command
  2. The field is visible regardless of the Key value
  3. No errors are displayed

#### 1.4. Verify Fail Notification is Hidden for Non-Supported Command

**Prerequisite(s):**
  1. A non-HKJC or non-supported Command is available

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Click "Add New"
  3. Enter any Key value
  4. Select a non-supported Command (e.g., a standard non-HKJC job)
  5. Observe whether the Fail Notification field is displayed

**Test Result(s):**
  1. The Fail Notification field is NOT displayed for non-supported Commands
  2. The field is correctly hidden based on Command type
  3. No errors are displayed

#### 1.5. Verify Fail Notification is Hidden When Command is Empty

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Click "Add New"
  3. Enter any Key value
  4. Do NOT select any Command (leave empty)
  5. Observe whether the Fail Notification field is displayed

**Test Result(s):**
  1. The Fail Notification field is NOT displayed when Command is empty
  2. An empty Command does not incorrectly show Fail Notification for every job
  3. No errors are displayed

### 2. Background Job Edit Screen - Fail Notification Visibility

#### 2.1. Verify Fail Notification is Shown When Editing Supported HKJC Job

**Prerequisite(s):**
  1. An existing Background Job with a supported HKJC Command (e.g., Table Occupancy export) exists

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Open the existing supported HKJC job in Edit mode
  3. Verify the Fail Notification field is displayed
  4. Modify the Fail Notification email group
  5. Click Save

**Test Result(s):**
  1. The Fail Notification field is displayed in Edit mode for supported HKJC jobs
  2. The Fail Notification email group can be modified
  3. Changes are saved successfully
  4. No errors are displayed

#### 2.2. Verify Fail Notification is Hidden When Editing Non-Supported Job

**Prerequisite(s):**
  1. An existing Background Job with a non-supported Command exists

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Open the existing non-supported job in Edit mode
  3. Verify the Fail Notification field is NOT displayed

**Test Result(s):**
  1. The Fail Notification field is NOT displayed in Edit mode for non-supported jobs
  2. The field is correctly hidden based on Command type
  3. No errors are displayed

### 3. Background Job View Screen - Fail Notification Visibility

#### 3.1. Verify Fail Notification is Shown When Viewing Supported HKJC Job

**Prerequisite(s):**
  1. An existing Background Job with a supported HKJC Command exists

**Step(s):**
  1. Navigate to System Management > Background Jobs
  2. Open the existing supported HKJC job in View mode
  3. Verify the Fail Notification field is displayed
  4. Verify the Fail Notification email group value is correct

**Test Result(s):**
  1. The Fail Notification field is displayed in View mode for supported HKJC jobs
  2. The Fail Notification email group value is shown correctly
  3. No errors are displayed

### 4. Fail Notification Email Functionality

#### 4.1. Verify Fail Notification Email is Sent When Supported HKJC Job Fails

**Prerequisite(s):**
  1. A supported HKJC Background Job is configured with Fail Notification email group
  2. The job is configured to fail (e.g., invalid export parameters or simulate failure)

**Step(s):**
  1. Trigger the supported HKJC Background Job
  2. Allow the job to fail
  3. Check the Fail Notification email group inbox

**Test Result(s):**
  1. The Background Job fails as expected
  2. A fail notification email is sent to the configured email group
  3. The email contains relevant failure information
  4. No errors are displayed

## Compatibility Testing

### 5. Browser Compatibility

#### 5.1. Verify Fail Notification Field Visibility Across Browsers

**Prerequisite(s):**
  1. Access to Chrome, Firefox, and Edge browsers
  2. TMS module is accessible

**Step(s):**
  1. In Google Chrome, navigate to Background Jobs > Add New, select a supported HKJC Command, verify Fail Notification is displayed
  2. Repeat in Mozilla Firefox
  3. Repeat in Microsoft Edge

**Test Result(s):**
  1. Fail Notification field is displayed correctly in Chrome
  2. Fail Notification field is displayed correctly in Firefox
  3. Fail Notification field is displayed correctly in Edge
  4. Field visibility behavior is consistent across browsers

## Test Environment
- **Version**: v1.2.116.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment
