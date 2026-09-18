# HERO-62878 Test Report

## Functional Testing

### 1. Change Table Audit Log Enhancement - Workstation

#### 1.1 Change Table via Workstation - Audit Log Description Shows From and To Table Information

**Prerequisite(s):**
1. A POS workstation with a valid old check (open check) exists in the system
2. The workstation has access to the "Change Table" function
3. Access to the Audit Log viewer is available to verify audit log entries

**Step(s):**
1. Login to the POS workstation
2. Open an existing old check associated with table "101"
3. Click function "Change Table"
4. Enter target table number "102" and click "OK"
5. Confirm the table change is successful
6. Navigate to Audit Log viewer and locate the audit log entry for this change table action

**Test Result(s):**
1. The audit log description displays: "Change table number from (101) to (102)"
2. The original table number and target table number are both present in the description
3. No error messages are displayed during the operation
4. The audit log entry is recorded with the correct timestamp and operator information

#### 1.2 Change Table with Table Extension - Audit Log Description Includes Extension

**Prerequisite(s):**
1. A POS workstation with a valid old check associated with table "107A" exists
2. The workstation has access to the "Change Table" function
3. Access to the Audit Log viewer is available

**Step(s):**
1. Login to the POS workstation
2. Open an existing old check associated with table "107A"
3. Click function "Change Table"
4. Enter target table number "107C" and click "OK"
5. Confirm the table change is successful
6. Navigate to Audit Log viewer and locate the audit log entry

**Test Result(s):**
1. The audit log description displays: "Change table number from (107A) to (107C)"
2. Both original and target table extensions (A, C) are included in the description
3. The table extension information is captured correctly for both source and target tables

#### 1.3 Change Table Failure - No Audit Log Entry Created

**Prerequisite(s):**
1. A POS workstation with a valid old check exists
2. A target table that cannot be assigned (e.g., already occupied with conflicting reservation) is identified
3. Access to the Audit Log viewer is available

**Step(s):**
1. Login to the POS workstation
2. Open an existing old check
3. Click function "Change Table"
4. Enter a target table number that will cause the change to fail
5. Observe the error handling behavior
6. Navigate to Audit Log viewer and check for any new entries

**Test Result(s):**
1. System displays an appropriate error message and aborts the action
2. No new audit log entry is created for the failed change table attempt
3. The original table assignment remains unchanged

### 2. Change Table Audit Log Enhancement - API Portal

#### 2.1 Change Table via API Portal - Audit Log Description Shows From and To Table Information

**Prerequisite(s):**
1. An API Portal client with valid authentication credentials
2. A valid old check exists in the system with table "101"
3. The API endpoint for "Change Table" is accessible

**Step(s):**
1. Send an API request to the Change Table endpoint for the target old check
2. Specify target table number "102" in the request payload
3. Confirm the API returns a successful response
4. Navigate to Audit Log viewer and locate the audit log entry

**Test Result(s):**
1. The API returns a success response with appropriate confirmation
2. The audit log description displays: "Change table number from (101) to (102)"
3. Both original and target table numbers are captured in the audit log
4. The audit log entry includes correct operator information from the API request

#### 2.2 Change Table via API Portal - Table Extension Included in Audit Log

**Prerequisite(s):**
1. An API Portal client with valid authentication credentials
2. A valid old check exists associated with table "201B"
3. The API endpoint for "Change Table" is accessible

**Step(s):**
1. Send an API request to the Change Table endpoint for the target old check
2. Specify target table number "201D" in the request payload
3. Confirm the API returns a successful response
4. Navigate to Audit Log viewer and locate the audit log entry

**Test Result(s):**
1. The API returns a success response
2. The audit log description displays: "Change table number from (201B) to (201D)"
3. Both table extensions are correctly included in the description

### 3. Void Paid Check / Force Void Paid Check - Table Change Audit Log

#### 3.1 Void Paid Check with Table Change - Audit Log Shows From and To Table

**Prerequisite(s):**
1. A POS workstation with access to Admin functions
2. A paid check exists that was originally on table "301" and changed to table "305" before payment
3. Access to Audit Log viewer is available

**Step(s):**
1. Login to the POS workstation with admin privileges
2. Click button "Admin"
3. Select tab "Check Operation"
4. Click function "Check Listing"
5. Select the target paid check
6. Click function "Void Paid Check"
7. Select void reason and confirm the void operation
8. Navigate to Audit Log viewer and locate the related audit log entry

**Test Result(s):**
1. The void paid check operation completes successfully
2. If the void operation triggers a table information update, the audit log description displays: "Change table number from (301) to (305)"
3. The table change audit log is recorded alongside the void operation audit log
4. Both audit entries have correct timestamps and operator information

#### 3.2 Force Void Paid Check with Table Change - Audit Log Correct

**Prerequisite(s):**
1. A POS workstation with access to Admin functions
2. A force-voidable paid check exists with a table change history
3. Access to Audit Log viewer is available

**Step(s):**
1. Login to the POS workstation with admin privileges
2. Click button "Admin"
3. Select tab "Check Operation"
4. Click function "Check Listing"
5. Select the target paid check
6. Click function "Force Void Paid Check"
7. Select void reason and confirm the force void operation
8. Navigate to Audit Log viewer and locate the table change audit log entry

**Test Result(s):**
1. The force void paid check operation completes successfully
2. The audit log for table change (if triggered) displays the full "from (original) to (target)" format
3. All audit log entries are correctly recorded

### 4. Additional Audit Log Enhancements

#### 4.1 Add Discount Code When Apply Discount - Audit Log Verification

**Prerequisite(s):**
1. A POS workstation with discount functions configured
2. Various discount codes are available for testing
3. Access to Audit Log viewer is available

**Step(s):**
1. Create a new check and order items
2. Apply a discount using a specific discount code
3. Complete or close the check as appropriate
4. Navigate to Audit Log viewer and locate the discount application audit log entry

**Test Result(s):**
1. The discount application audit log includes the discount code information
2. The discount code is displayed correctly in the audit log description
3. No duplicate or missing audit log entries are created

#### 4.2 Add User No. When Apply Print Check - Audit Log Verification

**Prerequisite(s):**
1. A POS workstation with print check functionality available
2. Multiple users are configured in the system
3. Access to Audit Log viewer is available

**Step(s):**
1. Login to POS workstation as User A (user number 001)
2. Create a check and order items
3. Click function to print the check
4. Navigate to Audit Log viewer and locate the print check audit log entry

**Test Result(s):**
1. The print check audit log includes the user number of the operator who initiated the print
2. The user number is correctly captured and displayed in the audit log description
3. Audit log entry has accurate timestamp and operation details
