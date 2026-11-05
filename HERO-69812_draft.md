# HERO-69812 Test Report

## Functional Testing

### 1. Footer Clock on Login Screen and After Login

#### 1.1. Footer Clock on Login Screen Displays Date, Weekday and Time

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The HKJC TMS Operation login screen is accessible.
3. The browser developer console is available for checking client-side script errors.

**Step(s):**
1. Open the HKJC TMS Operation login screen without logging in.
2. Check the footer clock at the bottom right of the login screen.
3. Compare the displayed date with the current system date.
4. Verify that the date is shown in the format YYYY-MM-DD (Weekday) HH:MM AM/PM, for example 2026-10-22 (Thursday) 11:48 AM.
5. Check the browser developer console for script errors related to date handling.

**Reproduction Result(s):**
1. Before the fix, the HKJC TMS Operation front-end bundle contained moment.js 2.30.1, which the SSDLC dependency check reported as an outdated library version.
2. The login screen opened and the footer clock displayed a date, weekday and time.
3. The reported defect was the dependency compliance finding rather than a visible failure on the login screen.

**Fix Result(s):**
1. The deployed front-end bundle contains moment.js 2.31.0 and the SSDLC dependency check no longer reports the outdated library version.
2. The footer clock on the login screen shows today's date, weekday and time.
3. The displayed date matches the current system date and the time is shown with the AM/PM indicator.
4. The footer area is not blank and no script error appears on the login screen.
5. The browser developer console shows no script error related to date handling.

#### 1.2. Footer Clock After Login Displays Date, Weekday and Time

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. A valid HKJC TMS operator account exists for the outlet under test.
3. The browser developer console is available for checking client-side script errors.

**Step(s):**
1. Log in to HKJC TMS Operation.
2. Check the footer clock at the bottom right of the operation screen.
3. Compare the displayed date with the current system date.
4. Verify the displayed weekday and time.
5. Check the browser developer console for script errors related to date handling.

**Reproduction Result(s):**
1. Before the fix, the operation screen was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. After login, the footer clock displayed a date, weekday and time.

**Fix Result(s):**
1. After login, the footer clock shows today's date, weekday and time.
2. The displayed date matches the current system date and the time is shown with the AM/PM indicator.
3. The footer area is not blank and no script error appears on screen after login.
4. The browser developer console shows no script error related to date handling.

#### 1.3. Footer Clock Updates Every Second

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to HKJC TMS Operation.
3. The footer clock is visible at the bottom right of the screen.

**Step(s):**
1. Observe the time shown in the footer clock.
2. Watch the footer clock continuously for at least ten seconds.
3. Confirm that the displayed time advances every second.

**Reproduction Result(s):**
1. Before the fix, the footer clock was rendered by moment.js 2.30.1 and was flagged by the SSDLC dependency check.
2. The clock advanced on the screen, but the bundled library version failed the dependency check.

**Fix Result(s):**
1. The seconds displayed in the footer clock change every second.
2. The date and weekday remain unchanged while the time advances.
3. The clock keeps updating without requiring a page refresh.
4. No script error appears on screen while the clock updates.

#### 1.4. Login Screen and Operation Screen Load Without Script Error

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The user has valid credentials for the outlet under test.
3. The browser developer console is available for checking client-side script errors.

**Step(s):**
1. Open the HKJC TMS Operation login screen and check the screen and browser developer console for script errors.
2. Log in to HKJC TMS Operation and check the screen and browser developer console for script errors.
3. Confirm that no date-related script error is reported on either screen.

**Reproduction Result(s):**
1. Before the fix, the front-end bundle contained moment.js 2.30.1, which the SSDLC dependency check reported as an outdated library version.
2. The login screen and the operation screen loaded and displayed their date and time values.
3. No visible page failure was observed, but the dependency check failed on the bundled library version.

**Fix Result(s):**
1. The login screen loads without any script error shown on screen or reported in the browser developer console.
2. The operation screen loads after login without any script error shown on screen or reported in the browser developer console.
3. No date-related script error is reported on either screen.
4. The deployed front-end bundle contains moment.js 2.31.0 and passes the SSDLC dependency check.

### 2. Booking and Reservation Form Date and Time Display

#### 2.1. Booking Date Displays a Valid and Readable Date

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to HKJC TMS Operation.
3. A booking or reservation form is available for the outlet under test.

**Step(s):**
1. Open a booking or reservation form.
2. Check the booking date shown on the form.
3. Compare the booking date with the date expected for the booking.
4. Click "Cancel" without saving.

**Reproduction Result(s):**
1. Before the fix, the booking date was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The booking date was displayed on the booking or reservation form.

**Fix Result(s):**
1. The booking date is displayed as a valid and readable date.
2. The displayed date matches the date expected for the booking.
3. The booking date area is not blank and no script error appears on the form.
4. Clicking "Cancel" closes the form without saving.

#### 2.2. Reservation Time Displays in 12-Hour Format

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to HKJC TMS Operation.
3. A reservation form containing a reservation time is available for the outlet under test.

**Step(s):**
1. Open the reservation form.
2. Check the reservation time shown on the form, for example a time shown as 06:30 pm.
3. Verify that the reservation time is valid and readable.
4. Click "Cancel" without saving.

**Reproduction Result(s):**
1. Before the fix, the reservation time was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The reservation time was displayed on the reservation form in a readable form.

**Fix Result(s):**
1. The reservation time is displayed in the 12-hour format with the AM/PM indicator, for example 06:30 pm.
2. The displayed time matches the reservation time stored for the booking.
3. The reservation time area is not blank and no script error appears on the form.
4. Clicking "Cancel" closes the form without saving.

#### 2.3. Cancel Without Saving Leaves the Booking Unchanged

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to HKJC TMS Operation.
3. An existing booking or reservation is available and its booking date and reservation time are recorded.

**Step(s):**
1. Open the booking or reservation form of the existing booking.
2. Note the booking date and reservation time shown on the form.
3. Click "Cancel" without saving.
4. Reopen the same booking or reservation form.

**Reproduction Result(s):**
1. Before the fix, the booking form was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The booking date and reservation time were displayed before and after the cancellation.

**Fix Result(s):**
1. The form closes after "Cancel" is clicked and no change is saved.
2. The booking date and reservation time remain unchanged when the form is reopened.
3. No new booking or reservation is created by the cancel action.
4. No script error appears on screen during the cancel flow.

### 3. Asian Language Footer Weekday Display

#### 3.1. Footer Weekday Is Displayed in English

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. English is available as a display language for the site.
3. The user is logged in to HKJC TMS Operation.

**Step(s):**
1. Switch the display language to English.
2. Check the footer weekday name at the bottom right of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the English weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The English weekday name was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in English.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 3.2. Footer Weekday Is Displayed in Traditional Chinese (Hong Kong)

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. Traditional Chinese (Hong Kong) is available as a display language for the site.
3. The user is logged in to HKJC TMS Operation.

**Step(s):**
1. Switch the display language to Traditional Chinese (Hong Kong).
2. Check the footer weekday name at the bottom right of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Traditional Chinese (Hong Kong) weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The weekday name in Traditional Chinese (Hong Kong) was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Traditional Chinese (Hong Kong).
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 3.3. Footer Weekday Is Displayed in Simplified Chinese

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. Simplified Chinese is available as a display language for the site.
3. The user is logged in to HKJC TMS Operation.

**Step(s):**
1. Switch the display language to Simplified Chinese.
2. Check the footer weekday name at the bottom right of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Simplified Chinese weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The weekday name in Simplified Chinese was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Simplified Chinese.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 3.4. Footer Weekday Is Displayed in Japanese

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. Japanese is available as a display language for the site.
3. The user is logged in to HKJC TMS Operation.

**Step(s):**
1. Switch the display language to Japanese.
2. Check the footer weekday name at the bottom right of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Japanese weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The weekday name in Japanese was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Japanese.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 3.5. Footer Weekday Is Displayed in Korean

**Prerequisite(s):**
1. The HKJC TMS Operation front-end is deployed with the updated date library bundle.
2. Korean is available as a display language for the site.
3. The user is logged in to HKJC TMS Operation.

**Step(s):**
1. Switch the display language to Korean.
2. Check the footer weekday name at the bottom right of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Korean weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The weekday name in Korean was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Korean.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

## Test Environment

Testing is performed on the HKJC TMS Operation front-end deployed with the updated moment.js 2.31.0 bundle, connected to a test HKJC TMS backend for the outlet under test. The browser developer console is used to check for script errors, and the SSDLC dependency check report is used to confirm the bundled library version. English, Traditional Chinese (Hong Kong), Simplified Chinese, Japanese and Korean are configured as available display languages. Booking and reservation forms for the outlet are available for the date and time display checks.
