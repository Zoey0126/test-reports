# TMS 2.0 - Pass Reservation Info for PayBy Payment Request Landing Page Test Report

## Functional Testing

### 1.PayBy Payment Method Additional Parameters

#### 1.1.Verify New Additional Parameters Checkboxes Are Displayed
**Prerequisite(s):**
1. HQ is available and the user has the authority to maintain payment methods
2. A PayBy Payment interface payment method exists

**Step(s):**
1. Go to TMS > Payment Methods
2. Edit the PayBy Payment interface payment method
3. Open the "Additional Parameters (Reservation)" section
4. Review the checkboxes available in the section

**Test Result(s):**
1. The new checkbox "Outlet Name (outlet_name)" is displayed and checked by default on the new/edit form
2. The new checkbox "Total Cover (total_cover)" is displayed
3. The new checkbox "Remark (deposit remark)" is displayed
4. The existing reservation additional parameter checkboxes are still displayed and unchanged

#### 1.2.Verify Reservation Info Is Displayed on the PayBy Landing Page
**Prerequisite(s):**
1. The PayBy payment method has the checkboxes Outlet Name, Total Cover and Remark enabled and saved
2. The PayBy payment gateway is available and the landing page can be opened
3. A reservation with covers and special remarks exists

**Step(s):**
1. Go to TMS > Payment Methods and enable Outlet Name, Total Cover and Remark for the PayBy payment method
2. Save the payment method
3. Create a reservation with total covers and special remarks, then select the PayBy payment request
4. Complete the reservation payment request creation
5. Open the PayBy payment landing page

**Test Result(s):**
1. The create-payment request sends the additional reservation and payment fields to PayBy
2. The landing page shows the outlet name of the current language
3. The landing page shows the total cover matching the reservation guest count (resv_guests)
4. The landing page shows the payment remark from the deposit remark / rpay_remark
5. The guest is able to review the reservation details before payment

#### 1.3.Verify Fields Are Not Passed When the Checkboxes Are Disabled
**Prerequisite(s):**
1. The PayBy payment method has Outlet Name, Total Cover and Remark disabled and saved
2. A reservation payment request can be created

**Step(s):**
1. Go to TMS > Payment Methods and uncheck Outlet Name, Total Cover and Remark
2. Save the payment method
3. Create a reservation and the PayBy payment request
4. Open the PayBy landing page and review the displayed fields

**Test Result(s):**
1. The disabled additional parameters are not sent in the create-payment request
2. The corresponding information is not shown on the PayBy landing page
3. The landing page is displayed without error

#### 1.4.Verify Existing Reservation Additional Parameters Are Unchanged
**Prerequisite(s):**
1. The PayBy payment method has existing reservation additional parameters configured, such as reservationOutlet
2. A reservation payment request can be created

**Step(s):**
1. Go to TMS > Payment Methods and review the existing reservation additional parameters
2. Create a reservation and the PayBy payment request
3. Open the PayBy landing page
4. Compare the displayed information with the configured parameters

**Test Result(s):**
1. The existing reservation additional parameters are still displayed in the payment method setup
2. The existing parameters are still sent in the create-payment request
3. The existing reservation information is displayed on the landing page as before

#### 1.5.Verify Existing PayBy Payment Method Must Be Edited and Saved Again
**Prerequisite(s):**
1. A PayBy payment method was configured before this version and has not been edited after the upgrade
2. The PayBy payment gateway is available

**Step(s):**
1. After the upgrade, create a reservation and the PayBy payment request without editing the existing payment method
2. Open the PayBy landing page
3. Edit and save the PayBy payment method with the new checkboxes enabled
4. Create the payment request again and open the landing page

**Test Result(s):**
1. The new checkboxes are not stored in settings until the existing payment method is edited and saved again
2. The outlet name, total cover and remark are displayed after the payment method is edited and saved
3. No error occurs when the payment request is created before the payment method is re-saved

#### 1.6.Verify View Page Shows the Selected Additional Parameters
**Prerequisite(s):**
1. The PayBy payment method is saved with the new additional parameters selected

**Step(s):**
1. Go to TMS > Payment Methods and open the view page of the PayBy payment method
2. Review the displayed additional parameters
3. Compare the view page with existing reservation fields of the same method

**Test Result(s):**
1. The view page shows the selected additional parameters
2. The selected parameters are displayed in the same way as the existing reservation fields
3. The unselected parameters are not shown as selected

#### 1.7.Verify Remark Value From Deposit Remark
**Prerequisite(s):**
1. The PayBy payment method has the Remark checkbox enabled
2. A reservation has a deposit remark / rpay_remark value

**Step(s):**
1. Create a reservation with a deposit remark containing special characters and line breaks
2. Create the PayBy payment request and open the landing page
3. Review the remark displayed on the page

**Test Result(s):**
1. The remark value is mapped to the top-level PayBy field comment
2. The remark is displayed completely with the special characters preserved
3. The remark is shown as the payment remark on the landing page

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- TMS > Payment Methods setup for PayBy Payment interface payment methods
- Additional Parameters (Reservation) for other payment interfaces
- Reservation creation with deposit payment through PayBy
- PayBy payment request creation, payment result and cancellation
- PayBy landing page display for existing reservations
- Reservation additional parameter mapping for other third-party payments
- Payment method save and view pages
