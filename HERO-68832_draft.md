# HERO-68832 Test Report

## Functional Testing

### F1.Alphanumeric Keyboard Display for Table Number Input in Vertical Mobile View

#### F1.1.Reproduce Missing Buttons on Void Check Alphanumeric Keyboard

**Prerequisite(s):**
1. POS application is installed and running on a supported mobile device (Hi20 Android 13 or iPad/iPhone) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least one check exists in the system that can be voided
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with a valid user account that has Void Check permission
3. Tap the "Void Check" function from the POS main screen
4. On the Table Number input screen, tap "Alphanumeric Table No." to switch to the alphanumeric keyboard
5. Observe the layout and visibility of the alphanumeric keyboard buttons

**Reproduction Result(s):**
1. After switching to the alphanumeric keyboard, only half of the keys are visible
2. The remaining half of the alphanumeric buttons are missing/cut off in the vertical mobile view
3. The user is unable to access or tap the missing alphanumeric characters for table number entry
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, switch to the alphanumeric keyboard in vertical mobile view
2. All alphanumeric keyboard buttons are fully visible and properly laid out within the screen
3. No buttons are clipped, hidden, or cut off in vertical orientation
4. Each alphanumeric key is tappable and registers the expected character in the Table Number field

#### F1.2.Reproduce Missing Buttons on Unlock Table Alphanumeric Keyboard

**Prerequisite(s):**
1. POS application is installed and running on a supported mobile device (Hi20 Android 13 or iPad/iPhone) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least one locked table exists in the system
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with a valid user account that has Unlock Table permission
3. Tap the "Unlock Table" function from the POS main screen
4. On the Table Number input screen, tap "Alphanumeric Table No." to switch to the alphanumeric keyboard
5. Observe the layout and visibility of the alphanumeric keyboard buttons

**Reproduction Result(s):**
1. After switching to the alphanumeric keyboard, only half of the keys are visible
2. The remaining half of the alphanumeric buttons are missing/cut off in the vertical mobile view
3. The user is unable to access or tap the missing alphanumeric characters for table number entry
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, switch to the alphanumeric keyboard in vertical mobile view
2. All alphanumeric keyboard buttons are fully visible and properly laid out within the screen
3. No buttons are clipped, hidden, or cut off in vertical orientation
4. Each alphanumeric key is tappable and registers the expected character in the Table Number field

#### F1.3.Reproduce Missing Buttons on Change Table Alphanumeric Keyboard

**Prerequisite(s):**
1. POS application is installed and running on a supported mobile device (Hi20 Android 13 or iPad/iPhone) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least one open table exists in the system
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with a valid user account that has Change Table permission
3. Tap the "Change Table" function from the POS main screen
4. On the Table Number input screen, tap "Alphanumeric Table No." to switch to the alphanumeric keyboard
5. Observe the layout and visibility of the alphanumeric keyboard buttons

**Reproduction Result(s):**
1. After switching to the alphanumeric keyboard, only half of the keys are visible
2. The remaining half of the alphanumeric buttons are missing/cut off in the vertical mobile view
3. The user is unable to access or tap the missing alphanumeric characters for table number entry
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, switch to the alphanumeric keyboard in vertical mobile view
2. All alphanumeric keyboard buttons are fully visible and properly laid out within the screen
3. No buttons are clipped, hidden, or cut off in vertical orientation
4. Each alphanumeric key is tappable and registers the expected character in the Table Number field

#### F1.4.Reproduce Missing Buttons on Merge Table Alphanumeric Keyboard

**Prerequisite(s):**
1. POS application is installed and running on a supported mobile device (Hi20 Android 13 or iPad/iPhone) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least two open tables exist in the system to support a Merge Table operation
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with a valid user account that has Merge Table permission
3. Tap the "Merge Table" function from the POS main screen
4. On the Table Number input screen, tap "Alphanumeric Table No." to switch to the alphanumeric keyboard
5. Observe the layout and visibility of the alphanumeric keyboard buttons

**Reproduction Result(s):**
1. After switching to the alphanumeric keyboard, only half of the keys are visible
2. The remaining half of the alphanumeric buttons are missing/cut off in the vertical mobile view
3. The user is unable to access or tap the missing alphanumeric characters for table number entry
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, switch to the alphanumeric keyboard in vertical mobile view
2. All alphanumeric keyboard buttons are fully visible and properly laid out within the screen
3. No buttons are clipped, hidden, or cut off in vertical orientation
4. Each alphanumeric key is tappable and registers the expected character in the Table Number field

#### F1.5.Reproduce Missing Buttons on Merge Multiple Table Alphanumeric Keyboard

**Prerequisite(s):**
1. POS application is installed and running on a supported mobile device (Hi20 Android 13 or iPad/iPhone) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least three open tables exist in the system to support a Merge Multiple Table operation
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with a valid user account that has Merge Multiple Table permission
3. Tap the "Merge Multiple Table" function from the POS main screen
4. On the Table Number input screen, tap "Alphanumeric Table No." to switch to the alphanumeric keyboard
5. Observe the layout and visibility of the alphanumeric keyboard buttons

**Reproduction Result(s):**
1. After switching to the alphanumeric keyboard, only half of the keys are visible
2. The remaining half of the alphanumeric buttons are missing/cut off in the vertical mobile view
3. The user is unable to access or tap the missing alphanumeric characters for table number entry
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, switch to the alphanumeric keyboard in vertical mobile view
2. All alphanumeric keyboard buttons are fully visible and properly laid out within the screen
3. No buttons are clipped, hidden, or cut off in vertical orientation
4. Each alphanumeric key is tappable and registers the expected character in the Table Number field

### F2.Alphanumeric Keyboard Layout Consistency Across Target Functions

#### F2.1.Verify Consistent Button Rendering Across All Affected Functions

**Prerequisite(s):**
1. POS application with the fix applied is installed and running on a supported mobile device (Hi20 Android 13, iPad, iPhone 15) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials covering Void Check, Unlock Table, Change Table, Merge Table and Merge Multiple Table permissions
3. At least three open tables and one locked table exist in the system to support all target functions

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with the test user account
3. Open the Void Check function, switch to "Alphanumeric Table No." keyboard and observe the button layout
4. Close and return to the main screen, then open the Unlock Table function, switch to the alphanumeric keyboard and observe the layout
5. Repeat the same observation for Change Table, Merge Table and Merge Multiple Table functions
6. Compare the alphanumeric keyboard layouts rendered across all five target functions

**Fix Result(s):**
1. The alphanumeric keyboard layout is identical and consistent across Void Check, Unlock Table, Change Table, Merge Table and Merge Multiple Table functions
2. All alphanumeric buttons are completely visible and correctly aligned in vertical (portrait) orientation for every target function
3. No function shows clipped, missing, or partially hidden keys after the fix

### F3.Alphanumeric Keyboard Input Functional Verification After Fix

#### F3.1.Verify Alphanumeric Table Number Entry Submits Successfully

**Prerequisite(s):**
1. POS application with the fix applied is installed and running on a supported mobile device (Hi20 Android 13, iPad, iPhone 15) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least one open table and one locked table exist in the system

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with the test user account
3. Open Void Check function, switch to "Alphanumeric Table No." keyboard
4. Tap alphanumeric keys to enter a valid alphanumeric table number
5. Confirm the input and observe the system response
6. Repeat the alphanumeric entry and submission flow for Unlock Table, Change Table, Merge Table and Merge Multiple Table

**Fix Result(s):**
1. Each tapped alphanumeric key registers the corresponding character in the Table Number input field
2. The entered alphanumeric table number is correctly accepted by the system for each target function
3. The system navigates to the expected next step (e.g. check details, table selection confirmation) for each function
4. No input errors, layout distortion or unresponsive buttons are observed after the fix

### F4.Regression Scope for Surrounding Mobile Keyboard Behaviors

#### F4.1.Verify Numeric Keyboard and Other Mobile Keyboard Layouts Remain Functional

**Prerequisite(s):**
1. POS application with the fix applied is installed and running on a supported mobile device (Hi20 Android 13, iPad, iPhone 15) in vertical (portrait) orientation
2. User is able to log in to POS with valid credentials
3. At least one open table and one locked table exist in the system

**Step(s):**
1. Launch POS on the mobile device in vertical (portrait) orientation
2. Log in with the test user account
3. Open Void Check function and verify the default numeric Table Number keyboard is fully visible and functional
4. Switch to the alphanumeric keyboard and confirm it is also fully visible and functional
5. Repeat the verification for Unlock Table, Change Table, Merge Table and Merge Multiple Table
6. Confirm there is no negative impact on other on-screen keyboards (e.g. quantity, discount, search) used elsewhere in POS in vertical mobile view

**Fix Result(s):**
1. The default numeric Table Number keyboard continues to render and function correctly in vertical mobile view
2. The alphanumeric keyboard now also renders and functions correctly after the fix
3. Other on-screen keyboards (quantity, discount, search, etc.) are unaffected by the fix and remain fully visible and operational
4. No new layout regressions are introduced in vertical (portrait) orientation

## Test Environment Information

- **Test Environment**: Local test environment
- **Test Devices**: Workstation (Windows 11 Enterprise), Hi20 (Android 13), iPad (OS 27), iPhone 15 (OS 27)
- **Test Orientation**: Vertical (Portrait) mobile view
- **Component**: POS
- **Issue Type**: Bug
- **QC Tester**: Orianna Wen (orianna.wen@shijigroup.com)
- **QC Reviewer**: Kiet Zou (kiet.zou@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-68832)

In vertical mode, half of the buttons of the alphanumeric keyboard are missing when entering the table number for the functions below:

- Void Check
- Unlock Table
- Change Table
- Merge Table
- Merge Multiple Table

Steps to Reproduce:

- Log into POS in vertical mode
- Click the target function
- Click "Alphanumeric Table No." to switch to the alphanumeric keyboard

Actual Result: Half of the keys are missing for Void Check, Unlock Table, Change Table, Merge Table, Merge Multiple Table.

Expected Result: All keys should be displayed correctly.

Release Notes:
- Bug Fixing: [Jira-68832] Buttons of the alphanumeric keyboard for inputting table number screen are missing in mobile view (Steven)
- It is only available in vertical mobile view
- The alphanumeric keyboard should show all buttons
- It is fixed in current version
- Setup: None
