# General V3 membership interface - SVC coupon enquiry prompts error "Incorrect category type" if interface setup "Discount Details From" value as "Infrasys POS discount setup" Test Report

## Functional Testing

### 1.SVC Coupon Enquiry With Infrasys POS Discount Setup

#### 1.1.Enquire Valid Coupon And View Coupon Information
**Prerequisite(s):**
1. Discount Details From is set as Infrasys POS discount setup for the target General V3 membership interface
2. A member is attached to the check
3. A valid coupon number available for the member is prepared

**Step(s):**
1. Open a new check and add an item
2. Click function "SVC Coupon Enquiry"
3. Input the target coupon number
4. Click button "Enquiry"

**Reproduction Result(s):**
1. With the previous version, system showed error "Incorrect category type" during the enquiry and aborted applying the coupon discount

**Fix Result(s):**
1. System triggers the external party API to enquire the coupon without the error "Incorrect category type"
2. The coupon information is displayed correctly

#### 1.2.Apply Coupon Discount From Infrasys POS Discount Setup
**Prerequisite(s):**
1. Coupon information is displayed successfully in SVC Coupon Enquiry with Discount Details From = Infrasys POS discount setup

**Step(s):**
1. Select the enquired coupon and apply it to the check
2. Check the applied discount on the check

**Reproduction Result(s):**
1. With the previous version, the coupon could not be applied because the enquiry failed with "Incorrect category type"

**Fix Result(s):**
1. The coupon discount from Infrasys POS discount setup is applied to the check
2. The discount amount is calculated correctly and the check total is updated

### 2.Interface Configuration And Remark Verification

#### 2.1.Configure Discount Details From As Infrasys POS Discount Setup
**Prerequisite(s):**
1. Infrasys POS platform is accessible with an account having interface configuration permission
2. A General V3 membership interface record exists

**Step(s):**
1. Go to Interface Control > Interfaces
2. Select the target General V3 membership interface setting
3. Click "Add New" to add a new record or select an existing interface record
4. Select section "Coupon Setup"
5. Set Discount Details From = Infrasys POS discount setup
6. Click "Save" to save the record

**Reproduction Result(s):**
1. With the previous version, the configuration could be saved but it triggered the enquiry error "Incorrect category type" at runtime (defect trigger condition)

**Fix Result(s):**
1. Discount Details From is saved as Infrasys POS discount setup successfully
2. Under this configuration the SVC Coupon Enquiry flow works normally

#### 2.2.Verify Updated Remark Of Discount Details From Setting
**Prerequisite(s):**
1. Discount Details From is set as Infrasys POS discount setup for the target General V3 interface

**Step(s):**
1. Reopen the interface record in Interface Control > Interfaces
2. Check the remark text shown for Discount Details From

**Reproduction Result(s):**
1. With the previous version, the remark showed Only for category "item" or "check"

**Fix Result(s):**
1. The remark is updated to Only for function "SVC Coupon Enquiry"
2. The remark Only for category "item" or "check" is removed

### 3.Enquiry With External Interface Discount Details Option
**Prerequisite(s):**
1. Discount Details From is set as the external interface option (not Infrasys POS discount setup)

**Step(s):**
1. Open a new check and add an item
2. Click function "SVC Coupon Enquiry"
3. Input a valid coupon number and click button "Enquiry"

**Reproduction Result(s):**
1. Control scenario - with Discount Details From set to the external interface option, the error "Incorrect category type" did not occur before the fix

**Fix Result(s):**
1. The error "Incorrect category type" is not shown
2. Coupon enquiry and apply flows work as before with no regression

### 4.Regression Scope
1. General V3 interface outlet configuration - Coupon Setup section in Interface Control > Interfaces
2. SVC Coupon Enquiry - enquiry and apply coupon flows
3. Coupon enquiry with Discount Details From = external interface option
4. Coupon discount calculation on the check
5. Other membership functions using the same General V3 interface
