# HQ - Migrate system_tools functions to admin page (Platform Dev) Test Report

## Functional Testing

### 1.Function Migration to POS System > Data In

#### 1.1.Recover Sales Data Available Under POS System > Data In

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The local transaction server admin page is accessible.
3. The signed-in user has been granted access to the Recover Sales Data function under User Management > Access Control > System.

**Step(s):**
1. Log in to the local transaction server admin page.
2. Navigate to POS System > Data In.
3. Verify that the Recover Sales Data function is listed under Data In.
4. Click the Recover Sales Data entry.

**Test Result(s):**
1. The Recover Sales Data function is available under POS System > Data In.
2. The function opens correctly without errors.
3. The function is no longer available only through the local server backend tool (system_tools).
4. No errors displayed.

#### 1.2.Request Export Transaction File in HQ Available Under POS System > Data In

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The local transaction server admin page is accessible.
3. The signed-in user has been granted access to the Request Export Transaction File in HQ function under User Management > Access Control > System.

**Step(s):**
1. Log in to the local transaction server admin page.
2. Navigate to POS System > Data In.
3. Verify that the Request Export Transaction File in HQ function is listed under Data In.
4. Click the Request Export Transaction File in HQ entry.

**Test Result(s):**
1. The Request Export Transaction File in HQ function is available under POS System > Data In.
2. The function opens correctly without errors.
3. The function is no longer available only through the local server backend tool (system_tools).
4. No errors displayed.

#### 1.3.Request Data Recovery Available Under POS System > Data In

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The local transaction server admin page is accessible.
3. The signed-in user has been granted access to the Request Data Recovery function under User Management > Access Control > System.

**Step(s):**
1. Log in to the local transaction server admin page.
2. Navigate to POS System > Data In.
3. Verify that the Request Data Recovery function is listed under Data In.
4. Click the Request Data Recovery entry.

**Test Result(s):**
1. The Request Data Recovery function is available under POS System > Data In.
2. The function opens correctly without errors.
3. The function is no longer available only through the local server backend tool (system_tools).
4. No errors displayed.

### 2.Local Transaction Server Scope

#### 2.1.Functions Available Only on the Local Transaction Server Admin Page

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The user can access both the local transaction server admin page and a non-local-transaction-server admin page (for example a regular HQ admin page).

**Step(s):**
1. Log in to the local transaction server admin page and confirm that the three migrated functions are visible under POS System > Data In.
2. Log in to a non-local-transaction-server admin page.
3. Navigate to POS System > Data In.

**Test Result(s):**
1. The three functions are visible only on the local transaction server admin page.
2. The three functions are NOT visible on the non-local-transaction-server admin page.
3. No errors displayed.

#### 2.2.Non-Administrator User Can Run Functions Without Temporary Administrator Role

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A non-Administrator user account exists for Shiji support staff.
3. The user has been granted access to the three migrated functions through the new access control configuration.

**Step(s):**
1. Log in as the non-Administrator user.
2. Navigate to POS System > Data In.
3. Run each of the three migrated functions (Recover Sales Data, Request Export Transaction File in HQ, Request Data Recovery).

**Test Result(s):**
1. The non-Administrator user can see and run the three migrated functions.
2. The hotel IT does NOT need to temporarily assign the Administrator role to Shiji support staff to run these functions.
3. No errors displayed.

### 3.Access Control Configuration

#### 3.1.Access Control Configuration Available Under User Management > Access Control > System

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The signed-in user can access User Management > Access Control.

**Step(s):**
1. Navigate to User Management > Access Control > System.
2. Verify that configuration entries exist for the three migrated functions.
3. Edit the access control setting for each function (for example grant to a role or a user).

**Test Result(s):**
1. The access control configuration for the three migrated functions is available under User Management > Access Control > System.
2. Access can be granted to or revoked from a role or user.
3. No errors displayed.

#### 3.2.User Without Access Cannot See or Run the Functions

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A user account without access to the three migrated functions exists.

**Step(s):**
1. Log in as the user without access.
2. Navigate to POS System > Data In.
3. Attempt to access each of the three migrated functions through the menu and through a direct URL.

**Test Result(s):**
1. The three functions are not visible in the menu for the user without access.
2. Direct URL access is rejected with a permission error or redirect.
3. No errors displayed.

#### 3.3.Role-Based Access Applies to All Users in the Role

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A role with multiple users has been granted access to the three migrated functions.

**Step(s):**
1. Log in as each user in the role.
2. Navigate to POS System > Data In.
3. Confirm visibility and execution of each function.

**Test Result(s):**
1. Every user in the role can see and run the three migrated functions.
2. Removing the role revokes the access for all users in the role.
3. No errors displayed.

### 4.Audit Logs

#### 4.1.Audit Log Recorded for Recover Sales Data

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The signed-in user has access to the Recover Sales Data function and to the audit log view.

**Step(s):**
1. Run the Recover Sales Data function from POS System > Data In.
2. Open the audit log view and search for the most recent Recover Sales Data entry.

**Test Result(s):**
1. An audit log entry is recorded for the Recover Sales Data action.
2. The entry contains user, timestamp, outlet/server context and action type.
3. No errors displayed.

#### 4.2.Audit Log Recorded for Request Export Transaction File in HQ

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The signed-in user has access to the Request Export Transaction File in HQ function and to the audit log view.

**Step(s):**
1. Run the Request Export Transaction File in HQ function from POS System > Data In.
2. Open the audit log view and search for the most recent Request Export Transaction File in HQ entry.

**Test Result(s):**
1. An audit log entry is recorded for the Request Export Transaction File in HQ action.
2. The entry contains user, timestamp, outlet/server context and action type.
3. No errors displayed.

#### 4.3.Audit Log Recorded for Request Data Recovery

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. The signed-in user has access to the Request Data Recovery function and to the audit log view.

**Step(s):**
1. Run the Request Data Recovery function from POS System > Data In.
2. Open the audit log view and search for the most recent Request Data Recovery entry.

**Test Result(s):**
1. An audit log entry is recorded for the Request Data Recovery action.
2. The entry contains user, timestamp, outlet/server context and action type.
3. No errors displayed.

### 5.Functional Behaviour Consistency with Legacy system_tools

#### 5.1.Recover Sales Data Produces the Same Result as system_tools

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A test scenario that requires sales data recovery is available.
3. The legacy system_tools behaviour is documented or reproducible for comparison.

**Step(s):**
1. Run Recover Sales Data from POS System > Data In with the same inputs used by the legacy system_tools flow.
2. Verify the recovery outcome (recovered sales data, status, logs).

**Test Result(s):**
1. Recover Sales Data behaves consistently with the legacy system_tools function.
2. The same recovery result is produced for the same input.
3. No errors displayed.

#### 5.2.Request Export Transaction File in HQ Produces the Same Result as system_tools

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A test scenario that requires an HQ transaction file export is available.
3. The legacy system_tools behaviour is documented or reproducible for comparison.

**Step(s):**
1. Run Request Export Transaction File in HQ from POS System > Data In with the same inputs used by the legacy system_tools flow.
2. Verify the export request outcome (file produced, status, logs).

**Test Result(s):**
1. Request Export Transaction File in HQ behaves consistently with the legacy system_tools function.
2. The same export file is produced for the same input.
3. No errors displayed.

#### 5.3.Request Data Recovery Produces the Same Result as system_tools

**Prerequisite(s):**
1. A build with the HERO-68359 change is deployed.
2. A test scenario that requires data recovery is available.
3. The legacy system_tools behaviour is documented or reproducible for comparison.

**Step(s):**
1. Run Request Data Recovery from POS System > Data In with the same inputs used by the legacy system_tools flow.
2. Verify the recovery request outcome (recovered data, status, logs).

**Test Result(s):**
1. Request Data Recovery behaves consistently with the legacy system_tools function.
2. The same recovery result is produced for the same input.
3. No errors displayed.

### 6.Regression Scope

- Legacy system_tools access path for the three functions should no longer be the only way to run them.
- Users who previously required the Administrator role for these functions should now be able to use role-based access control.
- Audit log coverage for all three migrated functions on the local transaction server.
- Other existing functions under POS System > Data In remain unaffected.
- Other entries under User Management > Access Control > System remain unaffected.

## Test Environment

- **Version**: Infrasys POS Platform 1.2.116.0 (containing the HERO-68359 change)
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
