# TMS2.0 - Rebranding - New login page for Profile Management Test Report

## Functional Testing

### 1.Login Page Branding Display

#### 1.1.Default Profile Management login page shows the new branded UI

**Prerequisite(s):**
1. The Infrasys POS build containing the rebranding changes is deployed.
2. No custom login background image has been configured yet.

**Step(s):**
1. Open the Profile Management login page.
2. Check the product name, product logo, module name, and default background image.
3. Log in with valid credentials and check the general page header after login.

**Test Result(s):**
1. The Profile Management login page uses the same login UI as the Admin login page.
2. The product name shows "Infrasys POS" instead of "Infrasys Cloud" and the new product logo is displayed on the login page and the general page header.
3. The module name "Profile Management" is shown on the login page.
4. The default login background image is the same one used by the Admin login page.

#### 1.2.Login with valid and invalid credentials on the new login page

**Prerequisite(s):**
1. A valid Profile Management user account exists.
2. The Profile Management login page is accessible.

**Step(s):**
1. Enter a valid username and password on the new login page and click Login.
2. Log out, then enter an invalid username or password and click Login.

**Test Result(s):**
1. Login with valid credentials succeeds and navigates to the Profile Management home page.
2. Login with invalid credentials fails and an error message is displayed on the new login page.

### 2.Custom Login Background Image Configuration

#### 2.1.Admin configures the login background via Global Settings

**Prerequisite(s):**
1. Administrator account has access to System Management > System Configuration.

**Step(s):**
1. Go to System Management > System Configuration > Global Settings.
2. Edit the Custom Login Background Image, upload a new image, and save.
3. Open the Profile Management login page.

**Test Result(s):**
1. The custom background image is saved successfully.
2. The Profile Management login page displays the uploaded background image.

#### 2.2.Member Module configuration updates the login background

**Prerequisite(s):**
1. Administrator account has access to System Management > System Configuration.
2. A custom background image has already been set in Global Settings.

**Step(s):**
1. Go to System Management > System Configuration > Member Module.
2. Edit the Custom Login Background Image with a different image and save.
3. Open the Profile Management login page.

**Test Result(s):**
1. The member module background image is saved successfully.
2. The Profile Management login page displays the image configured in the Member Module.

#### 2.3.Sub config zone overrides the background image in Enterprise mode

**Prerequisite(s):**
1. The system is running in Enterprise mode with a sub config zone that has an active Shop.
2. A background image is configured in the current config zone global setting.

**Step(s):**
1. In the sub config zone, override the Custom Login Background Image with its own image and save.
2. Open the Profile Management login page of that sub config zone.

**Test Result(s):**
1. The background image override for Profile Management is supported in the sub config zone.
2. The sub config zone login page uses its own background image.

#### 2.4.Background image fallback precedence chain

**Prerequisite(s):**
1. Enterprise mode with a sub config zone, a current config zone, and the Supreme Config Zone are available.
2. Background images can be configured at each level independently.

**Step(s):**
1. Clear the background image of the sub config zone and open its Profile Management login page.
2. Clear the current config zone global setting background image and open the login page again.
3. Clear the member module background image of the Supreme Config Zone and open the login page again.
4. Clear the Supreme Config Zone global setting background image and open the login page again.

**Test Result(s):**
1. With no sub config zone image, the login page falls back to the background image of the current config zone global setting.
2. With no current config zone global setting image, the login page falls back to the background image of the member module of the Supreme Config Zone.
3. With no Supreme member module image, the login page falls back to the background image of the Supreme Config Zone global setting.

#### 2.5.Sub config zone without an active Shop cannot edit the background image

**Prerequisite(s):**
1. Enterprise mode with a sub config zone that does not have an active Shop.

**Step(s):**
1. Open the background image configuration of the sub config zone without an active Shop.
2. Try to edit the Custom Login Background Image.

**Test Result(s):**
1. The background image cannot be edited when the sub config zone does not have an active Shop.

### 3.Login Authentication and Password Reset

#### 3.1.Authentication can be enabled for Profile Management login

**Prerequisite(s):**
1. Administrator account has access to the authentication configuration.
2. The Profile Management login page is accessible.

**Step(s):**
1. Enable the Authentication option for Profile Management login and save.
2. Open the Profile Management login page and log in with valid credentials.

**Test Result(s):**
1. Profile Management login supports enabling Authentication.
2. Login behaves according to the enabled authentication setting.

#### 3.2.Password reset is supported from Profile Management login

**Prerequisite(s):**
1. The Profile Management login page is accessible.
2. A user account with an email address exists for password reset.

**Step(s):**
1. On the Profile Management login page, trigger the Reset Password flow with the user account email.
2. Complete the reset process and log in with the new password.

**Test Result(s):**
1. Profile Management login supports resetting the password.
2. The user can log in successfully with the new password.

### 4.Audit Log for Background Image Modification

**Prerequisite(s):**
1. Administrator account has access to System Management > System Configuration and Audit Logs Management > Audit Logs.

**Step(s):**
1. Go to System Management > System Configuration > Global Settings, modify the Custom Login Background Image, and save.
2. Go to Audit Logs Management > Audit Logs and view the audit log.

**Test Result(s):**
1. After the background image is modified and saved successfully, an audit log is generated.
2. The audit log details reflect the background image modification.

### 5.Regression Scope

1. Admin login page and its default background image display.
2. General page header logo and product name across other modules.
3. Normal Profile Management login and logout flows.
4. Other System Configuration settings editing and saving.
5. Audit log generation for other configuration changes.
