# HERO-69622 Bug Fix Test Report - General V3 Membership Interface Auto Triggers Member Enquiry Unexpectedly

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Member Enquiry Triggered With Empty Setup and Empty Payment Code

#### F1.1.Verify General V3 Member Enquiry Is Not Prompted When Interface Setup and Payment Code Are Empty
**Prerequisite(s):**
1. General V3 membership interface is configured and available on the POS station
2. Interface setup "Auto Trigger Member Enquiry If No Member Attached Payment List" is set to empty: POS System > Interface Control > Interfaces > target General V3 membership interface
3. Payment method setup is empty: POS System > Payment Setup > Payment Methods > target payment method > Code = empty

**Step(s):**
1. Open a printed check that is not attached with any interface
2. Click "Paid" to go to the cashier screen
3. Select the target payment method whose payment code setup is empty
4. Observe whether the General V3 member enquiry screen is prompted
5. Complete the payment and settle the check

**Reproduction Result(s):**
1. The General V3 member enquiry screen is prompted even though the interface setup "Auto Trigger Member Enquiry If No Member Attached Payment List" is empty
2. The operator has to close or cancel the member enquiry screen before the payment can continue
3. The payment workflow is interrupted and the check cannot be settled in the existing flow

**Fix Result(s):**
1. The General V3 member enquiry screen is not prompted when the interface setup is empty and the selected payment code is empty
2. The system directly follows the existing payment workflow and settles the check
3. No unexpected member enquiry request is sent to the General V3 interface

#### F1.2.Verify Existing Payment Workflow Is Followed For All Payment Methods With Empty Code
**Prerequisite(s):**
1. Interface setup "Auto Trigger Member Enquiry If No Member Attached Payment List" is empty on the General V3 membership interface
2. At least three payment methods are configured with empty Code setup, including cash, credit card and a third-party payment method

**Step(s):**
1. Open a printed check with no interface attached
2. Click "Paid" to go to the cashier screen
3. Select each payment method with empty code setup one by one
4. Observe the behaviour of the member enquiry screen for each payment method

**Reproduction Result(s):**
1. The General V3 member enquiry screen is prompted for every payment method with empty code setup
2. The member enquiry screen cannot be bypassed without cancelling the enquiry

**Fix Result(s):**
1. No member enquiry screen is prompted for any payment method with empty code setup
2. Each payment method follows the existing payment workflow as configured
3. The check can be settled successfully for each payment method

#### F1.3.Verify Member Enquiry Is Still Prompted When Setup Is Configured
**Prerequisite(s):**
1. Interface setup "Auto Trigger Member Enquiry If No Member Attached Payment List" is configured with the target payment method list
2. The target payment method has a valid payment code setup

**Step(s):**
1. Open a printed check with no interface attached
2. Click "Paid" to go to the cashier screen
3. Select the target payment method that is listed in the interface setup

**Reproduction Result(s):**
1. The member enquiry screen is prompted for the configured payment method
2. The member enquiry screen is prompted as designed, so the original issue is limited to the empty setup scenario

**Fix Result(s):**
1. The General V3 member enquiry screen is still prompted for the configured payment method
2. The member enquiry screen is prompted as designed and the configured workflow is not affected by the fix
3. The enquiry result is returned and applied to the check correctly

#### F1.4.Verify Check Settlement Result After the Fix
**Prerequisite(s):**
1. Interface setup "Auto Trigger Member Enquiry If No Member Attached Payment List" is empty
2. The selected payment method has an empty Code setup
3. Payment posting to the backend is enabled

**Step(s):**
1. Open a printed check, order items and go to cashier
2. Select the payment method with empty code setup
3. Complete the payment
4. Check the check status, payment record and backend posting result

**Reproduction Result(s):**
1. The member enquiry screen blocks the payment flow, so the settlement cannot be completed normally
2. The payment record is incomplete or requires the operator to restart the payment workflow

**Fix Result(s):**
1. The check is settled successfully without any member enquiry prompt
2. The payment record is created with the correct payment method, amount and check reference
3. The backend posting result is correct and no General V3 member enquiry request is generated

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- General V3 membership interface member enquiry for payment methods that are configured in "Auto Trigger Member Enquiry If No Member Attached Payment List"
- General V3 member enquiry by member card, phone number and member search
- Interface setup maintenance for "Auto Trigger Member Enquiry If No Member Attached Payment List" (add, edit and clear the value)
- Payment method setup for Code field (add, edit and clear the value)
- Payment workflow for cash, credit card and third-party payment methods on a printed check
- Payment workflow for a check that is attached with a member through other membership interfaces
- Check settlement, payment recording and backend posting after member enquiry is skipped
- Existing payment functions such as Split Payment, Paid and Print And Paid on a printed check
