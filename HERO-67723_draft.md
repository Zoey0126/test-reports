# POS Interface - General V3 - Add payment details in /vouchers/redeem/prepare Test Report

## Functional Testing

### 1.Interface Payment Setting Configuration

#### 1.1.Configure the new payment setting with default value Yes
**Prerequisite(s):**
1. Infrasys POS platform is accessible with an account that has configuration permission
2. A General V3 membership interface (vendor key general_v3) is already configured

**Step(s):**
1. Go to POS System > Interface Configurations > Payment Settings
2. Click Add New and select Apply To (All Locations, Shop, Outlet or Station) and the corresponding target scope
3. Select the target General V3 membership interface and set Payment Type = By input voucher number
4. Check the default value of the setting Add Unsaved Payments Into Transaction Payload For Prepare Redeem Voucher before changing it
5. Set the setting to Yes and click Save

**Test Result(s):**
1. The payment setting record is created and saved successfully
2. The setting Add Unsaved Payments Into Transaction Payload For Prepare Redeem Voucher defaults to Yes when it is not explicitly configured
3. The record with value Yes is saved and listed in the Payment Settings page

#### 1.2.Configure the setting with different Apply To scopes
**Prerequisite(s):**
1. Configuration permission is available
2. At least one shop, one outlet and one station exist in the environment

**Step(s):**
1. Create a payment setting with Apply To = Shop and select the target shop
2. Create a payment setting with Apply To = Outlet and select the target outlet
3. Create a payment setting with Apply To = Station and select the target station
4. Perform a voucher redemption at a location inside and outside each configured scope

**Test Result(s):**
1. Each record is saved successfully with the selected scope
2. The setting takes effect only within the configured shop, outlet or station
3. Locations outside the configured scope follow the existing workflow

### 2.Prepare Redeem Voucher Payload Validation

#### 2.1.Verify payment details are added to the payload when the setting is Yes
**Prerequisite(s):**
1. The setting Add Unsaved Payments Into Transaction Payload For Prepare Redeem Voucher is set to Yes for the target payment method
2. The Payment Type of the target payment method is By input voucher number or By input voucher number and member number
3. An open printed check with a General V3 member attached and a valid voucher are available
4. Interface packet log capture is enabled

**Step(s):**
1. Open the printed check and click Paid to go to the cashier
2. Select the target payment method attached with the General V3 membership interface
3. Enter the voucher number and continue the redemption
4. Check the outgoing request to /v3/{companyName}/vouchers/redeem/prepare in the interface log

**Test Result(s):**
1. The external API request Prepare Redeem Voucher is triggered
2. The transaction payload checkItems contains a payment item of the selected payment method with itemType = Payment
3. The payment item includes itemType, itemSubType, itemCode, itemName, itemDescription, itemCategory, itemSerialNumber, itemInventoryCode, itemDepartmentCode, itemChargeCode, itemChargeCodeGroup, itemizerId and itemTotal
4. itemTotal carries the amount details of the selected payment method so the external system can validate the voucher-to-payment-method mapping

#### 2.2.Verify payment details are not added when the setting is No or does not exist
**Prerequisite(s):**
1. The setting Add Unsaved Payments Into Transaction Payload For Prepare Redeem Voucher is set to No for one payment method and does not exist for another payment method, both with a valid Payment Type
2. An open check with a General V3 member attached and a valid voucher are available
3. Interface packet log capture is enabled

**Step(s):**
1. Redeem a voucher using the payment method with the setting set to No and check the outgoing Prepare Redeem Voucher payload
2. Redeem a voucher using the payment method without the setting configured and check the outgoing payload again

**Test Result(s):**
1. In both scenarios the system follows the existing workflow to prepare the transaction payload
2. The payload does not contain the payment details of the selected payment method
3. The redemption flow continues without error

### 3.Voucher Redemption Flow

#### 3.1.Redeem voucher with Payment Type By input voucher number
**Prerequisite(s):**
1. The target payment method is configured with Payment Type = By input voucher number
2. A valid voucher and an open check with a General V3 member attached are available

**Step(s):**
1. Open the check, click Paid and select the target payment method
2. Input the voucher number when prompted
3. Complete the redemption and settle the check

**Test Result(s):**
1. The system triggers the Prepare Redeem Voucher request with the voucher serial number
2. The voucher is redeemed successfully and the check is settled correctly
3. The payment details of the selected payment method are included in the payload when the new setting is Yes

#### 3.2.Redeem voucher with Payment Type By input voucher number and member number
**Prerequisite(s):**
1. The target payment method is configured with Payment Type = By input voucher number and member number
2. A valid voucher, its matching member and an open check with the member attached are available

**Step(s):**
1. Open the check, click Paid and select the target payment method
2. Input the voucher number and member number when prompted
3. Complete the redemption and settle the check

**Test Result(s):**
1. The system triggers the Prepare Redeem Voucher request with the voucher and member information
2. The voucher is redeemed successfully and the check is settled correctly
3. The payment details of the selected payment method are included in the payload when the new setting is Yes

#### 3.3.Payment method without valid Payment Type follows the existing workflow
**Prerequisite(s):**
1. A payment method attached with the General V3 membership interface exists without the Payment Type setting, or with a value other than By input voucher number and By input voucher number and member number

**Step(s):**
1. Open a check with a General V3 member attached, click Paid and select that payment method
2. Observe the system behavior and the interface log

**Test Result(s):**
1. The system follows the existing workflow for the continue operation and does not trigger the Prepare Redeem Voucher request
2. No error is shown and no external call to /vouchers/redeem/prepare is made

### 4.Voucher to Payment Method Validation

#### 4.1.External system rejects mismatched voucher type and payment method
**Prerequisite(s):**
1. The external General V3 system distinguishes Regular and Sponsored vouchers mapped to different payment methods (e.g. Regular voucher = Incert Payment A, Sponsored voucher = Incert Payment B)
2. Payment details are added into the Prepare Redeem Voucher payload (setting Yes)

**Step(s):**
1. Redeem a Regular voucher using the payment method mapped to Incert Payment B
2. Observe the response of the external system and the POS behavior

**Test Result(s):**
1. The Prepare Redeem Voucher payload contains the payment details so the external system can recognize the used payment method
2. The external system rejects the invalid redemption scenario before the redemption transaction is completed
3. The POS displays the error returned by the external system and the redemption is not completed

#### 4.2.Matched voucher type and payment method completes redemption
**Prerequisite(s):**
1. A valid Regular voucher and the payment method mapped to Incert Payment A are available
2. Payment details are added into the Prepare Redeem Voucher payload (setting Yes)

**Step(s):**
1. Redeem the Regular voucher using the payment method mapped to Incert Payment A
2. Complete the redemption and settle the check

**Test Result(s):**
1. The external system validates the voucher-to-payment-method mapping successfully
2. The voucher is redeemed and the redemption transaction is completed without error

### 5.Regression Verification

#### 5.1.Refund prepare payload still contains payment details
**Prerequisite(s):**
1. A check with a redeemed voucher is available for the refund (negative check) operation
2. Interface packet log capture is enabled

**Step(s):**
1. Perform the refund operation that triggers the request to /vouchers/refund/prepare
2. Check the outgoing payload in the interface log

**Test Result(s):**
1. The refund prepare payload still contains the payment details as in the existing behavior
2. The refund flow completes without error
