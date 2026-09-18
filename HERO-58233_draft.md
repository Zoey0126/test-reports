# HERO-58233 Test Report

## Functional Testing

### 1. Toggle Training Mode - Function Key Visibility

#### 1.1 Function Key "Toggle Training Mode" Is Displayed When Default Enter Training Mode Is Off and Training Mode Server Is Configured

**Prerequisite(s):**
1. A POS workstation with valid client setup "Training mode server" configured (non-empty value)
2. Client setup "Default Enter Training Mode" is turned off
3. User has permission to access Admin functions

**Step(s):**
1. Login to the POS workstation
2. Click button "Admin" to open the admin mode screen
3. Select tab "General"
4. Look for function key "Toggle Training Mode"

**Test Result(s):**
1. Function key "Toggle Training Mode" is visible in tab "General"
2. The function key label is correctly displayed
3. Other existing functions in the General tab are not affected

#### 1.2 Function Key "Toggle Training Mode" Is Displayed When Default Enter Training Mode Is On

**Prerequisite(s):**
1. A POS workstation with client setup "Default Enter Training Mode" turned on
2. User has permission to access Admin functions

**Step(s):**
1. Login to the POS workstation (system will enter training mode automatically)
2. Click button "Admin" to open the admin mode screen
3. Select tab "General"
4. Look for function key "Toggle Training Mode"

**Test Result(s):**
1. Function key "Toggle Training Mode" is visible in tab "General"
2. The workstation shows login page with "Training Mode" watermark
3. Other existing functions in the General tab are not affected

#### 1.3 Error Message When Training Mode Server Is Not Configured

**Prerequisite(s):**
1. A POS workstation where client setup "Training mode server" has empty or invalid URL
2. Client setup "Default Enter Training Mode" is turned off
3. User has permission to access Admin functions

**Step(s):**
1. Login to the POS workstation
2. Click button "Admin"
3. Select tab "General"
4. Look for function key "Toggle Training Mode"

**Test Result(s):**
1. Function key "Toggle Training Mode" is NOT displayed in tab "General"
2. No misleading function key is shown when training server is unavailable

### 2. Enter Training Mode via Toggle Function

#### 2.1 Workflow - User Without Authority Cannot Enter Training Mode

**Prerequisite(s):**
1. POS workstation with "Toggle Training Mode" function key visible
2. User account does NOT have authority to use "Toggle Training Mode" function
3. User is logged in at production server

**Step(s):**
1. Login to POS workstation as a user without the required authority
2. Click button "Admin"
3. Select tab "General"
4. Click function "Toggle Training Mode"

**Test Result(s):**
1. System validates the user's authority for "Toggle Training Mode"
2. Authority validation fails
3. System ends the workflow without switching to training mode
4. Appropriate error or denial message is displayed
5. No audit log entry is created for unauthorized attempts

#### 2.2 Workflow - User With Authority Successfully Enters Training Mode

**Prerequisite(s):**
1. POS workstation with valid "Training mode server" URL configured
2. Client setup "Default Enter Training Mode" is turned off
3. User account HAS authority to use "Toggle Training Mode" function
4. User is logged in at production server

**Step(s):**
1. Login to POS workstation as an authorized user
2. Click button "Admin"
3. Select tab "General"
4. Click function "Toggle Training Mode"
5. Observe the system behavior

**Test Result(s):**
1. System validates the authority control and passes
2. System adds an audit log for the "Toggle Training Mode" action
3. Client setup "Default Enter Training Mode" is turned on
4. System automatically logs out from the original (production) server
5. Login page appears with "Training Mode" watermark on top
6. User can successfully login to the training mode server
7. The workstation now operates in training mode

### 3. Leave Training Mode via Toggle Function

#### 3.1 Workflow - User Without Authority Cannot Leave Training Mode

**Prerequisite(s):**
1. POS workstation is currently in training mode (logged into training server)
2. Client setup "Default Enter Training Mode" is turned on
3. User account does NOT have authority to use "Toggle Training Mode" function

**Step(s):**
1. Login to POS workstation as a user without required authority while in training mode
2. Click button "Admin"
3. Select tab "General"
4. Click function "Toggle Training Mode"

**Test Result(s):**
1. System validates the user's authority for "Toggle Training Mode"
2. Authority validation fails
3. System ends the workflow without leaving training mode
4. Appropriate error or denial message is displayed
5. No audit log entry is created for unauthorized attempts

#### 3.2 Workflow - User With Authority Successfully Leaves Training Mode

**Prerequisite(s):**
1. POS workstation is currently in training mode (logged into training server)
2. Client setup "Default Enter Training Mode" is turned on
3. User account HAS authority to use "Toggle Training Mode" function

**Step(s):**
1. Login to POS workstation as an authorized user while in training mode
2. Click button "Admin"
3. Select tab "General"
4. Click function "Toggle Training Mode"
5. Observe the system behavior

**Test Result(s):**
1. System validates the authority control and passes
2. System adds an audit log for the "Toggle Training Mode" action
3. Client setup "Default Enter Training Mode" is turned off
4. System automatically logs out from the training server
5. Login page appears WITHOUT the "Training Mode" watermark
6. User can successfully login to the original (production) server
7. The workstation now operates in production mode

### 4. Audit Log and Setup Changes Verification

#### 4.1 Audit Log Records Toggle Training Mode Actions

**Prerequisite(s):**
1. POS workstation with valid training mode configuration
2. Authorized user account with permission to toggle training mode
3. Access to Audit Log viewer is available

**Step(s):**
1. As authorized user, perform a "Toggle Training Mode" action to enter training mode
2. After login to training server, navigate to Audit Log viewer
3. Locate the audit log entry for the toggle action
4. Now perform another "Toggle Training Mode" action to leave training mode
5. Navigate to Audit Log viewer again and locate the entry

**Test Result(s):**
1. Each toggle action generates an audit log entry
2. Audit log includes correct timestamp, operator username, and action type
3. Audit log clearly identifies whether entering or leaving training mode
4. No duplicate or missing audit entries for valid toggle operations

#### 4.2 Client Setup "Default Enter Training Mode" Is Correctly Updated

**Prerequisite(s):**
1. POS workstation with valid training mode configuration
2. Authorized user account with permission to toggle training mode

**Step(s):**
1. Before any toggle action, verify client setup "Default Enter Training Mode" is Off
2. Perform "Toggle Training Mode" to enter training mode
3. After successful switch, verify client setup "Default Enter Training Mode" has been changed to On
4. Perform "Toggle Training Mode" to leave training mode
5. After successful switch, verify client setup "Default Enter Training Mode" has been changed back to Off

**Test Result(s):**
1. After entering training mode, "Default Enter Training Mode" client setup value is On
2. After leaving training mode, "Default Enter Training Mode" client setup value is Off
3. The client setup value persists across workstation restarts
4. No data corruption occurs in the client setup file

### 5. Error Handling

#### 5.1 Toggle Training Mode When Training Server URL Is Invalid

**Prerequisite(s):**
1. POS workstation has client setup "Training mode server" configured with an invalid/unreachable URL
2. Client setup "Default Enter Training Mode" is turned off
3. Authorized user account with permission to toggle training mode

**Step(s):**
1. Login to POS workstation as authorized user
2. Click button "Admin"
3. Select tab "General"
4. Click function "Toggle Training Mode"

**Test Result(s):**
1. Function key should not be visible (invalid training server URL means it's hidden from UI)
2. If the function key is visible due to configuration timing, attempting to switch results in an appropriate error
3. Error message states that valid training server setting cannot be found
4. User is directed to check with system administrator
5. No partial state changes occur (no switch to broken training mode)
