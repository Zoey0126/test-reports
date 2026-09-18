# POS Interface - Support for Multiple Advance Orders applied in the same POS check Test Report

## Functional Testing

### 1.Retrieve And Attach Multiple Advance Orders

#### 1.1.Retrieve Multiple Advance Orders To Same Check
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. At least two valid advance order records exist in the external party system
3. A check is open on the POS workstation

**Step(s):**
1. Open a check
2. Click function "Retrieve Advance Order"
3. Follow the existing workflow to search and retrieve the first target advance order
4. Click function "Retrieve Advance Order" again
5. Follow the existing workflow to search and retrieve a second target advance order
6. Verify that both advance orders are attached to the check

**Test Result(s):**
1. The first advance order is retrieved and attached to the check following the existing workflow
2. When retrieving a second advance order while one is already attached, the system continues retrieving the new advance order and attaching it to the check without prompting for confirmation or clearing previously attached advance orders
3. Both advance orders are successfully attached to the same check

#### 1.2.Search Advance Order To Same Check
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. At least two valid advance order records exist in the external party system
3. A check is open on the POS workstation

**Step(s):**
1. Open a check
2. Click function "Search Advance Order"
3. Follow the existing workflow to search and retrieve the first target advance order
4. Click function "Search Advance Order" again
5. Follow the existing workflow to search and retrieve a second target advance order
6. Verify that both advance orders are attached to the check

**Test Result(s):**
1. The first advance order is retrieved and attached to the check following the existing workflow
2. When retrieving a second advance order while one is already attached, the system continues retrieving the new advance order and attaching it to the check without any confirmation or process of clearing previously attached advance orders
3. Both advance orders are successfully attached to the same check

#### 1.3.First Advance Order Attachment To Empty Check
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. At least one valid advance order record exists in the external party system
3. A check is open with no advance orders attached

**Step(s):**
1. Open a check with no advance orders attached
2. Click function "Retrieve Advance Order" or "Search Advance Order"
3. Follow the existing workflow to retrieve the new advance order

**Test Result(s):**
1. The system follows the existing workflow to retrieve the new advance order and attach it to the check
2. The advance order is attached to the check successfully

### 2.Printing Variables For Multiple Advance Orders

#### 2.1.Verify Printing Variables On Guest Check
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. Multiple advance orders are attached to a check
3. The guest check print format is configured with the new printing variables

**Step(s):**
1. Open a check with multiple advance orders attached
2. Click function "Print and Paid"
3. Observe the printed guest check
4. Verify that all advance order printing variables are displayed with the correct values

**Test Result(s):**
1. The printed guest check displays all advance order printing variables for each attached advance order
2. The following variables are populated for each advance order up to a maximum count of 10:
   - CheckAdvanceOrderIntfBalanceAmount
   - CheckAdvanceOrderIntfCustomerFaxNumber
   - CheckAdvanceOrderIntfCustomerName
   - CheckAdvanceOrderIntfCustomerPhone
   - CheckAdvanceOrderIntfDepositAmount
   - CheckAdvanceOrderIntfDepositNumber
   - CheckAdvanceOrderIntfPickupDate
   - CheckAdvanceOrderIntfNote1
   - CheckAdvanceOrderIntfNote2
   - CheckAdvanceOrderIntfUsedAmount
3. The values for each variable are correctly displayed for each advance order attached to the check

#### 2.2.Verify Printing Variables On Receipt
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. Multiple advance orders are attached to a check
3. The receipt print format is configured with the new printing variables

**Step(s):**
1. Open a check with multiple advance orders attached
2. Click function "Print and Paid"
3. Follow the existing workflow to settle the check
4. Observe the printed receipt
5. Verify that all advance order printing variables are displayed with the correct values

**Test Result(s):**
1. The printed receipt displays all advance order printing variables for each attached advance order
2. The values for each variable are correctly displayed for each advance order attached to the check
3. The receipt is printed successfully without any error

### 3.Check Settlement And Write Off Deposit API

#### 3.1.Verify WriteOffDeposit API Call With Multiple Advance Orders
**Prerequisite(s):**
1. The FAS advance order interface is configured and available
2. Multiple advance orders are attached to a check
3. The default payment method is specified in the interface setup

**Step(s):**
1. Open a check with multiple advance orders attached
2. Click function "Print and Paid"
3. Follow the existing workflow to settle the check with the default payment method specified in interface setup
4. Observe the external API call triggered by the system
5. Follow the existing handling to save the check

**Test Result(s):**
1. The check is settled successfully with the default payment method
2. The system triggers the external API call "writeOffDeposit" with information of all advance orders retrieved on the check
3. The check is saved following the existing handling without any error

### 4.Printing Variable Setup Configuration

#### 4.1.Configure Printing Variables For Guest Check And Receipt
**Prerequisite(s):**
1. The Infrasys POS platform is accessible
2. An existing check or receipt print format is available for editing

**Step(s):**
1. Go to Infrasys POS platform: POS System -> Printing Setup -> Print Formats
2. Click "Add New" to add a record or select an existing check or receipt format
3. Select the "Print Format" tab
4. Add the printing variables:
   - CheckAdvanceOrderIntfBalanceAmount1~10
   - CheckAdvanceOrderIntfCustomerFaxNumber1~10
   - CheckAdvanceOrderIntfCustomerName1~10
   - CheckAdvanceOrderIntfCustomerPhone1~10
   - CheckAdvanceOrderIntfDepositAmount1~10
   - CheckAdvanceOrderIntfDepositNumber1~10
   - CheckAdvanceOrderIntfPickupDate1~10
   - CheckAdvanceOrderIntfNote11~10
   - CheckAdvanceOrderIntfNote21~10
   - CheckAdvanceOrderIntfUsedAmount1~10
5. Define other printing content accordingly
6. Click "Save" to save the record

**Test Result(s):**
1. The printing variables are added successfully to the print format
2. The variables are saved with the correct loop configuration and available slip type (Guest Check and Receipt)
3. The print format record is saved without error

### 5.Regression Scope

The following related modules and scenarios must be retested after the change:

- FAS advance order interface retrieve workflow
- FAS advance order interface search workflow
- Existing single advance order attachment workflow continues to work as before
- Guest check printing with advance order variables
- Receipt printing with advance order variables
- Check settlement with default payment method specified in interface setup
- WriteOffDeposit external API call with single and multiple advance orders
- FAS Taiwan Deposits System API integration continues to operate correctly
