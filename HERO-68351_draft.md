# HERO-68351 POS Interface - Show message if no spouse or fail to retrieve spouse information in Photo & DoB tab Test Report

## Functional Testing

### 1. No Spouse Record Found

#### 1.1. Display "No Spouse record found in MCRM" When No Spouse Family Member Type Is Found

**Prerequisite(s):**
  1. MCRM membership interface is configured and attached to target payment
  2. A member with suffix "-01" (Principal account) exists in MCRM
  3. The member has no Spouse family member type in MCRM

**Step(s):**
  1. Open a check
  2. Click function "SVC Enquiry"
  3. Perform online member enquiry
  4. Select the target member ending with "-01"
  5. Click tab "Photo & DoB"
  6. Observe the spouse section

**Test Result(s):**
  1. The message "No Spouse record found in MCRM" is displayed in the spouse section
  2. The message is displayed in default font color
  3. Main member information (membership number, DOB, photo) is displayed correctly
  4. No errors are displayed

### 2. Fail to Retrieve Spouse Information

#### 2.1. Prompt Error Message with Retry and Cancel When Member Profile List Enquiry Fails

**Prerequisite(s):**
  1. MCRM membership interface is configured
  2. A member with suffix "-01" exists in MCRM
  3. MCRM API for member profile list enquiry is simulated to return timeout or error

**Step(s):**
  1. Open a check
  2. Click function "SVC Enquiry"
  3. Perform online member enquiry
  4. Select the target member ending with "-01"
  5. Click tab "Photo & DoB"
  6. System triggers external API for member profile list enquiry
  7. Verify error message appears with "Retry" and "Cancel" buttons

**Test Result(s):**
  1. Error message is prompted when member profile list enquiry times out or returns error
  2. "Retry" and "Cancel" buttons are displayed
  3. No crash or unhandled exception occurs

#### 2.2. Click Cancel to Display "Fail to retrieve spouse information"

**Prerequisite(s):**
  1. Continue from 2.1 with error message displayed

**Step(s):**
  1. Click "Cancel" button on the error message
  2. Observe the spouse section in "Photo & DoB" tab

**Test Result(s):**
  1. The error message window is closed
  2. The message "Fail to retrieve spouse information" is displayed in the spouse section
  3. The message is displayed in red font color
  4. Main member information is still displayed correctly

#### 2.3. Click Retry to Re-Trigger Member Profile List Enquiry

**Prerequisite(s):**
  1. Continue from 2.1 with error message displayed

**Step(s):**
  1. Click "Retry" button on the error message
  2. Observe system behavior

**Test Result(s):**
  1. System re-triggers the external API for member profile list enquiry
  2. If the enquiry succeeds, spouse information is displayed normally
  3. If the enquiry fails again, the error message with "Retry" and "Cancel" is shown again

### 3. Fail to Retrieve Spouse Photo

#### 3.1. Prompt Error Message with Retry and Cancel When Spouse Photo Enquiry Fails

**Prerequisite(s):**
  1. MCRM membership interface is configured
  2. A member with suffix "-01" exists and has a Spouse family member in MCRM
  3. MCRM API for member photo enquiry is simulated to return timeout or error for spouse photo

**Step(s):**
  1. Open a check
  2. Click function "SVC Enquiry"
  3. Perform online member enquiry
  4. Select the target member ending with "-01"
  5. Click tab "Photo & DoB"
  6. System retrieves spouse member number successfully
  7. System triggers external API for member photo enquiry
  8. Verify error message appears for spouse photo with "Retry" and "Cancel" buttons

**Test Result(s):**
  1. Error message is prompted when spouse photo enquiry times out or returns error
  2. "Retry" and "Cancel" buttons are displayed
  3. Main member photo is displayed correctly (if main member photo enquiry succeeded)

#### 3.2. Click Cancel to Display "Fail to retrieve spouse photo"

**Prerequisite(s):**
  1. Continue from 3.1 with error message displayed

**Step(s):**
  1. Click "Cancel" button on the error message
  2. Observe the spouse photo area in "Photo & DoB" tab

**Test Result(s):**
  1. The error message window is closed
  2. The message "Fail to retrieve spouse photo" is displayed in the spouse photo area
  3. The message is displayed in red font color
  4. Spouse membership number and DOB are still displayed (if retrieved)

### 4. Fail to Retrieve Main Member Photo

#### 4.1. Display "Fail to retrieve member photo" When Main Member Photo Enquiry Fails

**Prerequisite(s):**
  1. MCRM membership interface is configured
  2. A member exists in MCRM
  3. MCRM API for member photo enquiry is simulated to return timeout or error for main member photo

**Step(s):**
  1. Open a check
  2. Click function "SVC Enquiry"
  3. Perform online member enquiry
  4. Select the target member
  5. Click tab "Photo & DoB"
  6. System triggers external API for member photo enquiry
  7. Verify error message appears for main member photo with "Retry" and "Cancel" buttons
  8. Click "Cancel"
  9. Observe the main member photo area

**Test Result(s):**
  1. Error message is prompted when main member photo enquiry times out or returns error
  2. "Retry" and "Cancel" buttons are displayed
  3. After clicking "Cancel", the message "Fail to retrieve member photo" is displayed in the main member photo area
  4. The message is displayed in red font color
  5. Main member membership number and DOB are still displayed correctly

### 5. Member Payment Settlement - Photo & DoB Tab

#### 5.1. Verify Photo & DoB Tab Messages During Member Payment Settlement

**Prerequisite(s):**
  1. MCRM membership interface is configured and attached to target payment
  2. An old check with items exists
  3. A member type payment method is configured

**Step(s):**
  1. Open an old check
  2. Click function "Paid" to go to cashier
  3. Select the payment attached with MCRM membership interface
  4. Perform online member enquiry
  5. Select target member ending with "-01"
  6. Click tab "Photo & DoB"
  7. Verify the same message behaviors as SVC Enquiry (No Spouse, Fail to Retrieve Spouse Info/Photo, Fail to Retrieve Member Photo)

**Test Result(s):**
  1. All error messages and prompts behave identically to the SVC Enquiry workflow
  2. "No Spouse record found in MCRM" displays when no spouse is found
  3. "Fail to retrieve spouse information" displays in red after Cancel on profile list error
  4. "Fail to retrieve spouse photo" displays in red after Cancel on spouse photo error
  5. "Fail to retrieve member photo" displays in red after Cancel on member photo error

### 6. Offline Member Enquiry

#### 6.1. Verify No Spouse Information Displayed for Offline Enquiry

**Prerequisite(s):**
  1. MCRM membership interface is configured
  2. Offline member enquiry is available

**Step(s):**
  1. Open a check
  2. Click function "SVC Enquiry"
  3. Perform offline member enquiry
  4. Select target member
  5. Click tab "Photo & DoB"
  6. Observe the spouse section

**Test Result(s):**
  1. System follows existing workflow to display information without any spouse information
  2. No spouse-related error messages are displayed
  3. No "Retry"/"Cancel" prompts appear for spouse-related queries
  4. Main member information is displayed per existing offline workflow

## Compatibility Testing

### 7. Browser Compatibility

#### 7.1. Verify Error Messages Display Correctly Across Browsers

**Prerequisite(s):**
  1. MCRM membership interface is configured
  2. Access to Chrome, Firefox, and Edge browsers

**Step(s):**
  1. In Google Chrome, perform SVC Enquiry and trigger each error scenario (No Spouse, Fail to Retrieve Spouse Info/Photo, Fail to Retrieve Member Photo)
  2. Repeat in Mozilla Firefox
  3. Repeat in Microsoft Edge

**Test Result(s):**
  1. All error messages and prompts display correctly in Chrome
  2. All error messages and prompts display correctly in Firefox
  3. All error messages and prompts display correctly in Edge
  4. Red font color for error messages is consistent across browsers
  5. "Retry" and "Cancel" buttons function correctly in all browsers

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
