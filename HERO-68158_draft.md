# TMS 2.0 - Add deposit status and support refund with third party payment - Dev (5/5) Test Report

## Functional Testing

### 1.Deposit Status Display Verification

#### 1.1.Verify Pending Deposit Status Display

**Prerequisite(s):**
1. User is logged into TMS 2.0 with valid credentials and deposit view permission.
2. A deposit transaction in Pending status exists in the system.
3. The Deposit Management module is accessible.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Locate the deposit transaction with Pending status.
3. Observe the status column for the selected transaction in the list view.
4. Open the transaction detail view to verify the status label.
5. Verify the status badge color and label text.

**Test Result(s):**
1. The deposit transaction displays "Pending" status correctly in the list view.
2. The status badge shows the configured color (e.g., yellow/orange) for Pending.
3. The status label is consistent between the list view and the detail view.
4. The Pending status indicates the deposit has been created but not yet confirmed by the payment gateway.

#### 1.2.Verify Confirmed Deposit Status Display

**Prerequisite(s):**
1. User is logged into TMS 2.0 with valid credentials and deposit view permission.
2. A deposit transaction in Confirmed status exists in the system.
3. The Deposit Management module is accessible.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Locate the deposit transaction with Confirmed status.
3. Observe the status column for the selected transaction in the list view.
4. Open the transaction detail view to verify the status label.
5. Verify the status badge color and label text.

**Test Result(s):**
1. The deposit transaction displays "Confirmed" status correctly in the list view.
2. The status badge shows the configured color (e.g., green) for Confirmed.
3. The status label is consistent between the list view and the detail view.
4. The Confirmed status indicates the deposit has been received and verified by the payment gateway.

#### 1.3.Verify Refunded Deposit Status Display

**Prerequisite(s):**
1. User is logged into TMS 2.0 with valid credentials and deposit view permission.
2. A deposit transaction in Refunded status exists in the system.
3. The Deposit Management module is accessible.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Locate the deposit transaction with Refunded status.
3. Observe the status column for the selected transaction in the list view.
4. Open the transaction detail view to verify the status label and refund information.
5. Verify the status badge color, label text, and refund details.

**Test Result(s):**
1. The deposit transaction displays "Refunded" status correctly in the list view.
2. The status badge shows the configured color (e.g., grey/blue) for Refunded.
3. The status label is consistent between the list view and the detail view.
4. The refund amount, refund time, and operator information are visible in the detail view.
5. The Refunded status indicates the deposit has been fully returned to the customer through the third party payment gateway.

### 2.Third Party Payment Refund Flow

#### 2.1.Initiate Refund through Third Party Payment Gateway

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A deposit transaction in Confirmed status paid via third party payment exists.
3. The third party payment gateway integration is active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of a Confirmed deposit transaction paid via third party payment.
3. Click the "Refund" button.
4. Enter the refund amount (full amount).
5. Select a refund reason from the dropdown list.
6. Confirm the refund operation.
7. Wait for the third party payment gateway response.

**Test Result(s):**
1. The refund request is successfully sent to the third party payment gateway.
2. The gateway returns a successful refund response within the expected timeframe.
3. The deposit status changes from "Confirmed" to "Refunded".
4. A refund confirmation message is displayed to the user.
5. A refund transaction record is created in the system with the gateway reference number.

#### 2.2.Verify Status Changes After Refund

**Prerequisite(s):**
1. User is logged into TMS 2.0 with deposit view permission.
2. A refund operation has been completed for a deposit transaction via third party payment.
3. The deposit transaction has a Refunded status.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Locate the refunded deposit transaction in the list.
3. Open the transaction detail view.
4. Verify the status field and status history log.
5. Check the refund timestamp and operator information.

**Test Result(s):**
1. The deposit status displays "Refunded" correctly in both list and detail views.
2. The status history log shows the transition from Confirmed to Refunded.
3. The refund timestamp and operator details are recorded in the history log.
4. The original payment and refund records are linked correctly.
5. The refund reference number from the third party gateway is displayed in the transaction detail.

#### 2.3.Verify Refund Amount Accuracy

**Prerequisite(s):**
1. User is logged into TMS 2.0 with deposit view permission.
2. A confirmed deposit transaction with a known amount exists.
3. A refund operation has been processed through the third party payment gateway.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the refunded transaction detail view.
3. Compare the original deposit amount with the refund amount.
4. Verify the remaining balance (if a partial refund was performed).
5. Check the third party gateway settlement report for the refund record.

**Test Result(s):**
1. The refund amount matches the requested refund value exactly.
2. The original deposit amount is correctly displayed in the transaction detail.
3. The remaining balance (if applicable) is calculated and displayed correctly.
4. The amount in the third party gateway settlement report matches the system record.
5. No discrepancy exists between the system record and the gateway record.

### 3.Refund with Multiple Payment Methods

#### 3.1.Refund with VGS Payment Method

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A confirmed deposit transaction paid via VGS payment method exists.
3. The VGS payment gateway integration is active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of the VGS-paid deposit transaction.
3. Click the "Refund" button.
4. Enter the refund amount (full amount).
5. Confirm the refund operation.
6. Wait for the VGS gateway response.

**Test Result(s):**
1. The refund request is successfully processed through the VGS payment gateway.
2. The VGS gateway returns a successful refund response.
3. The deposit status changes to "Refunded".
4. The VGS refund reference number is recorded in the transaction detail.
5. The refund amount matches the original VGS payment amount.

#### 3.2.Refund with Credit Card Payment Method

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A confirmed deposit transaction paid via credit card exists.
3. The credit card payment gateway integration is active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of the credit card-paid deposit transaction.
3. Click the "Refund" button.
4. Enter the refund amount (full amount).
5. Confirm the refund operation.
6. Wait for the credit card gateway response.

**Test Result(s):**
1. The refund request is successfully processed through the credit card payment gateway.
2. The credit card gateway returns a successful refund response.
3. The deposit status changes to "Refunded".
4. The credit card refund reference number is recorded in the transaction detail.
5. The refund amount matches the original credit card payment amount.
6. The refund transaction appears correctly in the customer's credit card statement.

#### 3.3.Refund with Mixed Payment Method

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A confirmed deposit transaction paid with mixed payment methods (e.g., VGS + credit card) exists.
3. Both payment gateway integrations are active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of the mixed-payment deposit transaction.
3. Click the "Refund" button.
4. Select refund allocation for each payment method.
5. Enter refund amount for each payment method.
6. Confirm the refund operation.
7. Wait for both gateway responses.

**Test Result(s):**
1. The refund is successfully processed through both payment gateways.
2. Each gateway returns a successful refund response.
3. The deposit status changes to "Refunded".
4. Each payment method's refund reference is recorded in the transaction detail.
5. The total refund amount matches the sum of the original payment amounts.
6. The refund allocation is correctly displayed in the transaction detail view.

### 4.Edge Cases

#### 4.1.Partial Refund Processing

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A confirmed deposit transaction with a known amount exists.
3. The third party payment gateway integration is active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of the confirmed deposit transaction.
3. Click the "Refund" button.
4. Enter a partial refund amount (less than the original deposit amount).
5. Confirm the refund operation.
6. Wait for the gateway response.
7. Verify the deposit status after partial refund.
8. Attempt a second refund for the remaining balance.

**Test Result(s):**
1. The partial refund is successfully processed through the third party gateway.
2. The deposit status displays "Partially Refunded" (or the appropriate intermediate status).
3. The remaining balance is correctly calculated and displayed.
4. The refund amount is recorded accurately in the transaction detail.
5. A subsequent refund for the remaining balance can be processed successfully.
6. After all partial refunds complete, the deposit status changes to "Refunded".

#### 4.2.Refund Cancellation

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A refund operation is in progress (not yet completed/sent to gateway).
3. The refund can be cancelled before gateway confirmation.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of a deposit transaction with refund in progress.
3. Click the "Cancel Refund" button.
4. Confirm the cancellation in the confirmation dialog.
5. Verify the deposit status after cancellation.
6. Check the transaction history log for the cancellation record.

**Test Result(s):**
1. The refund operation is successfully cancelled before gateway submission.
2. The deposit status reverts to "Confirmed" after cancellation.
3. No refund transaction is sent to the third party payment gateway.
4. The cancellation is logged in the status history with timestamp and operator.
5. The deposit amount remains unchanged.

#### 4.3.Concurrent Refund Attempts

**Prerequisite(s):**
1. Two users are logged into TMS 2.0 with refund permission.
2. A confirmed deposit transaction exists in the system.
3. The third party payment gateway integration is active and reachable.

**Step(s):**
1. User A and User B both open the same confirmed deposit transaction detail.
2. User A clicks "Refund" button and enters the refund amount.
3. User B clicks "Refund" button simultaneously and enters the refund amount.
4. Both users confirm the refund operation at the same time.
5. Observe the system response to the concurrent operations.
6. Verify the deposit status after both operations complete.

**Test Result(s):**
1. Only one refund operation is processed successfully by the system.
2. The other refund operation is rejected with an appropriate error message indicating the deposit is already being refunded.
3. The deposit status changes to "Refunded" only once.
4. No duplicate refund transactions are created in the third party payment gateway.
5. The system maintains data integrity and the correct refund amount.

#### 4.4.Refund After Void

**Prerequisite(s):**
1. User is logged into TMS 2.0 with refund permission.
2. A deposit transaction has been voided (Void status).
3. The third party payment gateway integration is active and reachable.

**Step(s):**
1. Navigate to TMS > Deposit Management page.
2. Open the detail of the voided deposit transaction.
3. Verify if the "Refund" button is available/enabled.
4. Attempt to initiate a refund operation on the voided transaction.
5. Observe the system response to the refund attempt.
6. Verify the deposit status remains unchanged.

**Test Result(s):**
1. The "Refund" button is disabled or hidden for voided transactions.
2. If a refund is attempted, the operation is rejected with an appropriate error message.
3. The deposit status remains "Voided" after the refund attempt.
4. No refund transaction is sent to the third party payment gateway.
5. The system prevents refund operations on voided transactions correctly.
