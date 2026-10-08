# HERO-68915 KDS2-DEV-08-6 : KDS Device Authentication Test Report

## Functional Testing

### 1.Passcode Login

#### 1.1.Login with correct passcode after device setup

**Prerequisite(s):**
1. The KDS device build containing the HERO-68915 feature is deployed.
2. The device passcode is set in Kitchen Display System > Device Setup > Devices > Edit the device, and saved.
3. The updated device passcode is synchronized to the KDS device.
4. The Tenant ID and Device UID are set correctly in the KDS device settings.
5. The region is configured in the KDS device settings.

**Step(s):**
1. Open the KDS web client.
2. On the login screen, verify only the passcode field is shown (Tenant ID, Device UID and region are already configured).
3. Enter the correct passcode.
4. Submit the passcode.

**Test Result(s):**
1. The login screen only requires the passcode to be entered.
2. After submitting the correct passcode, the device starts loading the required data.
3. The app verifies the device session and finishes initial setup (handshake and device sync).
4. After setup completes, the ticket page is shown.

#### 1.2.Login with incorrect passcode shows "Unauthorized client" error

**Prerequisite(s):**
1. The KDS device build containing the HERO-68915 feature is deployed.
2. The device passcode is set and synchronized to the KDS device.
3. The Tenant ID and Device UID are set correctly.

**Step(s):**
1. Open the KDS web client.
2. On the login screen, enter an incorrect passcode.
3. Submit the passcode.

**Test Result(s):**
1. The login fails and an error message "Unauthorized client" is shown on the login screen.
2. The device does not proceed to the ticket page.
3. The user can re-enter the passcode and try again.

#### 1.3.Login with network error shows a clear message on the login screen

**Prerequisite(s):**
1. The KDS device build containing the HERO-68915 feature is deployed.
2. The device passcode is set and synchronized.
3. The network can be disconnected or the OAuth service can be made unreachable.

**Step(s):**
1. Open the KDS web client.
2. Disconnect the network or make the regional OAuth service unreachable.
3. On the login screen, enter the correct passcode.
4. Submit the passcode.

**Test Result(s):**
1. The login fails due to the network error.
2. A clear error message is shown on the login screen indicating a network or connection issue.
3. The device does not proceed to the ticket page.
4. The user can retry the login after the network is restored.

### 2.Session Management - Validity and Refresh

#### 2.1.Session stays valid for 1 day and then the user is logged out

**Prerequisite(s):**
1. The KDS device is successfully logged in with the correct passcode.
2. The session validity period is set to 1 day.

**Step(s):**
1. After a successful login, verify the device is on the ticket page and the session is active.
2. Wait for the session to approach expiry (or simulate the session expiry by advancing the session timestamp).
3. Verify the device behavior when the session expires.

**Test Result(s):**
1. The session remains valid for 1 day after login.
2. When the session expires (after approximately 1 day), the device is automatically logged out.
3. The user is returned to the login screen and must sign in again with the passcode.

#### 2.2.Browser refresh keeps the user signed in if the session is still valid

**Prerequisite(s):**
1. The KDS device is successfully logged in and the session is still valid (within the 1-day period).

**Step(s):**
1. After a successful login, verify the device is on the ticket page.
2. Refresh the browser (F5 or browser refresh button).
3. Verify the device behavior after the refresh.

**Test Result(s):**
1. After refreshing the browser, the user remains signed in because the session is still valid.
2. The app verifies the device session from the secure cookie and restores the signed-in state.
3. The ticket page is shown again without requiring a new login.

#### 2.3.Browser refresh logs the user out if the session has expired

**Prerequisite(s):**
1. The KDS device was logged in but the session has expired (more than 1 day has passed).

**Step(s):**
1. After the session has expired, refresh the browser.
2. Verify the device behavior after the refresh.

**Test Result(s):**
1. After refreshing the browser with an expired session, the user is logged out.
2. The login screen is shown, requiring the user to sign in again with the passcode.
3. The expired session is not restored.

### 3.Session Revocation and Unauthorized Access

#### 3.1.Server returns unauthorized and the user is sent back to the login screen

**Prerequisite(s):**
1. The KDS device is successfully logged in and the session is active.
2. The server can be configured to return an unauthorized response for the device session.

**Step(s):**
1. After a successful login, configure the server to return an unauthorized response for the device session.
2. Perform an action that triggers a server request (e.g., navigate or refresh).
3. Verify the device behavior when the server returns unauthorized.

**Test Result(s):**
1. When the server returns unauthorized, the user is sent back to the login screen.
2. The session is ended and the user must sign in again with the passcode.
3. The protected screens are no longer accessible until the user logs in again.

#### 3.2.Network briefly unavailable during refresh check keeps an existing valid session when possible

**Prerequisite(s):**
1. The KDS device is successfully logged in and the session is still valid.
2. The network can be briefly disconnected during a refresh check.

**Step(s):**
1. After a successful login, briefly disconnect the network.
2. Refresh the browser while the network is briefly unavailable.
3. Restore the network and verify the device behavior.

**Test Result(s):**
1. When the network is briefly unavailable during the refresh check, the app keeps the existing valid session when possible.
2. The user is not immediately logged out due to a brief network interruption.
3. After the network is restored, the session continues to be valid and the ticket page is shown.

### 4.Post-Login Setup - Handshake and Device Sync

#### 4.1.Successful handshake and device sync opens the ticket page

**Prerequisite(s):**
1. The KDS device is successfully logged in with the correct passcode.
2. The server is reachable and the handshake and device sync can complete successfully.

**Step(s):**
1. Enter the correct passcode and submit.
2. Observe the app verifying the device session.
3. Observe the handshake and device sync process.
4. Verify the ticket page is shown after setup completes.

**Test Result(s):**
1. After a successful login, the app runs the handshake and device sync.
2. The initial setup completes successfully.
3. The ticket page is shown after the setup finishes.
4. The user can interact with the tickets on the KDS device.

#### 4.2.Post-login setup failure logs the user out and allows retry

**Prerequisite(s):**
1. The KDS device is successfully logged in with the correct passcode.
2. The handshake or device sync can be made to fail (e.g., server error during setup).

**Step(s):**
1. Enter the correct passcode and submit.
2. Trigger a failure in the handshake or device sync (e.g., simulate a server error during setup).
3. Verify the device behavior when the post-login setup fails.

**Test Result(s):**
1. If the post-login setup (handshake or device sync) fails, the user is logged out.
2. The login screen is shown again, allowing the user to try again.
3. The device does not show the ticket page when the setup fails.
4. The user can retry the login after the setup issue is resolved.

### 5.Protected Screens and Logout

#### 5.1.Protected screens require login and setup/login pages remain available when logged out

**Prerequisite(s):**
1. The KDS device is logged out (no active session).

**Step(s):**
1. Open the KDS web client while logged out.
2. Attempt to access a protected screen (e.g., the ticket page) directly.
3. Verify the behavior.
4. Verify that the setup (device settings) and login pages are accessible while logged out.

**Test Result(s):**
1. Protected screens (e.g., ticket page) require login and are not accessible when logged out.
2. The user is redirected to the login screen when attempting to access a protected screen while logged out.
3. The setup (device settings) page remains available when logged out.
4. The login page remains available when logged out.

#### 5.2.Logout from the header revokes the server session and clears local login state

**Prerequisite(s):**
1. The KDS device is successfully logged in and the session is active.

**Step(s):**
1. While on the ticket page, click the logout button in the header.
2. Verify the device behavior after logout.

**Test Result(s):**
1. Clicking logout from the header revokes the server session when possible.
2. The local login state (signed in / expiry flags) is cleared.
3. The user is returned to the login screen.
4. The passcode is not stored locally after logout.

### 6.Device Settings Change - Forced Re-Login

#### 6.1.Changing Tenant ID, Device UID, or region logs the user out and requires login again

**Prerequisite(s):**
1. The KDS device is successfully logged in and the session is active.
2. The device settings (Tenant ID, Device UID, region) can be changed.

**Step(s):**
1. While logged in, open the device settings.
2. Change the Tenant ID.
3. Save the change and verify the device behavior.
4. Log back in, then change the Device UID and verify the behavior.
5. Log back in, then change the region and verify the behavior.

**Test Result(s):**
1. Changing the Tenant ID logs the user out and requires login again with the passcode.
2. Changing the Device UID logs the user out and requires login again with the passcode.
3. Changing the region logs the user out and requires login again with the passcode.
4. The login session and related device data are cleared when any of these settings change.

### 7.Passcode Setup and Synchronization

#### 7.1.Set and update the device passcode in KDS Device Setup

**Prerequisite(s):**
1. Access to Infrasys Cloud Admin page is available.
2. The KDS device is registered in Kitchen Display System > Device Setup > Devices.

**Step(s):**
1. Go to Kitchen Display System > Device Setup > Devices.
2. Edit the target device.
3. Set or update the device passcode.
4. Click "Save".
5. Verify the updated passcode is synchronized to the KDS device.

**Test Result(s):**
1. The device passcode is set or updated successfully in the Device Setup.
2. The updated passcode is synchronized to the KDS device.
3. The KDS device can log in using the updated passcode.
4. The old passcode (if changed) no longer works for login.

### 8.Localization - Login and Related Messages

#### 8.1.Login screen and error messages are translated

**Prerequisite(s):**
1. The KDS device build containing the HERO-68915 feature is deployed.
2. Multiple languages are configured for the KDS device.

**Step(s):**
1. Open the KDS web client with one language configured.
2. Verify the login screen labels, buttons, and messages are displayed in the configured language.
3. Enter an incorrect passcode and verify the error message ("Unauthorized client") is translated.
4. Trigger a network error and verify the error message is translated.
5. Switch to another language and verify all login and related messages are translated.

**Test Result(s):**
1. The login screen labels and buttons are displayed in the configured language.
2. The "Unauthorized client" error message is translated for the incorrect passcode scenario.
3. The network error message is translated.
4. All login and related messages are correctly translated for each configured language.
