# HERO-66602 POS - Missing of Membership Number in Audit Log for Member Payment Settlement Test Report

## Functional Testing

### 1. Membership Number in Audit Log for Member Payment

#### 1.1. Verify Membership Number is Recorded in Audit Log When Settling with Member Payment

**Prerequisite(s):**
  1. Internal member module is available with member profile management turned on
  2. A member type payment method is configured under POS System > Payment Setup > Payment Methods (Type = Member)
  3. A Member Profile setup is configured under POS System > Ordering Setup > Config by Location (Support = Yes, Target membership interface = Target interface)
  4. At least one member exists in the system

**Step(s):**
  1. Log in to POS client
  2. Open a check and add items
  3. Click button "Paid"
  4. Select the payment with payment type = member
  5. Search for a member
  6. Select the member and click button "Set Member"
  7. Click "Enter" to settle the check
  8. Go to Platform > System Management > Audit Control and search for the audit log record

**Test Result(s):**
  1. The check is settled successfully with member payment
  2. The audit log record is created for the member payment settlement
  3. The audit log description includes the Membership Number of the member used for payment
  4. The Membership Number is correctly matching the selected member's number
  5. No errors are displayed

#### 1.2. Verify Membership Number is Not Recorded for Non-Member Payment

**Prerequisite(s):**
  1. A non-member payment method (e.g., Cash) is configured

**Step(s):**
  1. Log in to POS client
  2. Open a check and add items
  3. Click button "Paid"
  4. Select a non-member payment (e.g., Cash)
  5. Settle the check
  6. Go to Platform > System Management > Audit Control and search for the audit log record

**Test Result(s):**
  1. The check is settled successfully with non-member payment
  2. The audit log record is created but does not include a Membership Number
  3. The audit log behaves as before for non-member payments
  4. No errors are displayed

### 2. Audit Control Report POS043 Display

#### 2.1. Verify Membership Number is Displayed in Audit Control Report POS043

**Prerequisite(s):**
  1. At least one member payment settlement has been completed (from 1.1)
  2. Audit Control Report POS043 is available

**Step(s):**
  1. Navigate to Report Console
  2. Open Audit Control Report POS043
  3. Set parameters to include the date range of the member payment settlement
  4. Run the report
  5. Locate the audit log record for the member payment
  6. Verify the Membership Number column

**Test Result(s):**
  1. POS043 report loads successfully
  2. The Membership Number is displayed for the member payment settlement record
  3. The Membership Number matches the member used during payment
  4. Non-member payment records do not show a Membership Number
  5. No errors are displayed

#### 2.2. Verify Membership Number is Not Displayed for Non-Member Payment in POS043

**Prerequisite(s):**
  1. At least one non-member payment settlement exists (from 1.2)

**Step(s):**
  1. Open Audit Control Report POS043
  2. Set parameters to include both member and non-member payment records
  3. Run the report
  4. Compare member payment records with non-member payment records

**Test Result(s):**
  1. Member payment records show the Membership Number
  2. Non-member payment records do not show a Membership Number
  3. The distinction is clear and correct
  4. No errors are displayed

### 3. Multiple Member Payment Scenarios

#### 3.1. Verify Membership Number is Recorded for Multiple Member Payments

**Prerequisite(s):**
  1. Multiple members exist in the system
  2. Member payment method is configured

**Step(s):**
  1. Open a check and add items
  2. Click "Paid"
  3. Select member payment and use Member A
  4. Settle the first payment
  5. Open another check
  6. Select member payment and use Member B
  7. Settle the second payment
  8. Check audit log records for both payments in POS043

**Test Result(s):**
  1. Both checks are settled successfully
  2. Audit log for first payment shows Member A's Membership Number
  3. Audit log for second payment shows Member B's Membership Number
  4. Each Membership Number correctly corresponds to the respective member
  5. No errors are displayed

#### 3.2. Verify Membership Number is Recorded for Split Check with Member Payment

**Prerequisite(s):**
  1. POS System > Ordering Setup > Config by Location > Support Partial Payment = Yes
  2. Member payment method is configured

**Step(s):**
  1. Open a check and add items
  2. Click "Paid"
  3. Select a non-member payment (e.g., Cash) for partial payment
  4. Click "Finish" to save the partial payment
  5. Click "Exit" back to floor plan
  6. Open the check again
  7. Select member payment for the remaining balance
  8. Settle the check
  9. Check audit log records in POS043

**Test Result(s):**
  1. Check is settled successfully with split payments
  2. Audit log for the member payment portion includes the Membership Number
  3. Audit log for the non-member payment portion does not include a Membership Number
  4. No errors are displayed

## Compatibility Testing

### 4. Browser Compatibility

#### 4.1. Verify Audit Control Report POS043 Displays Membership Number Across Browsers

**Prerequisite(s):**
  1. At least one member payment settlement has been completed
  2. Access to Chrome, Firefox, and Edge browsers

**Step(s):**
  1. Open POS043 in Google Chrome and verify Membership Number is displayed
  2. Open POS043 in Mozilla Firefox and verify Membership Number is displayed
  3. Open POS043 in Microsoft Edge and verify Membership Number is displayed

**Test Result(s):**
  1. Membership Number is displayed correctly in Chrome
  2. Membership Number is displayed correctly in Firefox
  3. Membership Number is displayed correctly in Edge
  4. No layout or rendering issues across browsers

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
