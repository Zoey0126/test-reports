# HERO-64777 Test Report - General v3 Membership Interface Triggers Redeem Prepare Cancel API When Exit Cashier with Partial Payments

## Background

For checks attached with a General V3 membership interface member, when the cashier applies partial payment(s) with a redeem point payment method, clicks "Finish" and then clicks "Exit" to leave the cashier screen, the system wrongly triggers the external party API of point redeem prepare cancel for the successfully redeemed partial payment(s). When two partial redeem payments are applied (e.g., Redeem Multiple Point (AUD) and Redeem Multiple Point (Tips)), the redeem prepare cancel API is triggered twice, as shown in membership_packet.log.

Expected behaviour: the system does not trigger the external party API of point redeem prepare cancel for successfully redeemed points in partial payment after finishing payment and exiting the cashier.

## Functional Testing

### 1. Bug Reproduction and Fix Verification - Redeem Prepare Cancel on Exit Cashier After Partial Payment

#### 1.1. Verify Partial Payment with Payment Type "By redeem point" Does Not Trigger Redeem Prepare Cancel on Exit Cashier

**Prerequisite(s):**
1. Setup "Support Partial Payment" under POS System > Ordering Setup > Config by Location exists with Support Partial Payment = Yes and Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No
2. A payment method under POS System > Interface Configurations > Payment Settings is attached with the target General V3 membership interface and Payment Type = "By redeem point"
3. A printed check attached with a General V3 member exists
4. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the payment method with Payment Type = "By redeem point"
4. Input the redeem point amount following the existing workflow
5. Click button "Finish" to finish the partial payment
6. Check membership_packet.log for the external party API calls triggered at this point
7. Click button "Exit" to exit the cashier
8. Check membership_packet.log again for the external party API calls triggered after exit

**Reproduction Result(s):**
1. Before the fix, after clicking "Finish" the system triggers the external party API of point redeem
2. Before the fix, after clicking "Exit" the system wrongly triggers the external party API of point redeem prepare cancel for the successfully redeemed partial payment

**Fix Result(s):**
1. After clicking "Finish", the system triggers the external party API of point redeem as per the existing workflow
2. After clicking "Exit", the system does not trigger the external party API of point redeem prepare cancel for the successfully redeemed point in the partial payment
3. The partial payment record remains applied on the check and membership_packet.log shows no redeem prepare cancel request

#### 1.2. Verify Partial Payment with Payment Type "Point amount mapping" Does Not Trigger Redeem Prepare Cancel on Exit Cashier

**Prerequisite(s):**
1. Setup "Support Partial Payment" under POS System > Ordering Setup > Config by Location exists with Support Partial Payment = Yes and Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No
2. A payment method under POS System > Interface Configurations > Payment Settings is attached with the target General V3 membership interface and Payment Type = "Point amount mapping"
3. A printed check attached with a General V3 member exists
4. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the payment method with Payment Type = "Point amount mapping"
4. Input the redeem point amount following the existing workflow
5. Click button "Finish" to finish the partial payment
6. Check membership_packet.log for the external party API calls triggered at this point
7. Click button "Exit" to exit the cashier
8. Check membership_packet.log again for the external party API calls triggered after exit

**Reproduction Result(s):**
1. Before the fix, after clicking "Finish" the system triggers the external party API of point redeem
2. Before the fix, after clicking "Exit" the system wrongly triggers the external party API of point redeem prepare cancel for the successfully redeemed partial payment

**Fix Result(s):**
1. After clicking "Finish", the system triggers the external party API of point redeem as per the existing workflow
2. After clicking "Exit", the system does not trigger the external party API of point redeem prepare cancel for the successfully redeemed point in the partial payment
3. The partial payment record remains applied on the check and membership_packet.log shows no redeem prepare cancel request

#### 1.3. Verify Partial Payment with Payment Type "By multiple redeem point" Does Not Trigger Redeem Prepare Cancel on Exit Cashier

**Prerequisite(s):**
1. Setup "Support Partial Payment" under POS System > Ordering Setup > Config by Location exists with Support Partial Payment = Yes and Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No
2. A payment method under POS System > Interface Configurations > Payment Settings is attached with the target General V3 membership interface and Payment Type = "By multiple redeem point" (e.g., Redeem Multiple Point (AUD))
3. A printed check attached with a General V3 member exists
4. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the payment method with Payment Type = "By multiple redeem point"
4. Input the redeem point amount following the existing workflow
5. Click button "Finish" to finish the partial payment
6. Check membership_packet.log for the external party API calls triggered at this point
7. Click button "Exit" to exit the cashier
8. Check membership_packet.log again for the external party API calls triggered after exit

**Reproduction Result(s):**
1. Before the fix, after clicking "Finish" the system triggers the external party API of point redeem
2. Before the fix, after clicking "Exit" the system wrongly triggers the external party API of point redeem prepare cancel for the successfully redeemed partial payment

**Fix Result(s):**
1. After clicking "Finish", the system triggers the external party API of point redeem as per the existing workflow
2. After clicking "Exit", the system does not trigger the external party API of point redeem prepare cancel for the successfully redeemed point in the partial payment
3. The partial payment record remains applied on the check and membership_packet.log shows no redeem prepare cancel request

#### 1.4. Verify Multiple Partial Redeem Payments in One Cashier Session Do Not Trigger Redeem Prepare Cancel on Exit Cashier

**Prerequisite(s):**
1. Setup "Support Partial Payment" under POS System > Ordering Setup > Config by Location exists with Support Partial Payment = Yes and Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No
2. Two payment methods under POS System > Interface Configurations > Payment Settings are attached with the target General V3 membership interface and Payment Type = "By multiple redeem point", distinguished by "Point Balance Code For Multiple Point Redemption" (e.g., P1 for Redeem Multiple Point (AUD), P2 for Redeem Multiple Point (Tips))
3. A printed check attached with a General V3 member exists
4. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the payment method Redeem Multiple Point (AUD), input the value and click button "Enter"
4. Input the tips if necessary and click button "Enter"
5. Select the payment method Redeem Multiple Point (Tips), input the value and click button "Enter"
6. Click button "Finish" to finish the partial payments
7. Check membership_packet.log for the external party API calls triggered at this point
8. Click button "Exit" to exit the cashier
9. Check membership_packet.log again for the external party API calls triggered after exit

**Reproduction Result(s):**
1. Before the fix, after clicking "Finish" the system triggers the external party API of point redeem for the applied partial payments
2. Before the fix, after clicking "Exit" the system wrongly triggers the external party API of point redeem prepare cancel twice, once for each successfully redeemed partial payment

**Fix Result(s):**
1. After clicking "Finish", the system triggers the external party API of point redeem for the applied partial payments as per the existing workflow
2. After clicking "Exit", the system does not trigger the external party API of point redeem prepare cancel for any of the successfully redeemed partial payments
3. Both partial payment records remain applied on the check and membership_packet.log shows no redeem prepare cancel request

### 2. Regression - General V3 Redeem Point Payment and Cashier Exit Flows

#### 2.1. Verify Exit Cashier Without Finishing the Redeem Payment Still Triggers Redeem Prepare Cancel

**Prerequisite(s):**
1. Setup "Support Partial Payment" under POS System > Ordering Setup > Config by Location exists with Support Partial Payment = Yes and Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No
2. A payment method under POS System > Interface Configurations > Payment Settings is attached with the target General V3 membership interface and Payment Type = "By redeem point" or "Point amount mapping" or "By multiple redeem point"
3. A printed check attached with a General V3 member exists
4. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the redeem point payment method and input the redeem point amount following the existing workflow, so that the point redeem prepare is initiated
4. Without clicking button "Finish", click button "Exit" to exit the cashier
5. Check membership_packet.log for the external party API calls triggered after exit

**Reproduction Result(s):**
1. Before the fix, exiting the cashier without finishing the payment triggers the external party API of point redeem prepare cancel to roll back the uncommitted redeem prepare (existing behaviour)

**Fix Result(s):**
1. After the fix, exiting the cashier without finishing the payment still triggers the external party API of point redeem prepare cancel to roll back the uncommitted redeem prepare
2. The legitimate redeem prepare cancel behaviour for unfinished payments is not affected by the fix

#### 2.2. Verify Full Settlement with Redeem Point Payment Is Not Affected

**Prerequisite(s):**
1. A payment method under POS System > Interface Configurations > Payment Settings is attached with the target General V3 membership interface and Payment Type = "By redeem point" or "Point amount mapping" or "By multiple redeem point"
2. A printed check attached with a General V3 member exists, with a balance that can be fully settled by the redeem point payment
3. Tester has access to membership_packet.log to check the external party API calls

**Step(s):**
1. Open the printed check attached with the General V3 member
2. Click function "Paid" to go to cashier
3. Select the redeem point payment method and input the redeem point amount to fully settle the check
4. Click button "Finish" to complete the payment
5. Check the check status and membership_packet.log

**Reproduction Result(s):**
1. Before the fix, full settlement with the redeem point payment completes the check normally (the bug only occurs for partial payments followed by exit cashier)

**Fix Result(s):**
1. The check is fully settled and closed as per the existing workflow
2. The external party API of point redeem is triggered for the redeemed amount and no point redeem prepare cancel is triggered
3. The full settlement flow with the General V3 membership interface behaves exactly as before the fix

### 3. Regression Scope

The following related modules and functions must be retested after the fix:

- Cashier partial payment flow with General V3 membership interface redeem point payments (Payment Type = "By redeem point" / "Point amount mapping" / "By multiple redeem point"), including the "Finish" and "Exit" button actions
- External party API calls of the General V3 membership interface: point redeem prepare, point redeem, and point redeem prepare cancel (verifiable in membership_packet.log)
- General V3 membership interface variants based on the same general_v3 framework (e.g., HMC, DailyPoint, AWE, Eber, CIRSA, MCC, HardRock, COMO)
- Normal full settlement with redeem point payment methods attached with the General V3 membership interface
- Partial payment flow with non-membership payment methods (to confirm the fix does not change general partial payment behaviour)
- Redeem prepare cancel rollback behaviour when exiting the cashier without finishing the payment

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-64777 |
| Summary | General v3 membership interface - Trigger redeem prepare cancel API to external if exit cashier with applying partial payments |
| Sprint | POS - 20261019-20261030 |
| Template | infrasys |
| Issue Type | Bug |
| QC Tester | Phoebe Chen (phoebe.chen@shijigroup.com) |
| QC Reviewer | Briana Li (briana.li@shijigroup.com) |
| Test Target | POS cashier partial payment with General V3 membership interface redeem point payment |
| Partial Payment Configuration | POS System > Ordering Setup > Config by Location > "Support Partial Payment": Support Partial Payment = Yes; Continue To Pay After Partial Settling Check At Cashier (After Clicking "Finish" Button) = No |
| Redeem Point Payment Configuration | POS System > Interface Configurations > Payment Settings: Interface = target General V3 membership interface; Payment Type = "By redeem point" / "Point amount mapping" / "By multiple redeem point" |
| Verification Points | membership_packet.log for external party API calls of point redeem and point redeem prepare cancel; partial payment records on the check |
