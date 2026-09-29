# HERO-68451 Platform - HQ - Hide Modules/Functions Without Read Permission Test Report

## Functional Testing

### F1.Setting Configuration & Visibility

#### F1.1.Verify "Function Permission Filter" Setting Is Visible
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. User has access to System Management > System Configuration
3. The feature has been deployed in the test environment

**Step(s):**
1. Navigate to System Management > System Configuration
2. Look for the "Function Permission Filter" setting in Global Settings or User Module section

**Test Result(s):**
1. The "Function Permission Filter" setting is visible under System Configuration
2. The setting has an On/Off toggle control
3. The setting label and description are clearly displayed

#### F1.2.Verify Setting Defaults to Disable (Off)
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. The "Function Permission Filter" setting has not been previously toggled

**Step(s):**
1. Navigate to System Management > System Configuration
2. Check the current state of the "Function Permission Filter" setting

**Test Result(s):**
1. The "Function Permission Filter" setting is set to "Disable" (Off) by default
2. All modules and functions are visible regardless of Read permissions

#### F1.3.Verify Toggle Setting On
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. The "Function Permission Filter" setting is currently Off (Disable)
3. User has Read permission for some but not all functions in at least one module

**Step(s):**
1. Navigate to System Management > System Configuration
2. Toggle the "Function Permission Filter" setting to On (Enable)
3. Save the configuration
4. Refresh the page (or log out and log back in)

**Test Result(s):**
1. The setting is successfully toggled to On
2. After page refresh, modules/functions without Read permission are hidden from navigation
3. A message "Unauthorized functions are hidden" appears below the Functions area
4. Only modules/functions with Read permission remain visible

#### F1.4.Verify Toggle Setting Off Restores Default Visibility
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. The "Function Permission Filter" setting is currently On (Enable)
3. Some modules/functions were previously hidden

**Step(s):**
1. Navigate to System Management > System Configuration
2. Toggle the "Function Permission Filter" setting to Off (Disable)
3. Save the configuration
4. Refresh the page (or log out and log back in)

**Test Result(s):**
1. The setting is successfully toggled to Off
2. After page refresh, all modules and functions are visible regardless of Read permissions
3. Previously hidden modules are now accessible and visible in navigation
4. The "Unauthorized functions are hidden" message no longer appears

#### F1.5.Verify Only System Administrator Role Can Configure
**Prerequisite(s):**
1. User with a non-System Administrator role (e.g., IT Manager, System Master) is logged into HQ Admin Console
2. User has access to System Management > System Configuration

**Step(s):**
1. Navigate to System Management > System Configuration
2. Attempt to locate and toggle the "Function Permission Filter" setting

**Test Result(s):**
1. The "Function Permission Filter" setting is either not visible or not editable for non-System Administrator users
2. The user cannot toggle the setting On or Off
3. An appropriate permission denied message or hidden state is observed

#### F1.6.Verify Setting Only Configurable in Supreme Config Zone
**Prerequisite(s):**
1. User with System Administrator role is logged into a non-Supreme config zone (e.g., region or area zone)
2. User has access to System Management > System Configuration

**Step(s):**
1. Navigate to System Management > System Configuration in a non-Supreme config zone
2. Attempt to locate and toggle the "Function Permission Filter" setting

**Test Result(s):**
1. The "Function Permission Filter" setting is not configurable in non-Supreme config zones
2. The setting is either read-only or hidden at lower config zones
3. The setting cannot be overridden at lower zones

### F2.Module Hiding Behavior

#### F2.1.Verify Entire Module Hidden When No Read Permission for Any Function
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable) in Supreme config zone
2. A test user has no Read permission for any function under the "Messaging" module
3. The test user has logged out and logged back in after the setting was toggled

**Step(s):**
1. Log in as the test user
2. Observe the Admin Console navigation menu
3. Look for the "Messaging" module in the navigation

**Test Result(s):**
1. The "Messaging" module is not visible in the Admin Console navigation
2. The module and all its sub-headings are completely hidden
3. No empty space or placeholder is left for the hidden module

#### F2.2.Verify Partial Module Visibility When Some Read Permissions Exist
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. A test user has Read permission for "User Management > Create User" but NOT for "User Management > Delete User"
3. The test user has logged out and logged back in after the setting was toggled

**Step(s):**
1. Log in as the test user
2. Navigate to User Management module
3. Observe which functions are visible under User Management

**Test Result(s):**
1. The "User Management" module is visible in navigation
2. The "Create User" function is visible within the module
3. The "Delete User" function is hidden within the module
4. Only functions with Read permission are displayed

#### F2.3.Verify Module Visible But Empty When No Child Function Has Read Permission
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. A test user has Read permission for a module itself but no Read permission for any of its child functions
3. The test user has logged out and logged back in

**Step(s):**
1. Log in as the test user
2. Navigate to the module in the Admin Console
3. Open the module to view its child functions

**Test Result(s):**
1. The module is visible in the navigation
2. The module appears empty or greyed out when opened
3. No child functions are listed under the module

#### F2.4.Verify "Unauthorized Functions Are Hidden" Message Appears
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. User has some functions without Read permission
3. User has refreshed the page after the setting was enabled

**Step(s):**
1. Log in as the test user
2. Navigate to any page with hidden functions
3. Observe the area below the Functions section

**Test Result(s):**
1. A message "Unauthorized functions are hidden" appears below the Functions area
2. The message is clearly visible and appropriately styled
3. The message indicates that some functions have been hidden due to permission settings

### F3.Permission Change Effects

#### F3.1.Verify Visibility Changes on User Access Control Update
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. A test user currently has Read permission for "Digital Signage > Display Screens"
3. Admin user is logged in to modify permissions

**Step(s):**
1. Navigate to User Access Control settings
2. Select the test user
3. Remove Read permission for "Digital Signage > Display Screens"
4. Save the permission changes
5. Log out as the test user and log back in

**Test Result(s):**
1. After re-login, the "Display Screens" sub-menu under "Digital Signage" is hidden
2. If "Display Screens" was the only function with Read permission under "Digital Signage", the entire "Digital Signage" module is hidden
3. The visibility change reflects the updated permission immediately after re-login

#### F3.2.Verify Visibility Changes on Adding/Removing User from User Group
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. A test user is currently in User Group A which has Read permission for "Reports" module
3. Admin user is logged in

**Step(s):**
1. Navigate to User Group Management
2. Remove the test user from User Group A
3. Save the changes
4. Log out as the test user and log back in

**Test Result(s):**
1. After re-login, the user's module/function visibility reflects the permissions of the updated user group membership
2. If the user no longer has Read permission for "Reports" via any group, the "Reports" module is hidden
3. The visibility change is consistent with the user's new group membership

#### F3.3.Verify Visibility Changes on Setting Toggle
**Prerequisite(s):**
1. "Function Permission Filter" setting is currently On (Enable)
2. User has some functions hidden due to no Read permission
3. User is currently logged in

**Step(s):**
1. As System Administrator, toggle the "Function Permission Filter" setting to Off
2. Save the configuration
3. Refresh the page as the test user (without logging out)

**Test Result(s):**
1. After page refresh, all modules and functions become visible regardless of Read permissions
2. Previously hidden modules are now accessible
3. The "Unauthorized functions are hidden" message disappears

### F4.Refresh & Login Behavior

#### F4.1.Verify Changes Take Effect After Page Refresh
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. User is currently logged in with some modules visible
3. Admin user removes Read permission for a module the user currently sees

**Step(s):**
1. As admin, remove Read permission for a visible module from the test user
2. As the test user, refresh the page (F5 or browser refresh)
3. Observe the navigation menu

**Test Result(s):**
1. After page refresh, the module without Read permission is hidden
2. The change takes effect without requiring logout and re-login
3. The navigation updates to reflect the current permission state

#### F4.2.Verify Sub-Level Changes Take Effect After Re-Login
**Prerequisite(s):**
1. "Function Permission Filter" setting is changed from On to Off (or Off to On) at a child config zone
2. User is currently logged in at the child config zone

**Step(s):**
1. Change the "Function Permission Filter" setting at the child config zone
2. Refresh the page without logging out
3. Observe the navigation
4. Log out and log back in
5. Observe the navigation again

**Test Result(s):**
1. After page refresh alone, the sub-level setting change may not fully take effect
2. After logout and re-login, the sub-level setting change takes full effect
3. The navigation reflects the child zone's setting after re-login

### F5.Direct URL Access

#### F5.1.Verify Hidden Modules Inaccessible via Direct URL
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. A module (e.g., "Messaging") is hidden from the user due to no Read permission
3. User knows the direct URL of the hidden module

**Step(s):**
1. Log in as the test user
2. Enter the direct URL of the hidden "Messaging" module in the browser address bar
3. Press Enter to navigate

**Test Result(s):**
1. The user is not able to access the hidden module via direct URL
2. The user is redirected to a "Page Not Found" page or the dashboard
3. The hidden module content is not displayed even via direct URL access

### F6.Audit Log

#### F6.1.Verify Toggle Action Recorded in Audit Log
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. The "Function Permission Filter" setting is currently Off
3. User has access to view Audit Log

**Step(s):**
1. Toggle the "Function Permission Filter" setting to On
2. Save the configuration
3. Navigate to the Audit Log
4. Search for the most recent audit log entries

**Test Result(s):**
1. A new audit log entry is created for the toggle action
2. The entry records the UserID of the operator
3. The entry records the timestamp of the action
4. The entry records the action (setting turned On)
5. The entry is associated with the "Function Permission Filter" setting

#### F6.2.Verify Toggle Off Action Recorded in Audit Log
**Prerequisite(s):**
1. User with System Administrator role is logged into HQ Admin Console
2. The "Function Permission Filter" setting is currently On
3. User has access to view Audit Log

**Step(s):**
1. Toggle the "Function Permission Filter" setting to Off
2. Save the configuration
3. Navigate to the Audit Log
4. Search for the most recent audit log entries

**Test Result(s):**
1. A new audit log entry is created for the toggle Off action
2. The entry records the UserID, timestamp, and action (setting turned Off)
3. The audit log correctly distinguishes between On and Off actions

### F7.Navigation Without Permission Settings

#### F7.1.Verify Navigation Without Permission Settings Still Displayed
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. There exists a sub-navigation that cannot be set in User Access Control (i.e., no permission configuration available)
3. User has "All Access Rights = No Read" for this navigation

**Step(s):**
1. Log in as the test user
2. Observe the navigation menu
3. Look for the sub-navigation that cannot be configured with permissions

**Test Result(s):**
1. The sub-navigation that cannot be set in User Access Control is still displayed
2. The parent navigation of this sub-navigation is also still displayed
3. The "All Access Rights = No Read" setting does not hide this navigation because it cannot be permission-controlled

### F8.Zone-Level Configuration

#### F8.1.Verify Setting Cannot Be Overridden at Lower Zones
**Prerequisite(s):**
1. "Function Permission Filter" setting is On at Supreme config zone
2. User with System Administrator role is logged into a region or area config zone

**Step(s):**
1. Navigate to System Management > System Configuration at the lower config zone
2. Attempt to change the "Function Permission Filter" setting

**Test Result(s):**
1. The "Function Permission Filter" setting cannot be overridden at lower config zones
2. The setting is read-only or not editable at lower zones
3. The lower zone inherits the Supreme zone's setting

#### F8.2.Verify Setting Does Not Support HQ to LS Publishing
**Prerequisite(s):**
1. "Function Permission Filter" setting is configured at HQ
2. User with System Administrator role is logged into HQ

**Step(s):**
1. Navigate to System Management > System Configuration at HQ
2. Attempt to publish the "Function Permission Filter" setting to LS
3. Check the publishing options or publish queue

**Test Result(s):**
1. The "Function Permission Filter" setting cannot be published from HQ to LS
2. The setting is not included in the publish queue or publish options
3. LS must configure the setting independently at the LS Supreme config zone

### F9.HQ and LS Environment Support

#### F9.1.Verify Feature Works in HQ Environment
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable) in HQ Supreme config zone
2. A test user with limited Read permissions is available in HQ

**Step(s):**
1. Log in to HQ Admin Console as the test user
2. Observe the navigation menu
3. Verify modules without Read permission are hidden

**Test Result(s):**
1. The feature works correctly in HQ environment
2. Modules/functions without Read permission are hidden in HQ Admin Console
3. The "Unauthorized functions are hidden" message appears

#### F9.2.Verify Feature Works in LS Environment
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable) in LS Supreme config zone
2. A test user with limited Read permissions is available in LS

**Step(s):**
1. Log in to LS Admin Console as the test user
2. Observe the navigation menu
3. Verify modules without Read permission are hidden

**Test Result(s):**
1. The feature works correctly in LS environment
2. Modules/functions without Read permission are hidden in LS Admin Console
3. The "Unauthorized functions are hidden" message appears

## Compatibility Testing

### C1.Browser Compatibility for Platform (Backend)

#### C1.1.Verify Feature in Chrome
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. Chrome browser is installed on the test workstation
3. Test user has limited Read permissions

**Step(s):**
1. Open Chrome browser
2. Log in to Admin Console as the test user
3. Observe the navigation menu and module visibility

**Test Result(s):**
1. The feature works correctly in Chrome
2. Modules/functions are hidden/shown as expected
3. No rendering issues or JavaScript errors in the console

#### C1.2.Verify Feature in Firefox
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. Firefox browser is installed on the test workstation
3. Test user has limited Read permissions

**Step(s):**
1. Open Firefox browser
2. Log in to Admin Console as the test user
3. Observe the navigation menu and module visibility

**Test Result(s):**
1. The feature works correctly in Firefox
2. Modules/functions are hidden/shown as expected
3. No rendering issues or JavaScript errors

#### C1.3.Verify Feature in Microsoft Edge
**Prerequisite(s):**
1. "Function Permission Filter" setting is On (Enable)
2. Microsoft Edge browser is installed on the test workstation
3. Test user has limited Read permissions

**Step(s):**
1. Open Microsoft Edge browser
2. Log in to Admin Console as the test user
3. Observe the navigation menu and module visibility

**Test Result(s):**
1. The feature works correctly in Microsoft Edge
2. Modules/functions are hidden/shown as expected
3. No rendering issues or JavaScript errors

## Test Environment Information

### Local Test Environment
- **Server**: Local development server
- **Database**: Local test database
- **Browser**: Chrome (latest), Firefox (latest), Microsoft Edge (latest)
- **Test Data**: Test users with various permission configurations
- **Config Zone**: Supreme zone configured for testing

### HQ Test Environment
- **Server**: HQ staging server
- **Database**: HQ test database
- **Browser**: Chrome (latest), Firefox (latest), Microsoft Edge (latest)
- **Test Data**: Test users with various permission configurations
- **Config Zone**: Supreme zone and lower zones configured for testing

## Appendix

### Acceptance Criteria from JIRA (HERO-68451)

**Setting visibility and toggle:**
- Given a user with access to System Configuration is logged into the Admin Console
- When they navigate to System Management > System Configuration
- Then a new setting 'Hide Modules/Functions with no Access Rights' (On/Off toggle, default Off) is visible
- And the user can toggle the setting On or Off
- And the toggle action is recorded in Audit Log with UserID, timestamp, and action (On or Off)
- But a user without access to System Configuration cannot see or modify this setting

**Module hidden when no Read permission exists for any function:**
- Given the setting is On and a user has no Read permission for any function in the 'Reports' module
- When the user logs out and logs back in
- Then the 'Reports' module is not visible in the Admin Console navigation
- And navigating directly to any Reports URL redirects to a 'Page Not Found' page or the dashboard
- But the setting is Off, the 'Reports' module remains visible and navigating to it shows the 'No Access' error page as before

**Partial module visibility when some Read permissions exist:**
- Given the setting is On and a user has Read permission for 'User Management > Create User' but not for 'User Management > Delete User'
- When the user logs out and logs back in
- Then the 'User Management' module is visible in navigation
- And the 'Create User' function is visible within the module
- But the 'Delete User' function is hidden within the module
- And navigating directly to the 'Delete User' URL redirects to a 'Page Not Found' page or the dashboard

**Module visible but empty when no child function has Read permission:**
- Given the setting is On and a user has Read permission for a module but no Read permission for any of its child functions
- When the user logs out and logs back in
- Then the module is visible in navigation
- But the module appears empty or greyed out when opened, with no functions listed

**Zone-level configuration with property override:**
- Given the setting is Off at Supreme zone but On at a specific region zone
- When a user under that region logs into the Admin Console
- Then the hiding behavior follows the region zone's setting (On)
- And a property under that region can independently override the region's setting

**Setting Off restores default visibility:**
- Given the setting is toggled from On to Off
- When the user logs out and logs back in
- Then all modules and functions are displayed regardless of Read permissions, matching the current default behavior
- And previously hidden modules are accessible via direct URL and show their normal content
- But the visibility change does not take effect until logout and re-login; the current session retains the previous visibility state

### Release Notes Summary (v1.2.116.0)

A new setting, "Function Permission Filter," has been added. When enabled, the navigation will be hidden if a user does not have "Read" permission for it. If all functions under a group or module tab have no Read permission, hide the entire group or module tab. Both HQ and LS support this enhancement.

**Setup:**
- Add a new option in system settings: "Function Permission Filter", which is "Disable" by default
- Can only be configured in the supreme config zone by users with "System Administrator" role
- After enabling/disabling, navigation hiding/showing takes effect after refreshing the page
- Sub-level changes take effect after user logs in again
- Settings cannot be overridden
- Settings do not support publishing from HQ to LS
- Navigation without permission settings will still be displayed
- Once enabled, "Unauthorized functions are hidden" message appears
