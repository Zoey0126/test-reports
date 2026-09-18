# POS Interface - HTNG (HotelIFC/Shiji Box) - Block check modifications after partial payment Test Report

## Functional Testing

### 1.Setup Configuration For Block Check Modification

#### 1.1.Enable Block Updating Check After Partial Payment Setup
**Prerequisite(s):**
1. The Infrasys Cloud platform is accessible
2. A HTNG (HotelIFC/Shiji Box) PMS interface record exists under Interface Control -> Interfaces
3. The reference document "Infrasys Cloud POS - HTNG (Shiji Type) PMS Setup and Workflow Document v1.0.0.15" is available

**Step(s):**
1. Go to Infrasys Cloud platform: Interface Control -> Interfaces
2. Edit the target HTNG pms interface record
3. Locate the "Block Updating Check After Partial Payment (Setup 1)" field under Payment Setting
4. Set the value to "Yes"
5. Click "Save" to save the record

**Test Result(s):**
1. The setup field "Block Updating Check After Partial Payment (Setup 1)" is available with options No (Default) and Yes
2. The setup value is saved as "Yes" successfully
3. The configuration is applied to the HTNG PMS interface

### 2.Open Check Workflow After Partial Payment

#### 2.1.Open Check Screen Behavior With Setup 1 Set To Yes
**Prerequisite(s):**
1. Setup 1 "Block Updating Check After Partial Payment" is set to "Yes"
2. An existing check exists that has been partially paid with HTNG PMS interface payment

**Step(s):**
1. Open the old check that was partially paid with HTNG PMS interface payment
2. Observe the screen that the system displays

**Test Result(s):**
1. The system displays the check review screen with "Print" and "Paid Check" functions instead of the ordering panel
2. The ordering panel is not shown because the check is locked from modification

#### 2.2.Open Check Screen Behavior With Setup 1 Set To No Or Not Existing
**Prerequisite(s):**
1. Setup 1 "Block Updating Check After Partial Payment" is set to "No" or does not exist
2. An existing check exists that has been partially paid with HTNG PMS interface payment

**Step(s):**
1. Open the old check that was partially paid with HTNG PMS interface payment
2. Observe the screen that the system displays

**Test Result(s):**
1. The system follows the existing workflow and displays the ordering panel as usual
2. No restriction is applied to the check modifications

#### 2.3.Print And Paid Check Functions On Check Review Screen
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. The check review screen with "Print" and "Paid Check" functions is displayed

**Step(s):**
1. Click the "Print" button on the check review screen
2. Return to the table floor plan
3. Reopen the same check
4. Click the "Paid check" button on the check review screen

**Test Result(s):**
1. Clicking "Print" prints the check and returns to the table floor plan
2. Clicking "Paid check" opens the check in cashier for further payment

### 3.Split Item To Other Table And Split Item With Quantity

#### 3.1.Block Split Item When Target Check Is Partially Paid With Setup 1 Yes
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. An existing check exists that has been partially paid with HTNG PMS interface payment
3. Another check is open for the split item workflow

**Step(s):**
1. Open an old check
2. Click function "Split Item To Other Table" or "Split Item With Quantity"
3. Select the item to be split
4. Input the table number of the check partially paid with HTNG PMS interface payment

**Test Result(s):**
1. The system prompts an error dialog "Action not allow for check with HTNG PMS payment"
2. The split item operation is blocked and no change is applied to the partially paid check

#### 3.2.Allow Split Item When Setup 1 Is No Or Not Existing
**Prerequisite(s):**
1. Setup 1 is set to "No" or does not exist
2. An existing check exists that has been partially paid with HTNG PMS interface payment

**Step(s):**
1. Open an old check
2. Click function "Split Item To Other Table" or "Split Item With Quantity"
3. Select the item to be split
4. Input the table number of the check partially paid with HTNG PMS interface payment

**Test Result(s):**
1. The system continues the existing workflow for split item without any error dialog
2. The split item operation proceeds normally

### 4.Split Table Workflow

#### 4.1.Hide Partially Paid Tables From Split Table Available List
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. At least one check exists that has been partially paid with HTNG PMS interface payment
3. Another check is open for the split table workflow

**Step(s):**
1. Open an old check
2. Click function "Split Table"
3. Confirm the split table action in the confirmation dialog
4. Observe the list of available tables shown by the system

**Test Result(s):**
1. The system shows the confirmation dialog for split table
2. After clicking "Yes", the system shows the list of available tables
3. The tables with checks partially paid with HTNG PMS interface payment are hidden from the available tables list

#### 4.2.Show All Tables When Setup 1 Is No Or Not Existing
**Prerequisite(s):**
1. Setup 1 is set to "No" or does not exist
2. At least one check exists that has been partially paid with HTNG PMS interface payment

**Step(s):**
1. Open an old check
2. Click function "Split Table"
3. Confirm the split table action
4. Observe the list of available tables

**Test Result(s):**
1. The system shows the list of available tables including those with partially paid checks
2. No tables are hidden from the available tables list

### 5.Partial Payment Trigger Definition

#### 5.1.Activation Of Restriction After First Completed Payment
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. A check exists with no completed payment transactions
3. The HTNG PMS interface payment method is available

**Step(s):**
1. Open the check with no completed payments
2. Apply a partial tender via HTNG PMS interface payment
3. Attempt to perform any modification on the check (e.g. add item, delete item, change quantity, apply discount, change cover count)

**Test Result(s):**
1. The check modification restriction is activated immediately upon settlement of the partial tender
2. The system displays the warning message "Modifications are not allowed after partial payment"
3. The modification action is blocked and no change is applied to the check

#### 5.2.Lift Restriction When Payment Is Voided
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. A check exists that has one completed partial payment via HTNG PMS interface
3. No additional payments have been made after the partial payment

**Step(s):**
1. Open the partially paid check
2. Use "Void Payment" to void the previous partial payment
3. Attempt to perform any modification on the check

**Test Result(s):**
1. The void payment is processed successfully
2. The restriction is lifted and modifications to the check are allowed again
3. No warning message is displayed after the payment is voided

### 6.Payment Action Allowed After Partial Payment

#### 6.1.Additional Payment And Complete Remaining Balance
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. A check exists that has been partially paid via HTNG PMS interface payment
3. A remaining balance exists on the check

**Step(s):**
1. Open the partially paid check
2. Click "Paid check" to enter the cashier panel
3. Apply a further payment to settle the remaining balance

**Test Result(s):**
1. The payment action is allowed and proceeds normally
2. The remaining balance is settled and the check is completed
3. No warning message is displayed for the payment action

### 7.Restriction Applies To All User Roles

#### 7.1.Verify Restriction For All User Roles
**Prerequisite(s):**
1. Setup 1 is set to "Yes"
2. A check exists that has been partially paid via HTNG PMS interface payment
3. User accounts are available for cashier, server, supervisor and manager roles

**Step(s):**
1. Sign in as a cashier and attempt to modify the partially paid check
2. Sign in as a server and attempt to modify the partially paid check
3. Sign in as a supervisor and attempt to modify the partially paid check
4. Sign in as a manager and attempt to modify the partially paid check

**Test Result(s):**
1. The modification is blocked for the cashier role with the warning message "Modifications are not allowed after partial payment"
2. The modification is blocked for the server role with the same warning message
3. The modification is blocked for the supervisor role with the same warning message
4. The modification is blocked for the manager role with the same warning message
5. No override prompt, manager authorization flow or bypass mechanism is presented or available for any role

### 8.Checks With No Payment Or Fully Paid

#### 8.1.Checks With Zero Completed Payments
**Prerequisite(s):**
1. A check exists with zero completed payment transactions

**Step(s):**
1. Open the check with no payments
2. Attempt to perform any modification on the check

**Test Result(s):**
1. All modifications to the check are allowed as normal
2. No warning message is displayed

#### 8.2.Fully Paid And Closed Checks
**Prerequisite(s):**
1. A check exists that has been fully paid and closed

**Step(s):**
1. Open the fully paid and closed check
2. Observe the check state and modification rules

**Test Result(s):**
1. The check is in a closed state and standard closed-check modification rules apply independently
2. The block modification after partial payment restriction does not affect fully paid and closed checks

### 9.Regression Scope

The following related modules and scenarios must be retested after the change:

- HTNG PMS interface posting workflow continues to operate correctly
- Existing HTNG posting calculation accuracy remains unchanged
- Partial payment workflow via HTNG PMS interface payment
- Void payment workflow on partially paid checks
- Split Item To Other Table and Split Item With Quantity functions
- Split Table function and available table selection
- All user roles cashier, server, supervisor and manager interaction with partially paid checks
