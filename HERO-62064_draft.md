# POS Interface - Add the userId and userName for the 3 API calls to FAS Deposits System Test Report

## Functional Testing

### 1.Operator Identity in FAS API Payloads

#### 1.1.genDeposit returns userId and userName of the current POS operator
**Prerequisite(s):**
1. The FAS advance order interface is configured and connected to the FAS Deposits System
2. A POS operator is logged in and authorized to perform the advance order deposit function
3. Interface log capture is enabled

**Step(s):**
1. Log in to POS with the test operator account
2. Perform the deposit generation action that triggers the genDeposit API call
3. Check the outgoing genDeposit request payload in the interface log

**Test Result(s):**
1. The genDeposit payload includes userId with the Staff ID of the current operator
2. The payload includes userName with the Staff Name of the current operator
3. The values match the logged-in operator displayed in POS

#### 1.2.writeOffDeposit returns userId and userName of the current POS operator
**Prerequisite(s):**
1. The FAS advance order interface is configured and connected to the FAS Deposits System
2. An existing deposit that can be written off is available
3. Interface log capture is enabled

**Step(s):**
1. Log in to POS with the test operator account
2. Perform the write-off action that triggers the writeOffDeposit API call
3. Check the outgoing writeOffDeposit request payload in the interface log

**Test Result(s):**
1. The writeOffDeposit payload includes userId and userName of the current operator
2. The write-off is processed successfully by the FAS Deposits System

#### 1.3.voidTransaction returns userId and userName of the current POS operator
**Prerequisite(s):**
1. The FAS advance order interface is configured and connected to the FAS Deposits System
2. An existing transaction that can be voided is available
3. Interface log capture is enabled

**Step(s):**
1. Log in to POS with the test operator account
2. Perform the void action that triggers the voidTransaction API call
3. Check the outgoing voidTransaction request payload in the interface log

**Test Result(s):**
1. The voidTransaction payload includes userId and userName of the current operator
2. The transaction is voided successfully in the FAS Deposits System

### 2.Approval Scenario

#### 2.1.Approver identity is returned when approval from another user is required
**Prerequisite(s):**
1. The current operator is not authorized to perform the target advance order function and the function requires approval from another user
2. An authorized approver account is available

**Step(s):**
1. Log in with the unauthorized operator and trigger the genDeposit, writeOffDeposit or voidTransaction action
2. When the approval prompt appears, approve the function with the authorized approver account
3. Check the outgoing API payload in the interface log

**Test Result(s):**
1. The payload provides the user number and name of the user who approved the function
2. The approver identity is transmitted in the same identity fields so the deposit system records who actually performed and approved the action

#### 2.2.Unauthorized operator cancels the approval and no API call is sent
**Prerequisite(s):**
1. The current operator is not authorized to perform the target advance order function and approval is required

**Step(s):**
1. Trigger the advance order function with the unauthorized operator
2. Cancel or abort at the approval prompt

**Test Result(s):**
1. The function is not performed and no genDeposit, writeOffDeposit or voidTransaction API call is sent to the external party
2. No userId or userName is transmitted

### 3.Field Format Verification

#### 3.1.Modify user value and format follow the existing genDeposit field
**Prerequisite(s):**
1. The FAS advance order interface is configured with log capture enabled

**Step(s):**
1. Perform genDeposit and record the modify user field value and format
2. Perform writeOffDeposit and voidTransaction with modification scenarios
3. Compare the modify user field across the three payloads

**Test Result(s):**
1. writeOffDeposit and voidTransaction provide the modify user with the same value and format as the existing field in the genDeposit API
2. No format deviation or missing value is observed

#### 3.2.Staff name with special characters is transmitted correctly
**Prerequisite(s):**
1. A staff account whose name contains spaces, hyphens or non-ASCII characters is available

**Step(s):**
1. Log in with that staff account and perform genDeposit
2. Check the userName field in the outgoing payload

**Test Result(s):**
1. The full staff name is transmitted without truncation or encoding corruption
2. The FAS Deposits System receives and displays the name correctly

### 4.Functional Regression

#### 4.1.End-to-end advance order deposit flow
**Prerequisite(s):**
1. The FAS advance order interface is configured and connected
2. An authorized operator account is available

**Step(s):**
1. Create an advance order and generate the deposit through the standard POS flow
2. Check the deposit record in the FAS Deposits System

**Test Result(s):**
1. The deposit is created successfully end to end
2. The FAS system records who performed the payment action via userId and userName, ensuring auditability and reporting

#### 4.2.Write-off and void flow regression
**Prerequisite(s):**
1. Existing deposits and transactions are available for write-off and void operations

**Step(s):**
1. Write off a deposit through the standard flow
2. Void a transaction through the standard flow
3. Check the records in the FAS Deposits System

**Test Result(s):**
1. Both the write-off and the void operations succeed as before
2. The FAS system records the operator identity for both actions via userId and userName
