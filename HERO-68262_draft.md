# SJM - Gaming System - Wording Issue for Triggering the Scan Barcode Function Test Report

## Functional Testing

### 1.Scan Barcode Popup Message Verification

#### 1.1.Verify Updated Popup Message For Enquiry Functions
**Prerequisite(s):**
1. The SJM gaming interface is configured and available on the POS station
2. The following enquiry functions are accessible: Gaming Enquiry, Comp Inquiry and Patron Tier Discount

**Step(s):**
1. Open a check and trigger the Gaming Enquiry function
2. Observe the popup message that prompts the user to scan a barcode
3. Repeat the same trigger flow for Comp Inquiry
4. Repeat the same trigger flow for Patron Tier Discount

**Test Result(s):**
1. The popup message for Gaming Enquiry displays "Scan barcode" and "Please scan barcode" only
2. The popup message for Comp Inquiry displays "Scan barcode" and "Please scan barcode" only
3. The popup message for Patron Tier Discount displays "Scan barcode" and "Please scan barcode" only
4. The previous wording "Scan barcode / Swipe card" and "Please scan barcode / swipe card" is no longer displayed

#### 1.2.Verify Updated Popup Message For Payment Interfaces
**Prerequisite(s):**
1. The SJM gaming interface is configured and available on the POS station
2. The following payment interfaces are accessible: Redeem Dollar, Redeem Comp By Account Number and Redeem Comp By Physical Slip

**Step(s):**
1. Open a check and proceed to cashier
2. Trigger the Redeem Dollar payment method
3. Observe the popup message that prompts the user to scan a barcode
4. Repeat the same trigger flow for Redeem Comp By Account Number
5. Repeat the same trigger flow for Redeem Comp By Physical Slip

**Test Result(s):**
1. The popup message for Redeem Dollar displays "Scan barcode" and "Please scan barcode" only
2. The popup message for Redeem Comp By Account Number displays "Scan barcode" and "Please scan barcode" only
3. The popup message for Redeem Comp By Physical Slip displays "Scan barcode" and "Please scan barcode" only
4. The "swipe card" wording is removed from all SJM payment interface popup messages

#### 1.3.Verify Scan Barcode Icon Update
**Prerequisite(s):**
1. The SJM gaming interface is configured and available on the POS station
2. Any of the enquiry functions or payment interfaces are accessible

**Step(s):**
1. Open a check and trigger any of the SJM enquiry functions or payment interfaces
2. Observe the scan barcode icon displayed in the popup

**Test Result(s):**
1. The updated scan barcode icon is displayed correctly in the popup
2. The icon aligns with the new wording and reflects the barcode scanning functionality only

### 2.Gaming Tier Enquiry Coverage

#### 2.1.Verify Wording Update For Gaming Tier Enquiry
**Prerequisite(s):**
1. The SJM gaming interface is configured and available on the POS station
2. The Gaming Tier Enquiry function is accessible

**Step(s):**
1. Open a check and trigger the Gaming Tier Enquiry function
2. Observe the popup message that prompts the user to scan a barcode

**Test Result(s):**
1. The popup message for Gaming Tier Enquiry displays "Scan barcode" and "Please scan barcode" only
2. The "swipe card" wording is removed from the Gaming Tier Enquiry popup

### 3.Vertical Mobile View Exclusion

#### 3.1.Verify Scanner Workflow Not Available In Vertical Mobile View
**Prerequisite(s):**
1. The POS station is configured with vertical mobile view
2. The SJM gaming interface is configured and available

**Step(s):**
1. Switch the POS to vertical mobile view
2. Attempt to trigger any SJM enquiry function or payment interface that requires barcode scanning

**Test Result(s):**
1. The scanner workflow and popup are not shown in vertical mobile view because there is no workflow for scanner devices in mobile view
2. No incorrect wording or unexpected popup is displayed in vertical mobile view

### 4.Regression Scope

The following related modules and scenarios must be retested after the change:

- SJM Gaming Enquiry popup message and icon
- SJM Comp Inquiry popup message and icon
- SJM Gaming Tier Enquiry popup message and icon
- SJM Patron Tier Discount popup message and icon
- SJM Redeem Dollar payment popup message and icon
- SJM Redeem Comp By Account Number payment popup message and icon
- SJM Redeem Comp By Physical Slip payment popup message and icon
- Existing SJM gaming interface functions continue to operate correctly after the wording change
