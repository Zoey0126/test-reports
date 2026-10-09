# HERO-68821 Bug Fix Test Report - General V3 Membership Interface - System Refunds Last Coupon Only When Voiding Multiple Coupon Item Discounts

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Multiple Coupon Item Discount Void and Refund

#### F1.1.Verify All Coupons Refunded When Voiding Multiple Coupon Item Discounts

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. A valid membership account with multiple usable coupons is set up in the external membership system
3. Coupon item discount is configured and can be applied to items in POS
4. An external API "Prepare Refund Coupon" endpoint is available and monitored for requests
5. A new check is open

**Step(s):**
1. Open a new check
2. Order multiple items (e.g., Item A, Item B, Item C)
3. Apply General V3 coupon item discount to each item using different coupons (Coupon 1 on Item A, Coupon 2 on Item B, Coupon 3 on Item C)
4. Verify all three coupon item discounts are applied successfully
5. Click function "Void Multiple Item Discount" to void all the discount coupons at once
6. Monitor the external API "Prepare Refund Coupon" requests triggered by the system
7. Verify the number of refund API calls made and the coupon details in each request

**Reproduction Result(s):**
1. After clicking "Void Multiple Item Discount", the system triggers the external API "Prepare Refund Coupon" only once
2. The single API request contains the requestCompletionId from the last coupon (Coupon 3) only
3. Coupon 1 and Coupon 2 are not refunded to the external membership system
4. Only the last coupon is refunded while the other coupons remain consumed

**Fix Result(s):**
1. After clicking "Void Multiple Item Discount", the system triggers the external API "Prepare Refund Coupon" multiple times, once for each coupon
2. Each API request contains the correct requestCompletionId for the respective coupon (Coupon 1, Coupon 2, Coupon 3)
3. All three coupons are refunded to the external membership system
4. All coupon item discounts are voided and all coupons are returned

#### F1.2.Verify Error Handling When One Coupon Refund Fails During Multiple Void

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. A valid membership account with multiple usable coupons is set up
3. At least one coupon is configured to trigger a refund error (e.g., simulate API failure for one coupon)
4. A new check is open with multiple coupon item discounts applied (Coupon 1 on Item A, Coupon 2 on Item B)

**Step(s):**
1. Open a new check with multiple coupon item discounts already applied
2. Configure or simulate a refund error for Coupon 1 (e.g., network error or API rejection)
3. Click function "Void Multiple Item Discount" to void all the discount coupons
4. Observe the refund process for each coupon
5. Verify the error handling behavior: Coupon 1 refund fails, Coupon 2 refund should still proceed
6. Verify the item discount void status for each item

**Reproduction Result(s):**
1. The system triggers the refund API only once with the last coupon, so error handling for individual coupons is not exercised
2. If the single API call fails, all coupon refunds fail and no item discounts are voided correctly

**Fix Result(s):**
1. The system processes each coupon refund individually
2. If an error occurs during Coupon 1 refund, the error is handled and the remaining coupons (Coupon 2) are not refunded
3. Item discount is voided only for coupons with successful refunds
4. The item with the failed coupon refund (Item A) retains its item discount since the refund was not successful
5. The user is notified of the refund failure for the affected coupon

#### F1.3.Verify Item Discount Voided Only for Successful Refund Coupons

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. A valid membership account with multiple usable coupons is set up
3. A new check is open with multiple coupon item discounts applied (Coupon 1 on Item A, Coupon 2 on Item B, Coupon 3 on Item C)
4. One coupon (Coupon 2) is configured to trigger a refund error

**Step(s):**
1. Open the check with three coupon item discounts applied
2. Simulate a refund error for Coupon 2
3. Click function "Void Multiple Item Discount" to void all discount coupons
4. Verify Coupon 1 refund succeeds - Item A discount should be voided
5. Verify Coupon 2 refund fails - Item B discount should NOT be voided
6. Verify Coupon 3 refund succeeds - Item C discount should be voided
7. Check the final check state for each item's discount status

**Reproduction Result(s):**
1. The system does not process refunds individually, so partial void is not possible
2. All item discounts are either voided or not voided together since only one API call is made

**Fix Result(s):**
1. Coupon 1 refund succeeds and Item A item discount is voided
2. Coupon 2 refund fails and Item B item discount is NOT voided (retains the coupon discount)
3. Coupon 3 refund succeeds and Item C item discount is voided
4. Only items with successful coupon refunds have their item discounts voided
5. The check correctly reflects partial void status based on refund success

#### F1.4.Verify requestCompletionId Correctness in Each Refund API Request

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. A valid membership account with multiple usable coupons is set up
3. API request logging or monitoring is enabled to capture all "Prepare Refund Coupon" requests
4. A new check is open

**Step(s):**
1. Open a new check and order three items
2. Apply Coupon 1 to Item A, Coupon 2 to Item B, Coupon 3 to Item C
3. Record the requestCompletionId for each coupon at apply time
4. Click function "Void Multiple Item Discount" to void all coupons
5. Capture all "Prepare Refund Coupon" API requests
6. Verify the requestCompletionId in each refund request matches the corresponding coupon

**Reproduction Result(s):**
1. Only one "Prepare Refund Coupon" API request is captured
2. The requestCompletionId in the single request matches only the last coupon (Coupon 3)
3. The requestCompletionId values for Coupon 1 and Coupon 2 are not present in any refund request

**Fix Result(s):**
1. Three separate "Prepare Refund Coupon" API requests are captured (one per coupon)
2. The requestCompletionId in the first request matches Coupon 1
3. The requestCompletionId in the second request matches Coupon 2
4. The requestCompletionId in the third request matches Coupon 3
5. Each coupon is refunded with its correct requestCompletionId

### F2.Regression Testing - Membership Coupon and Related Reports

#### F2.1.Verify Single Coupon Item Discount Void and Refund (No Regression)

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. A valid membership account with at least one usable coupon is set up
3. A new check is open

**Step(s):**
1. Open a new check and order an item
2. Apply a single General V3 coupon item discount to the item
3. Verify the coupon item discount is applied successfully
4. Click function "Void Item Discount" to void the single coupon discount
5. Monitor the "Prepare Refund Coupon" API request
6. Verify the coupon is refunded correctly

**Reproduction Result(s):**
1. Not applicable - this is a regression test to ensure single coupon void still works after the fix
2. (If regression occurs) The single coupon void and refund does not work correctly

**Fix Result(s):**
1. The single coupon item discount is voided successfully
2. One "Prepare Refund Coupon" API request is triggered with the correct requestCompletionId
3. The coupon is refunded to the external membership system
4. The item discount is removed from the check

#### F2.2.Verify Item Discount Report (HKJC) - CUS0194 Reflects Voided Coupon Discounts

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. Multiple coupon item discounts have been applied and then voided on the current business day
3. Item Discount Report (HKJC) - CUS0194 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Item Discount Report (HKJC) - CUS0194
3. Select Date Type = Current Business Day
4. Select the target shop and outlet where the voided coupon checks exist
5. Generate the report in Detail view
6. Verify the voided coupon item discounts are reflected correctly
7. Verify the discount amounts show the voided status

**Reproduction Result(s):**
1. (If regression occurs) The report does not correctly reflect voided coupon discounts
2. (If regression occurs) Voided discount amounts are inconsistent with the actual check state

**Fix Result(s):**
1. The Item Discount Report (CUS0194) correctly reflects the voided coupon item discounts
2. The discount amounts show the correct voided status for each item
3. The report data is consistent with the actual check calculation after void

#### F2.3.Verify LPS Item Coupon Report - CUS0300 Reflects Voided Coupon Data

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. Multiple coupon item discounts have been applied and then voided on the current business day
3. LPS Item Coupon Report - CUS0300 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open LPS Item Coupon Report - CUS0300
3. Select Date Type = Current Business Day
4. Select the target shop and outlet where the voided coupon checks exist
5. Generate the report
6. Verify the LPS Coupon Name, LPS Coupon #, and LPS Account ID columns
7. Verify that voided coupons are not shown in the item data (per report spec: if coupon is void, item data is not shown)

**Reproduction Result(s):**
1. (If regression occurs) Voided coupon data still appears in the report when it should be excluded
2. (If regression occurs) The LPS Coupon fields show inconsistent data after void

**Fix Result(s):**
1. The LPS Item Coupon Report (CUS0300) correctly excludes voided coupon item data
2. LPS Coupon Name, LPS Coupon #, and LPS Account ID are not shown for voided coupons
3. The report data is consistent with the void status of the coupons

#### F2.4.Verify Detail Item Report (Discount Name) - CUS0253 Reflects Correct Discount After Void

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. Coupon item discounts have been applied and voided on the current business day
3. Detail Item Report (Discount Name) - CUS0253 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Detail Item Report (Discount Name) - CUS0253
3. Select Date Type = Current Business Day
4. Select the target shop and outlet
5. Generate the report in Detail view
6. Verify the Discount Name column reflects voided discounts correctly
7. Verify the Disc. column shows correct discount amounts after void

**Reproduction Result(s):**
1. (If regression occurs) The Discount Name column shows inconsistent data after void
2. (If regression occurs) The Disc. amount does not reflect the voided status

**Fix Result(s):**
1. The Detail Item Report (CUS0253) Discount Name column correctly reflects the void status
2. The Disc. column shows correct discount amounts (zero or voided) after coupon void
3. The report data is consistent with the actual check state

#### F2.5.Verify Discount Item Summary Hist Report - CUS0132 Reflects Correct Discount Totals After Void

**Prerequisite(s):**
1. General V3 membership interface is configured and available
2. Coupon item discounts have been applied and voided on the current business day
3. Discount Item Summary Hist Report - CUS0132 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Discount Item Summary Hist Report - CUS0132
3. Select Date Type = Current Business Day
4. Select the target shop and outlet
5. Generate the report
6. Verify the discount type summary reflects voided coupon discounts
7. Verify the DISC AMT and Total columns are correct after void

**Reproduction Result(s):**
1. (If regression occurs) The discount summary totals are incorrect after void
2. (If regression occurs) The DISC AMT does not reflect the voided coupon discounts

**Fix Result(s):**
1. The Discount Item Summary Hist Report (CUS0132) correctly reflects voided coupon discounts
2. The DISC AMT and Total columns show correct values after void
3. The report data is consistent with the actual check calculations

## Appendix

### Bug Description (HERO-68821)

**Component:** POS

**Current Behavior:**
It is available for General V3 membership interface. When voiding multiple coupon item discounts, the system only triggers the refund API to the external party with the last coupon details, instead of triggering multiple APIs for each coupon found.

**Workflow:**
1. Follow existing workflow to apply General V3 coupon item discount to multiple items
2. Click function "Void Multiple Item Discount" to void all the discount coupons
3. System triggers external API "Prepare Refund Coupon" only once, with the requestCompletionId from the last coupon

**Expected Result:**
- If there are multiple coupons, system should refund those coupons instead of just refund the last coupon
- If error occurs during any one of the coupon refund, the remaining coupons will not be refunded. Item discount is voided for those successful refund coupons

### Regression Test Scope - Related Modules and Functions

| Module / Function | Related Report | Regression Focus |
| --- | --- | --- |
| General V3 Membership Interface (Coupon Refund API) | - | Verify all coupons are refunded individually with correct requestCompletionId |
| Void Multiple Item Discount Function | - | Verify void processes each coupon and handles partial failures |
| Single Coupon Item Discount Void | - | Verify single coupon void and refund still works correctly |
| Coupon Error Handling | - | Verify failed refund does not void item discount; successful refunds void item discount |
| Item Discount Report (HKJC) | CUS0194 | Verify voided coupon discounts are reflected correctly in report |
| LPS Item Coupon Report | CUS0300 | Verify voided coupon item data is excluded from report |
| Detail Item Report (Discount Name) | CUS0253 | Verify discount name and amounts reflect void status |
| Discount Item Summary Hist Report | CUS0132 | Verify discount summary totals are correct after void |
| GPC Membership Discount Summary | POS0189 | Verify membership discount summary reflects correct data after void |
