# LPS voucher interface - Triggers void posting incorrectly if fails to redeem payment voucher Test Report

## Functional Testing

### 1.LPS Voucher Redeem Failure Handling

#### 1.1.Void posting is not triggered when redeem coupon API returns error

**Prerequisite(s):**
1. The build containing the HERO-67008 fix is deployed to the test POS environment
2. An LPS Voucher voucher interface is configured and attached to a payment method
3. A printed check is available for settlement
4. The external party LPS voucher API endpoint is reachable but can be made to return an error response

**Step(s):**
1. Open a printed check on the POS workstation
2. Click function "Paid" to enter the cashier panel
3. Select the payment method attached with the LPS Voucher voucher interface
4. Follow the existing workflow to perform coupon enquiry and apply the coupon
5. Save the payment and settle the check so the system triggers the redeem coupon API to the external party
6. Configure the external party to return an error response for the redeem coupon API call
7. Observe the system behaviour after the external party returns the error
8. Check the POS logs and the external party API call log for any void redeem API call

**Reproduction Result(s):**
1. The system cannot save the payments after the redeem coupon API returns an error
2. The system performs void posting to the LPS Voucher voucher interface as a rollback
3. The external party receives an unexpected void redeem API call even though the failure was caused by the redeem coupon API itself

**Fix Result(s):**
1. The system does not trigger the external party void redeem API call when the failure of saving the payment is caused by the redeem coupon API error from the LPS Voucher voucher interface
2. The system handles the redeem coupon API error gracefully without performing an unnecessary void posting rollback
3. No void redeem API call is recorded in the external party API call log
4. The check remains in the expected state and the operator is informed of the redeem failure

#### 1.2.Void posting is not triggered when redeem coupon API returns timeout

**Prerequisite(s):**
1. The build containing the HERO-67008 fix is deployed to the test POS environment
2. An LPS Voucher voucher interface is configured and attached to a payment method
3. A printed check is available for settlement
4. The external party LPS voucher API endpoint can be configured to delay its response beyond the configured timeout

**Step(s):**
1. Open a printed check on the POS workstation
2. Click function "Paid" to enter the cashier panel
3. Select the payment method attached with the LPS Voucher voucher interface
4. Follow the existing workflow to perform coupon enquiry and apply the coupon
5. Save the payment and settle the check so the system triggers the redeem coupon API to the external party
6. Configure the external party to delay its response so the redeem coupon API call times out
7. Observe the system behaviour after the redeem coupon API times out
8. Check the POS logs and the external party API call log for any void redeem API call

**Reproduction Result(s):**
1. The system cannot save the payments after the redeem coupon API times out
2. The system performs void posting to the LPS Voucher voucher interface as a rollback
3. The external party receives an unexpected void redeem API call even though the failure was caused by the redeem coupon API timeout

**Fix Result(s):**
1. The system does not trigger the external party void redeem API call when the failure of saving the payment is caused by the redeem coupon API timeout from the LPS Voucher voucher interface
2. No void redeem API call is recorded in the external party API call log
3. The check remains in the expected state and the operator is informed of the timeout

### 2.Void Redeem Triggered For Legitimate Removal

#### 2.1.Void redeem is still triggered when an existing payment needs to be removed

**Prerequisite(s):**
1. The build containing the HERO-67008 fix is deployed to the test POS environment
2. An LPS Voucher voucher interface is configured and attached to a payment method
3. A check exists with a successfully redeemed LPS voucher payment already saved
4. The existing payment needs to be removed for a reason other than a previous redeem coupon API failure (for example operator voids the saved LPS voucher payment)

**Step(s):**
1. Open the check that already contains a successfully redeemed LPS voucher payment
2. Trigger removal of the saved LPS voucher payment through the standard void payment workflow
3. Check the POS logs and the external party API call log for the void redeem API call

**Reproduction Result(s):**
1. The behaviour of void redeem being triggered for legitimate payment removal is the existing correct behaviour and is not affected by the bug

**Fix Result(s):**
1. The system still triggers the external party void redeem API call when the involved payment needs to be removed for a legitimate reason that is not a previous redeem coupon API failure
2. The void redeem API call is recorded in the external party API call log as expected
3. The fix does not regress the legitimate void redeem workflow

### 3.Regression Scope

The following related modules and scenarios must be retested after the fix:

- LPS Voucher voucher interface redeem coupon API success path
- LPS Voucher voucher interface redeem coupon API error and timeout paths
- Void payment workflow for LPS voucher payments that were successfully redeemed
- POS check settlement with LPS voucher as the only payment or as part of a multi-tender payment
- External party API call log for redeem and void redeem calls
