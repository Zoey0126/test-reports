# POS Operation - Able to define payment type restriction for advanced order Test Report

## Functional Testing

### 1.Configure Allowed Payment Methods in Advance Order Setting

**Prerequisite(s):**
1. User has access to the Infrasys POS platform with configuration rights
2. At least two payment methods exist in the system (e.g. Cash and Credit Card)

**Step(s):**
1. Go to Infrasys POS platform > POS System > Ordering Setup > Config by Location
2. Select setup "Advance Order Setting" and click "Add New"
3. Select Apply To = Outlet, then select the target shop and outlet
4. Under section "Payment Restriction", set Allowed Payment Methods to Cash and Credit Card
5. Click "Save" to save the record

**Test Result(s):**
1. The Advance Order Setting record is saved successfully with the allowed payment methods
2. The restriction scope is applied to the selected outlet

### 2.Advanced Order Payment with Restriction Enabled

#### 2.1.Settle advance order with an allowed payment method

**Prerequisite(s):**
1. Allowed Payment Methods is configured as Cash and Credit Card for the target outlet
2. A deposit item is configured for advance order
3. A new check can be opened on the POS

**Step(s):**
1. Open a new check and click function "Create Advance Order"
2. Fill in the required information for creating the advance order and click "Confirm"
3. Order an item, then order the deposit item of the advance order
4. Click function "Paid" to proceed to the cashier
5. Select Credit Card to settle the check and finish the payment

**Test Result(s):**
1. The Credit Card payment is accepted because it is included in Allowed Payment Methods
2. The payment is added to the payment basket and the check is settled successfully

#### 2.2.Settle advance order with a disallowed payment method

**Prerequisite(s):**
1. Allowed Payment Methods is configured as Cash and Credit Card for the target outlet
2. Another payment method (e.g. Alipay) exists but is not included in Allowed Payment Methods
3. A deposit item is configured for advance order

**Step(s):**
1. Open a new check and click function "Create Advance Order"
2. Fill in the required information and click "Confirm"
3. Order an item, then order the deposit item of the advance order
4. Click function "Paid" to proceed to the cashier
5. Select Alipay to settle the check

**Test Result(s):**
1. System prompts error message "Selected payment method not allow for advanced order"
2. No payment is added to the payment basket and the workflow ends at this step

#### 2.3.Retry settlement with an allowed payment method after a rejected one

**Prerequisite(s):**
1. Allowed Payment Methods is configured as Cash and Credit Card for the target outlet
2. An advance order check with items and deposit item is ready at the cashier screen

**Step(s):**
1. Select a disallowed payment method at the cashier screen
2. After the error is prompted, select Credit Card to settle the check
3. Finish the payment workflow

**Test Result(s):**
1. The disallowed payment method is rejected with error message "Selected payment method not allow for advanced order"
2. The allowed payment method is accepted and the check is settled successfully

### 3.Empty or Missing Restriction Configuration

#### 3.1.Settle advance order when Allowed Payment Methods is empty

**Prerequisite(s):**
1. An Advance Order Setting record exists for the target outlet but Allowed Payment Methods is left empty
2. A deposit item is configured for advance order

**Step(s):**
1. Open a new check, click function "Create Advance Order" and confirm the advance order
2. Order an item and the deposit item of the advance order
3. Click function "Paid" and select any payment method
4. Finish the payment workflow

**Test Result(s):**
1. No payment restriction is applied because Allowed Payment Methods is empty
2. The selected payment method is accepted and the check is settled successfully

#### 3.2.Settle advance order when Advance Order Setting does not exist

**Prerequisite(s):**
1. No Advance Order Setting record is configured for the target outlet
2. A deposit item is configured for advance order

**Step(s):**
1. Open a new check, click function "Create Advance Order" and confirm the advance order
2. Order an item and the deposit item of the advance order
3. Click function "Paid" and select any payment method
4. Finish the payment workflow

**Test Result(s):**
1. No payment restriction is applied because the setting does not exist
2. The selected payment method is accepted and the check is settled successfully

### 4.Restriction Scope and Non-Advanced Orders

#### 4.1.Normal check settlement is not affected by the restriction

**Prerequisite(s):**
1. Allowed Payment Methods is configured as Cash and Credit Card for the target outlet
2. A normal check (without Create Advance Order) is open with at least one ordered item

**Step(s):**
1. Click function "Paid" on the normal check
2. Select a payment method that is not included in Allowed Payment Methods
3. Finish the payment workflow

**Test Result(s):**
1. The payment restriction is not applied to the normal check
2. The selected payment method is accepted and the check is settled successfully

#### 4.2.Restriction applies only to the configured location scope

**Prerequisite(s):**
1. Allowed Payment Methods is configured for outlet A only (Apply To = Outlet)
2. Outlet B belongs to the same shop but has no Advance Order Setting
3. A deposit item is configured for advance order

**Step(s):**
1. On a POS station in outlet A, create an advance order and try to settle it with a disallowed payment method
2. On a POS station in outlet B, create an advance order and try to settle it with the same disallowed payment method

**Test Result(s):**
1. In outlet A the disallowed payment method is rejected with error message "Selected payment method not allow for advanced order"
2. In outlet B the same payment method is accepted because no restriction is configured for that outlet
