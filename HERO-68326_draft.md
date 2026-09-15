# TMS2.0-Switch to traditional Chinese login and a blank page appears Test Report

## Functional Testing

### 1. Language Switch Reproduction

#### 1.1 Reproduce Blank Page on Traditional Chinese Switch

**Prerequisite(s):**
- TMS2.0 application is deployed and accessible.
- A valid user account with Login Name and Password exists.
- At least one Outlet is configured and available for selection.
- The application supports traditional Chinese language pack.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet from the dropdown list.
5. Switch the language selector to traditional Chinese (繁體中文).
6. Click the Continue button to proceed.

**Reproduction Result(s):**
- After switching to traditional Chinese and clicking Continue, a blank page is displayed.
- No error message is shown on the screen.
- The application does not navigate to the main dashboard.
- Browser console may show JavaScript rendering errors or missing locale resource errors.

**Fix Result(s):**
- After applying the fix, switching to traditional Chinese and clicking Continue displays the page normally.
- The main dashboard loads with all UI elements rendered in traditional Chinese.
- No blank page or rendering errors occur.
- Navigation completes successfully to the expected landing page.

---

### 2. Fix Verification - Page Display After Language Switch

#### 2.1 Verify Page Rendering After Traditional Chinese Switch

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed to the test environment.
- A valid user account with Login Name and Password exists.
- At least one Outlet is configured and available for selection.
- Traditional Chinese language resources are fully loaded in the system.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet from the dropdown list.
5. Switch the language selector to traditional Chinese (繁體中文).
6. Click the Continue button to proceed.
7. Observe the loaded page content and layout.
8. Verify that all text labels, buttons, and menus are displayed in traditional Chinese.

**Reproduction Result(s):**
- Prior to the fix, a blank page was displayed after switching to traditional Chinese.
- UI components failed to render and no content was visible to the user.

**Fix Result(s):**
- The page is displayed normally with all UI elements rendered correctly.
- All text content is shown in traditional Chinese as expected.
- The layout and styling are consistent with other language displays.
- No console errors or rendering failures are observed.

#### 2.2 Verify Localization Resource Loading

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- Browser developer tools are accessible for network inspection.
- Traditional Chinese language resource files are available on the server.

**Step(s):**
1. Open browser developer tools (Network tab).
2. Navigate to the TMS2.0 Login View.
3. Input a valid Login Name and Password.
4. Click the Login button to authenticate.
5. On the Outlet selection page, switch the language selector to traditional Chinese.
6. Click the Continue button to proceed.
7. Monitor the Network tab for traditional Chinese locale resource requests.
8. Verify that all resource files return HTTP 200 status.

**Reproduction Result(s):**
- Before the fix, traditional Chinese locale resource requests were failing or returning empty responses.
- The application could not load the required translations, resulting in a blank page.

**Fix Result(s):**
- All traditional Chinese locale resource files load successfully with HTTP 200 status.
- Resource files contain complete translations for all UI elements.
- The application correctly binds the loaded resources to the UI components.

---

### 3. Multi-Language Switch Testing

#### 3.1 English to Traditional Chinese Switch

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- Both English and traditional Chinese language packs are installed.

**Step(s):**
1. Navigate to the TMS2.0 Login View with default language set to English.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet.
5. Switch the language selector from English to traditional Chinese.
6. Click the Continue button to proceed.
7. Observe the page display and language of UI elements.

**Reproduction Result(s):**
- Before the fix, switching from English to traditional Chinese resulted in a blank page.
- The application failed to render content after the language switch.

**Fix Result(s):**
- The page displays normally after switching from English to traditional Chinese.
- All UI elements are rendered in traditional Chinese.
- No rendering issues or blank page occurs.

#### 3.2 Traditional Chinese to English Switch

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- Both English and traditional Chinese language packs are installed.

**Step(s):**
1. Navigate to the TMS2.0 Login View with language set to traditional Chinese.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet.
5. Switch the language selector from traditional Chinese to English.
6. Click the Continue button to proceed.
7. Observe the page display and language of UI elements.

**Reproduction Result(s):**
- The reverse switch from traditional Chinese to English may also have rendering issues before the fix.
- Language resource binding may not reset properly.

**Fix Result(s):**
- The page displays normally after switching from traditional Chinese to English.
- All UI elements are rendered in English as expected.
- Language resource binding resets and loads correctly.

#### 3.3 Back and Forth Language Switching

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- The user has successfully logged in and reached the Outlet selection page.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, switch language to traditional Chinese.
5. Switch back to English.
6. Switch to traditional Chinese again.
7. Switch to English once more.
8. Click the Continue button after each switch to verify page rendering.
9. Observe the page display consistency across multiple switches.

**Reproduction Result(s):**
- Before the fix, repeated language switching could cause blank pages or rendering failures.
- Memory leaks or resource loading errors accumulated with each switch.

**Fix Result(s):**
- The page displays normally after each language switch.
- No blank pages or rendering issues occur across multiple switches.
- Language resources are properly managed and disposed of during switching.
- UI remains responsive and stable throughout the back-and-forth switching.

---

### 4. Edge Cases

#### 4.1 Switch Language During Page Loading

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- The application is in a state where page loading takes a measurable amount of time.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet.
5. Switch the language selector to traditional Chinese.
6. Immediately click the Continue button before the page fully loads.
7. Observe the application behavior and page rendering.

**Reproduction Result(s):**
- Before the fix, switching language during loading could cause a blank page or rendering failure.
- Race conditions between language resource loading and page rendering could occur.

**Fix Result(s):**
- The application handles the language switch during loading gracefully.
- The page renders correctly in traditional Chinese once loading completes.
- No blank page or rendering failure occurs.
- A loading indicator may be displayed until resources are fully loaded.

#### 4.2 Network Interruption During Language Resource Loading

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- Network throttling or interruption simulation tools are available.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, select an Outlet.
5. Simulate a network interruption or throttle the connection to a very low speed.
6. Switch the language selector to traditional Chinese.
7. Click the Continue button to proceed.
8. Restore the network connection.
9. Observe the application behavior and error handling.

**Reproduction Result(s):**
- Before the fix, network interruption during language resource loading could result in a blank page.
- No error message or retry mechanism was provided to the user.

**Fix Result(s):**
- The application displays an appropriate loading indicator or error message when network is interrupted.
- After network restoration, the application retries loading the language resources.
- The page renders correctly in traditional Chinese once resources are successfully loaded.
- No blank page or unrecoverable state occurs.

#### 4.3 Switch Language with Invalid Outlet Selection

**Prerequisite(s):**
- The fix patch for HERO-68326 has been deployed.
- A valid user account with Login Name and Password exists.
- The Outlet selection list is accessible.

**Step(s):**
1. Navigate to the TMS2.0 Login View.
2. Input a valid Login Name and Password.
3. Click the Login button to authenticate.
4. On the Outlet selection page, do not select an Outlet (leave it empty or default).
5. Switch the language selector to traditional Chinese.
6. Click the Continue button to proceed.
7. Observe the application behavior and validation messages.

**Reproduction Result(s):**
- Before the fix, switching language without selecting an Outlet could cause unexpected behavior or blank page.
- Validation messages may not display correctly in the selected language.

**Fix Result(s):**
- The application displays a validation message in traditional Chinese prompting the user to select an Outlet.
- No blank page or rendering failure occurs.
- The page remains on the Outlet selection page for the user to complete the selection.
- Validation messages are correctly localized in traditional Chinese.
