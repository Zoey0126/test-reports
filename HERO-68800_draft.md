# General V3 membership interface - Incorrect request ID for prepare refund coupon if there are multiple check discount coupon with the same discount type Test Report

## Functional Testing

### 1.Coupon Redemption Via SVC Coupon Enquiry

#### 1.1.Apply Single Check Discount Coupon
**Prerequisite(s):**
1. General V3 membership interface is configured and running normally
2. A General V3 member is attached to the check
3. At least one valid coupon with check discount type is available for the member
4. External API monitoring or log tool is available to capture request payloads

**Step(s):**
1. Open a check attached with General V3 member
2. Click function "SVC Coupon Enquiry"
3. Input the coupon number for enquiry
4. Click button "Apply Coupon"
5. Observe the external API "Prepare Redeem Coupon" request and response

**Reproduction Result(s):**
1. Baseline scenario - with a single coupon, no incorrect request ID behavior was observed before the fix

**Fix Result(s):**
1. System triggers external API "Prepare Redeem Coupon" successfully and the coupon discount with type as check discount is applied to the check
2. The requestCompletionId from the "Redeem Coupon" response is stored and bound to this coupon
3. The subsequent refund of this coupon uses the matching requestCompletionId

#### 1.2.Apply Multiple Check Discount Coupons With The Same Discount Type
**Prerequisite(s):**
1. General V3 membership interface is configured and running normally
2. A General V3 member is attached to the check
3. At least two valid coupons belonging to the same check discount type are available for the member

**Step(s):**
1. Open a check attached with General V3 member
2. Click function "SVC Coupon Enquiry"
3. Input the first coupon number and click button "Apply Coupon"
4. Select another target coupon
5. Click button "Apply Coupon" again
6. Return to ordering panel and check the applied discounts

**Reproduction Result(s):**
1. With the previous version, both coupons were applied successfully, but only the requestCompletionId of the last "Redeem Coupon" response was kept for the refund step, which led to the defect

**Fix Result(s):**
1. Each "Apply Coupon" operation triggers external API "Prepare Redeem Coupon" and its response requestCompletionId is stored separately per coupon
2. Both coupon discounts with check discount type are applied to the check correctly

### 2.Coupon Refund Request ID Validation

#### 2.1.Void Check Discount After Applying Multiple Coupons
**Prerequisite(s):**
1. A check with at least two applied check discount coupons of the same discount type
2. External API monitoring or log tool is available to capture "Prepare Refund Coupon" requests

**Step(s):**
1. Return to ordering panel
2. Click function "Void Check Discount" to void the discount coupons
3. Check the request payload of each external API "Prepare Refund Coupon" call

**Reproduction Result(s):**
1. With the previous version, "Prepare Refund Coupon" was posted with the requestCompletionId from the last coupon "Redeem Coupon" response, so the refund was made with an incorrect request ID

**Fix Result(s):**
1. System triggers external API "Prepare Refund Coupon" and each refund request carries the requestCompletionId from its corresponding "Redeem Coupon" response
2. The requestCompletionId from the last "Redeem Coupon" response is not reused for all refund requests
3. The refund is processed successfully on the membership side

#### 2.2.Void Multiple Item Discount After Applying Multiple Coupons
**Prerequisite(s):**
1. A check with at least two applied check discount coupons of the same discount type
2. External API monitoring or log tool is available to capture "Prepare Refund Coupon" requests

**Step(s):**
1. Return to ordering panel
2. Click function "Void Multiple Item Discount" to void the discount coupons
3. Check the request payload of each external API "Prepare Refund Coupon" call

**Reproduction Result(s):**
1. With the previous version, the refund requests were posted with the requestCompletionId from the last "Redeem Coupon" response instead of the corresponding one

**Fix Result(s):**
1. System triggers external API "Prepare Refund Coupon" with the requestCompletionId matching each corresponding "Redeem Coupon" response
2. All selected coupon discounts are voided correctly on the check

#### 2.3.Void Only One Coupon Among Multiple Applied Coupons
**Prerequisite(s):**
1. A check with at least two applied check discount coupons of the same discount type
2. External API monitoring or log tool is available

**Step(s):**
1. Return to ordering panel
2. Void only the first applied coupon discount
3. Check the request payload of external API "Prepare Refund Coupon"
4. Void the remaining coupon discount and check the request payload again

**Reproduction Result(s):**
1. With the previous version, the refund request of the first voided coupon still used the requestCompletionId from the last "Redeem Coupon" response

**Fix Result(s):**
1. The refund request of each voided coupon carries its own corresponding requestCompletionId
2. The remaining applied coupon discount stays effective on the check

### 3.Reapply Coupon After Refund
**Prerequisite(s):**
1. A check with two applied check discount coupons of the same discount type has been refunded with correct requestCompletionId values

**Step(s):**
1. Run "SVC Coupon Enquiry" again and apply the coupons to a new check
2. Return to ordering panel and void the discounts again
3. Check the request payload of each external API "Prepare Refund Coupon" call

**Reproduction Result(s):**
1. Negative verification scenario - the defect was reproduced when voiding multiple same-type coupons, not during the re-application flow itself

**Fix Result(s):**
1. The coupons can be re-applied after refund without error
2. New "Prepare Redeem Coupon" responses return new requestCompletionId values which are stored correctly
3. The subsequent refund requests use the newly stored requestCompletionId values

### 4.Regression Scope
1. General V3 SVC Coupon Enquiry - coupon enquiry and apply coupon flow
2. External API request payload of "Prepare Redeem Coupon" and "Prepare Refund Coupon"
3. Void Check Discount and Void Multiple Item Discount functions
4. Single check discount coupon apply and void flow
5. Coupon refund for coupons with different discount types (item discount coupons)
