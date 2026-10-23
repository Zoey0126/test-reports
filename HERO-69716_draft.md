# TMS Can View Aspen Member Photo on Edit Booking Test Report

## Functional Testing

### 1.Aspen Member Photo Display

#### 1.1.Verify Aspen Member Photo Field Is Displayed on Reservation Record
**Prerequisite(s):**
1. TMS Operation is available and the user can log in
2. The Aspen Interface is configured and available
3. The target member exists in Aspen with a photo captured

**Step(s):**
1. Go to TMS Operation > New Reservation page
2. Search for the member through the Aspen Interface
3. Save the reservation with the Aspen member
4. Go to the Current View page and open the newly created reservation
5. Open the "Reservation Record" page and switch to the "Membership Information" tab
6. Locate the "Aspen Member Photo" field under the Member Status field

**Test Result(s):**
1. The "Reservation Record" page displays the "Membership Information" tab by default for a reservation saved with an Aspen member
2. The new field "Aspen Member Photo" is displayed under the Member Status field
3. The field shows the photo icon and does not display the member photo until the icon is clicked

#### 1.2.Verify Aspen Member Photo Is Captured When Clicking the Photo Icon
**Prerequisite(s):**
1. A reservation saved with an Aspen member is opened
2. The Aspen member has a photo stored in the Aspen system
3. The network connection to the Aspen Interface is stable

**Step(s):**
1. Open the reservation record of the Aspen member
2. Click the photo icon of the "Aspen Member Photo" field
3. Wait for the photo capture to complete
4. Review the displayed photo with the member record in Aspen
5. Refresh the page and click the photo icon again

**Test Result(s):**
1. The photo of the Aspen member is captured through the Aspen Interface and displayed
2. The displayed photo matches the member photo in the Aspen system
3. A loading indicator is shown while the photo is being captured
4. The photo is displayed again after the page is refreshed

#### 1.3.Verify Reload Button Is Provided When Photo Capture Fails
**Prerequisite(s):**
1. A reservation saved with an Aspen member is opened
2. The network connection to the Aspen Interface can be interrupted or is unstable

**Step(s):**
1. Open the reservation record of the Aspen member
2. Disconnect the network or simulate a network fluctuation
3. Click the photo icon of the "Aspen Member Photo" field
4. Observe the message and the reload button
5. Restore the network and click the reload button

**Test Result(s):**
1. A reload button is provided when the Aspen photo cannot be captured due to network fluctuation or other reasons
2. Clicking the reload button requests the Aspen photo again
3. The photo is displayed successfully after the network is restored
4. When "Photo not found" is displayed, the message is shown clearly and the member may not have a photo in Aspen

#### 1.4.Verify Aspen Member Photo Field Is Hidden for Non Aspen Member
**Prerequisite(s):**
1. A reservation saved with a non Aspen membership is opened
2. The reservation record page is accessible

**Step(s):**
1. Open the reservation record of a non Aspen member reservation
2. Switch to the "Membership Information" tab
3. Observe whether the "Aspen Member Photo" field is displayed

**Test Result(s):**
1. The "Aspen Member Photo" field is not displayed for a reservation that is not saved with an Aspen membership
2. The existing membership information is displayed normally

#### 1.5.Verify Member Photo Display When Switching Membership Options
**Prerequisite(s):**
1. A reservation saved with an Aspen member is opened in the Edit Reservation page
2. Another non Aspen membership option is available

**Step(s):**
1. Open the reservation in the Edit Reservation page
2. Click the Member Photo icon to display the Aspen member photo
3. Switch "Membership Options" to a non Aspen Interface
4. Observe the displayed member photo
5. Switch back to the Aspen Interface
6. Observe the field and the reload button when the loading fails

**Test Result(s):**
1. The Aspen member photo is not displayed after switching to a non Aspen Interface
2. The Member Photo field follows the non Aspen membership behaviour
3. When switching back to Aspen, the Aspen member photo is displayed again
4. If the loading fails, the reload button is displayed and clicking it reloads the photo

#### 1.6.Verify Aspen Member Photo Is Not Affected by Theme Member Photo Settings
**Prerequisite(s):**
1. The theme has Member Photo width and height settings configured
2. A reservation saved with an Aspen member is opened

**Step(s):**
1. Note the Member Photo width and height settings in the theme
2. Open the reservation record and display the Aspen member photo
3. Compare the displayed Aspen photo with the theme Member Photo configuration
4. Change the theme Member Photo settings and reopen the reservation record

**Test Result(s):**
1. The "Aspen Member Photo" field is not affected by the "Member Photo" settings in the theme
2. The Aspen member photo is displayed with its own size settings
3. Changing the theme Member Photo settings does not change the Aspen member photo display

#### 1.7.Verify UI Type Fixed Display
**Prerequisite(s):**
1. The UI Type is set to "Fixed" in the reservation page configuration
2. A reservation saved with an Aspen member is opened

**Step(s):**
1. Set the UI Type to "Fixed"
2. Open the reservation record and the Edit Reservation page
3. Display the Aspen member photo
4. Switch the membership option and display the photo again

**Test Result(s):**
1. The "Aspen Member Photo" field is displayed correctly when the UI Type is "Fixed"
2. The photo icon and the reload button behave the same as in the non fixed UI Type
3. No layout issue occurs on the Membership Information tab

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- New Reservation page member search through Aspen Interface
- Reservation Record page Membership Information tab
- Edit Reservation page Member Photo field for non Aspen memberships
- Membership Options switching between Aspen and other interfaces
- Theme Member Photo width and height settings
- Reservation save, modify and cancel workflows
- Existing member photo display for other membership interfaces

## Compatibility Testing

### 1.Browser Compatibility For TMS Operation
#### 1.1.Verify Aspen Member Photo Display In Supported Browsers
**Prerequisite(s):**
1. TMS is deployed in the local, HQ and HKJC test environments
2. A reservation with an Aspen member photo is available

**Step(s):**
1. Open the reservation record and the Edit Reservation page in Chrome, Firefox, Microsoft Edge and Safari
2. Click the photo icon and review the Aspen member photo
3. Switch the membership option and review the photo display
4. Trigger the reload button by interrupting the network

**Test Result(s):**
1. The Aspen member photo is displayed correctly in all supported browsers
2. The photo icon layout and the reload button are displayed without issue in all supported browsers
3. No console error occurs in any supported browser

## Test Environment Information

- Local test environment
- HQ test environment
- HKJC test environment
