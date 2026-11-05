# HERO-69850 Test Report

## Functional Testing

### 1. Current View Loading After Login

#### 1.1. Current View Opens Without Script Error After Login

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. A valid TMS operator account exists for the outlet under test.
3. The browser developer console is available for checking client-side script errors.

**Step(s):**
1. Log in to TMS Operation.
2. Open Current View.
3. Observe the screen for any script error message or blank date area.
4. Check the browser developer console for script errors related to date handling.

**Reproduction Result(s):**
1. Before the fix, the TMS Operation front-end bundle contained moment.js 2.30.1, which the SSDLC dependency check reported as an outdated library version.
2. The Current View screen opened after login and the date and time areas were rendered.
3. The reported defect was the dependency compliance finding rather than a visible screen failure.

**Fix Result(s):**
1. The deployed front-end bundle contains moment.js 2.31.0 and the SSDLC dependency check no longer reports the outdated library version.
2. Current View opens after login without any script error shown on screen.
3. The browser developer console shows no script error related to date handling.
4. The date and time areas on Current View are rendered and remain available for operation.

#### 1.2. Footer Clock Displays Today's Date, Weekday and Time

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to TMS Operation.
3. Current View is open.

**Step(s):**
1. Check the footer clock at the bottom of the screen.
2. Compare the displayed date with the current system date.
3. Verify that the date is shown in the format YYYY-MM-DD (Weekday) HH:MM AM/PM, for example 2026-10-22 (Thursday) 11:47 AM.

**Reproduction Result(s):**
1. Before the fix, the footer clock was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The footer clock displayed a date, weekday and time without a visible functional failure.

**Fix Result(s):**
1. The footer clock shows today's date, weekday and time.
2. The displayed date matches the current system date.
3. The weekday is shown next to the date and the time is shown with the AM/PM indicator.
4. The footer area is not blank and no script error is shown on screen.

#### 1.3. Footer Clock Updates Every Second

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to TMS Operation and Current View is open.
3. The footer clock is visible at the bottom of the screen.

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

### 2. Booking Date Next to Outlet and Meal Period Displays a Valid Date

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to TMS Operation and Current View is open.
3. An outlet with a configured meal period is displayed on Current View.

**Step(s):**
1. On Current View, locate the booking date shown next to the outlet and meal period.
2. Verify the booking date value and its format.
3. Compare the booking date with the date expected for the displayed meal period.

**Reproduction Result(s):**
1. Before the fix, the booking date was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The booking date was displayed next to the outlet and meal period.

**Fix Result(s):**
1. The booking date is displayed next to the outlet and meal period.
2. The booking date is a valid date shown in the format YYYY-MM-DD (Weekday), for example 2026-10-22 (Thu).
3. The booking date matches the date of the displayed meal period.
4. The booking date area is not blank and no script error is shown.

### 3. Walk In Flow

#### 3.1. Walk In Panel Opens With Date and Time Intact

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to TMS Operation and Current View is open.
3. At least one outlet and meal period are available for a walk-in booking.

**Step(s):**
1. Click "Walk In" on Current View.
2. Confirm that the Walk In panel opens.
3. Review the date and time shown on the main screen while the Walk In panel is open.
4. Click "Cancel" without saving.

**Reproduction Result(s):**
1. Before the fix, the Walk In panel and the date and time areas were rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The Walk In panel opened and the date and time on the main screen were displayed.

**Fix Result(s):**
1. The Walk In panel opens when "Walk In" is clicked.
2. The date and time on the main screen remain displayed correctly while the Walk In panel is open.
3. The footer clock continues to update while the Walk In panel is open.
4. No script error appears on screen when the Walk In panel is opened.
5. Clicking "Cancel" closes the panel without saving.

#### 3.2. Walk In Cancellation Returns to Current View Without Saving

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The user is logged in to TMS Operation and Current View is open.
3. The booking date shown on Current View is recorded before the Walk In panel is opened.

**Step(s):**
1. Click "Walk In" on Current View.
2. Click "Cancel" without saving the walk-in booking.
3. Confirm that Current View is displayed again.
4. Check the booking date and the footer clock on Current View.

**Reproduction Result(s):**
1. Before the fix, the Walk In cancel flow completed and no walk-in booking was saved, while the SSDLC dependency check still reported moment.js 2.30.1.
2. The date and time areas on Current View were displayed before and after the cancellation.

**Fix Result(s):**
1. Current View is displayed again after "Cancel" is clicked.
2. No walk-in booking is created or saved.
3. The booking date and footer clock on Current View remain correct after the cancellation.
4. No script error appears on screen during the cancel flow.

### 4. Asian Language Footer Weekday Display

#### 4.1. Footer Weekday Is Displayed in English

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. English is available as a display language for the site.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to English.
2. Check the footer weekday name at the bottom of the screen.
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

#### 4.2. Footer Weekday Is Displayed in Traditional Chinese (Hong Kong)

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. Traditional Chinese (Hong Kong) is available as a display language for the site.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Traditional Chinese (Hong Kong).
2. Check the footer weekday name at the bottom of the screen.
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

#### 4.3. Footer Weekday Is Displayed in Simplified Chinese

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. Simplified Chinese is available as a display language for the site.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Simplified Chinese.
2. Check the footer weekday name at the bottom of the screen.
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

#### 4.4. Footer Weekday Is Displayed in Japanese

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. Japanese is available as a display language for the site.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Japanese.
2. Check the footer weekday name at the bottom of the screen.
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

#### 4.5. Footer Weekday Is Displayed in Korean

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. Korean is available as a display language for the site.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Korean.
2. Check the footer weekday name at the bottom of the screen.
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

### 5. European Language Footer Weekday Wording

#### 5.1. Footer Weekday Wording Is Correct in Polish

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The site uses Polish and Polish is available as a display language.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Polish.
2. Check the footer weekday name at the bottom of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Polish weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The Polish weekday name was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Polish with the wording provided by the updated library version.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 5.2. Footer Weekday Wording Is Correct in Swedish

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The site uses Swedish and Swedish is available as a display language.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Swedish.
2. Check the footer weekday name at the bottom of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Swedish weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The Swedish weekday name was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Swedish with the wording provided by the updated library version.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 5.3. Footer Weekday Wording Is Correct in Portuguese (Brazil)

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The site uses Portuguese (Brazil) and Portuguese (Brazil) is available as a display language.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to Portuguese (Brazil).
2. Check the footer weekday name at the bottom of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the Portuguese (Brazil) weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The weekday name in Portuguese (Brazil) was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in Portuguese (Brazil) with the wording provided by the updated library version.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

#### 5.4. Footer Weekday Wording Is Correct in German

**Prerequisite(s):**
1. The TMS Operation front-end is deployed with the updated date library bundle.
2. The site uses German and German is available as a display language.
3. The user is logged in to TMS Operation and Current View is open.

**Step(s):**
1. Switch the display language to German.
2. Check the footer weekday name at the bottom of the screen.
3. Check the date numbers shown in the footer clock.
4. Confirm that the clock continues to update every second.

**Reproduction Result(s):**
1. Before the fix, the German weekday name was rendered by moment.js 2.30.1, and the SSDLC dependency check reported the outdated library version.
2. The German weekday name was displayed in the footer clock.

**Fix Result(s):**
1. The footer weekday name is displayed in German with the wording provided by the updated library version.
2. The date numbers in the footer clock remain correct after the language switch.
3. The clock continues to update every second.
4. No script error appears on screen after the language switch.

## Test Environment

Testing is performed on the TMS Operation front-end deployed with the updated moment.js 2.31.0 bundle, connected to a test TMS backend for an outlet with configured meal periods. The browser developer console is used to check for script errors, and the SSDLC dependency check report is used to confirm the bundled library version. The site is configured with English, Traditional Chinese (Hong Kong), Simplified Chinese, Japanese and Korean, and where applicable with Polish, Swedish, Portuguese (Brazil) and German for the weekday wording checks.
