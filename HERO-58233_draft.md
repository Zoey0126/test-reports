# POS - Training Mode - Relocate switch training mode to Admin page in Operation screen Test Report

## Functional Testing

### 1.Toggle Training Mode Function Key Visibility

#### 1.1.Function Key Shown When Training Server Is Configured And Default Enter Is Off
**Prerequisite(s):**
1. Windows POS client is used
2. Client setup "Training mode server" has a non-empty valid URL
3. Client setup "Default Enter Training Mode" is turned off
4. The logged-in user has authority for function "Toggle Training Mode"

**Step(s):**
1. Login to the POS workstation on the original server
2. Click button "Admin"
3. Open tab "General"
4. Observe whether function key "Toggle Training Mode" is displayed

**Test Result(s):**
1. Function key "Toggle Training Mode" is displayed on Admin page General tab
2. The function key is available without opening workstation configuration

#### 1.2.Function Key Shown When Default Enter Training Mode Is On
**Prerequisite(s):**
1. Client setup "Default Enter Training Mode" is turned on
2. The workstation is currently in training mode

**Step(s):**
1. Login to the training mode server
2. Click button "Admin"
3. Open tab "General"
4. Observe whether function key "Toggle Training Mode" is displayed

**Test Result(s):**
1. Function key "Toggle Training Mode" is displayed so the user can switch back to the original server

#### 1.3.Function Key Hidden When Training Server Is Empty And Default Enter Is Off
**Prerequisite(s):**
1. Client setup "Default Enter Training Mode" is turned off
2. Client setup "Training mode server" is empty

**Step(s):**
1. Login to the POS workstation
2. Click button "Admin"
3. Open tab "General"
4. Observe whether function key "Toggle Training Mode" is displayed

**Test Result(s):**
1. Function key "Toggle Training Mode" is not displayed
2. Users cannot enter training mode from Admin when no training server is configured

### 2.Enter Training Mode Workflow

#### 2.1.Successful Switch To Training Mode
**Prerequisite(s):**
1. Client setup "Training mode server" has a valid URL
2. Client setup "Default Enter Training Mode" is turned off
3. The logged-in user has authority for "Toggle Training Mode"

**Step(s):**
1. Login POS workstation
2. Click button "Admin"
3. Click function "Toggle Training Mode"
4. Observe logout, login page and subsequent login

**Test Result(s):**
1. Authority validation passes
2. System adds an audit log for function "Toggle Training Mode"
3. System turns on client setup "Default Enter Training Mode"
4. System logs out of the original server automatically
5. Client shows the login page with "Training Mode" watermark on top
6. After the user logs in, the system logs in to the training mode server

#### 2.2.Authority Denied When Entering Training Mode
**Prerequisite(s):**
1. Function key "Toggle Training Mode" is visible
2. The logged-in user does not have authority for "Toggle Training Mode"

**Step(s):**
1. Click "Admin"
2. Click function "Toggle Training Mode"

**Test Result(s):**
1. Authority validation fails
2. The workstation does not switch to training mode
3. Client setup "Default Enter Training Mode" remains off
4. No logout to the training server occurs

### 3.Leave Training Mode Workflow

#### 3.1.Successful Switch Back To Original Server
**Prerequisite(s):**
1. The workstation is in training mode
2. Client setup "Default Enter Training Mode" is turned on
3. The logged-in user has authority for "Toggle Training Mode"

**Step(s):**
1. Login POS workstation in training mode
2. Click button "Admin"
3. Click function "Toggle Training Mode"
4. Observe logout, login page and subsequent login

**Test Result(s):**
1. Authority validation passes
2. System adds an audit log for function "Toggle Training Mode"
3. System turns off client setup "Default Enter Training Mode"
4. System logs out of the training server automatically
5. Client shows the login page and hides the "Training Mode" watermark
6. After the user logs in, the system logs in to the original server

#### 3.2.Authority Denied When Leaving Training Mode
**Prerequisite(s):**
1. The workstation is in training mode
2. The logged-in user does not have authority for "Toggle Training Mode"

**Step(s):**
1. Click "Admin"
2. Click function "Toggle Training Mode"

**Test Result(s):**
1. Authority validation fails
2. The workstation remains in training mode
3. The "Training Mode" watermark remains displayed

### 4.Invalid Training Server Setting
**Prerequisite(s):**
1. Training mode server is not configured or the URL is invalid
2. User attempts to use training mode switch where the product still surfaces the function or an equivalent error path

**Step(s):**
1. Open Admin page
2. Attempt to enter training mode when training server setting is missing or invalid

**Test Result(s):**
1. System shows error message "can not find valid training server setting, please check with system administrator" when the training server is missing or invalid
2. The workstation does not switch server

### 5.Client Setup Rename
**Prerequisite(s):**
1. Workstation / client setup page is accessible to IT or administrator

**Step(s):**
1. Open client setup
2. Locate the previous setting named "Enable Training Mode Server"

**Test Result(s):**
1. The client setup name is changed to "Default Enter Training Mode"
2. Existing training server URL configuration remains available

### 6.Audit Log For Toggle Training Mode
**Prerequisite(s):**
1. Audit or action log inquiry is available
2. A user with Toggle Training Mode authority is available

**Step(s):**
1. Enter training mode using Toggle Training Mode
2. Leave training mode using Toggle Training Mode
3. Open audit or action logs and search for "Toggle Training Mode"

**Test Result(s):**
1. Both enter and leave operations are recorded in audit or action logs
2. Log entries identify the user and the function "Toggle Training Mode"

## Compatibility Testing

### 1.Windows Client Only
**Prerequisite(s):**
1. Windows Client with Training Mode is available

**Step(s):**
1. Repeat enter and leave training mode on Windows Client

**Test Result(s):**
1. The enhancement is available for Training Mode and Windows Client
2. Toggle Training Mode works on the Admin page General tab

## Test Environment
- **Version**: v1.0.0
- **Device**: Windows POS Client
- **Environment**: HQ Test Environment

## Appendix

### Acceptance Criteria(from JIRA)
1. Function key allows user to enter training mode from production manager/Admin screen with user rights control
2. Function key allows user to exit training mode from training mode manager/Admin screen with user rights control
3. If workstation has no training server configured or invalid URL, show error "can not find valid training server setting, please check with system administrator"
4. Operations are recorded into audit or action logs
5. Client setup name changes from "Enable Training Mode Server" to "Default Enter Training Mode"
6. Toggle Training Mode appears when Default Enter Training Mode is off and Training mode server has a value, or when Default Enter Training Mode is on
