# HERO-69026 Test Report

## Functional Testing

### F1.GEMS Post Online Comp — Mask Date Of Birth Input On Mobile View

#### F1.1.Verify Station Group Configuration For Masking Date Of Birth

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. A GEMS gaming interface is configured on Infrasys Cloud platform > Interface Control > Interfaces (vendor key: gems) with the "Station Groups for Masking Date of Birth (Setup 1)" populated with the target station group L1
3. The GEMS gaming interface has the "Post Online Comp" payment type attached
4. Test environment: Local / HQ test environment

**Step(s):**
1. Open the Infrasys Cloud platform > Interface Control > Interfaces and select the target GEMS gaming interface
2. Verify the interface setup includes the "Station Groups for Masking Date of Birth (Setup 1)" field
3. Verify the field is populated with the target station group L1 and save the configuration
4. Confirm the target mobile station belongs to the configured station group L1

**Test Result(s):**
1. The GEMS gaming interface setup page shows the "Station Groups for Masking Date of Birth (Setup 1)" field as per the new feature
2. The target station group L1 is saved successfully in Setup 1
3. The target mobile station is confirmed to belong to the configured station group L1

#### F1.2.Verify Date Of Birth Input Is Masked On Vertical Mobile View For Configured Station Group

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. The GEMS gaming interface is configured with the "Station Groups for Masking Date of Birth (Setup 1)" populated with the target station group L1
3. The current station belongs to the configured station group L1 and the device is set to vertical mobile view
4. A printed check exists for which the Post Online Comp payment will be applied
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client
2. Click the function "Paid" to go to the cashier
3. Select the payment method attached with the GEMS gaming interface having the payment type "Post Online Comp"
4. Input the patron number
5. Click the button "Enquiry"
6. Verify the system triggers the external party API for enquiry and displays the patron details returned in the response
7. Verify the date of birth input field on the mobile view is masked when entering the date of birth (for example, characters are masked as the user types)

**Test Result(s):**
1. The "Paid" function opens the cashier screen and the GEMS "Post Online Comp" payment method is selectable
2. After inputting the patron number and clicking "Enquiry", the system triggers the external party API for enquiry and displays the patron details returned in the response
3. The date of birth input field on the vertical mobile view is masked during input — characters entered by the user are not displayed in plain text
4. The masking behaviour only takes effect when the current station belongs to the configured station group L1 and the device is set to vertical mobile view

### F2.Non-Configured Station / Non-Mobile Device Behaviour

#### F2.1.Verify Date Of Birth Input Is Unmasked When Station Is Not In The Configured Station Group

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. The GEMS gaming interface is configured with the "Station Groups for Masking Date of Birth (Setup 1)" populated with a station group that does NOT include the current station (or Setup 1 is empty)
3. The GEMS gaming interface has the "Post Online Comp" payment type attached
4. A printed check exists for which the Post Online Comp payment will be applied
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client
2. Click the function "Paid" to go to the cashier
3. Select the payment method attached with the GEMS gaming interface having the payment type "Post Online Comp"
4. Input the patron number
5. Click the button "Enquiry"
6. Verify the system triggers the external party API for enquiry and displays the patron details returned in the response
7. Verify the date of birth input field on the mobile view is NOT masked (displays input in plain text, following existing handling)

**Test Result(s):**
1. The GEMS "Post Online Comp" payment method is selectable and the enquiry is triggered successfully
2. The date of birth input field on the mobile view is NOT masked — input is displayed in plain text, following the existing handling
3. The system ends the operation following the existing handling when the current station does not belong to any station groups configured in Setup 1, or Setup 1 is empty

#### F2.2.Verify Date Of Birth Input Is Unmasked When Device Is Not Vertical Mobile

**Prerequisite(s):**
1. The fixed POS client build is deployed on a horizontal Workstation (Windows 11 Enterprise)
2. The GEMS gaming interface is configured with the "Station Groups for Masking Date of Birth (Setup 1)" populated with a station group that DOES include the current station
3. The GEMS gaming interface has the "Post Online Comp" payment type attached
4. A printed check exists for which the Post Online Comp payment will be applied
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client on the horizontal Workstation
2. Click the function "Paid" to go to the cashier
3. Select the payment method attached with the GEMS gaming interface having the payment type "Post Online Comp"
4. Input the patron number
5. Click the button "Enquiry"
6. Verify the date of birth input field is NOT masked (because the device is not a vertical mobile device)

**Test Result(s):**
1. The GEMS "Post Online Comp" payment method is selectable and the enquiry is triggered successfully on the horizontal Workstation
2. The date of birth input field is NOT masked on the horizontal Workstation, even when the station belongs to the configured station group — the masking only takes effect on vertical mobile view
3. The system displays the input of date of birth unmasked, following the existing handling and ending the operation accordingly

### F3.Regression — Existing GEMS Enquiry And Payment Flow

#### F3.1.Verify Existing GEMS Enquiry And Post Online Comp Payment Flow Is Preserved

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. The GEMS gaming interface is configured with the "Station Groups for Masking Date of Birth (Setup 1)" populated with the target station group L1
3. The current station belongs to the configured station group L1 and the device is set to vertical mobile view
4. A printed check exists for which the Post Online Comp payment will be applied
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client
2. Click the function "Paid" to go to the cashier
3. Select the GEMS "Post Online Comp" payment method
4. Input the patron number, click "Enquiry", and verify the patron enquiry response is displayed correctly
5. Input the (masked) date of birth and proceed with the Post Online Comp payment
6. Verify the Post Online Comp payment is applied to the check and the check is closed correctly
7. Verify the corresponding redemption record is reflected correctly in the Comp Report (CUS0272) / TA And Voucher Redemption Report (POS0163) on the local server

**Test Result(s):**
1. The GEMS "Post Online Comp" payment enquiry flow completes successfully with the date of birth input masked
2. The Post Online Comp payment is applied to the check and the check is closed correctly
3. The corresponding redemption record is reflected correctly in the Comp Report (CUS0272) / TA And Voucher Redemption Report (POS0163) — no regression in the redemption reporting is introduced by the date of birth masking feature

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69026):**

- Available for GEMS gaming interface; available for vertical mobile view ONLY.
- System supports to mask date of birth input for post online comp payment.
- Workflow: 1) Open a printed check; 2) Click function "Paid" to go to cashier; 3) Select the payment method attached with GEMS gaming interface having the payment type "Post Online Comp"; 4) Input the patron number; 5) Click button "Enquiry"; 6) The system triggers the external party API for enquiry; 7) The system displays the parton details returned in the response; 8) If current station belongs to a station group configured in Setup 1 AND station is vertical device, the system masking the input of date of birth; 9) If the current station does not belong to any station groups configured in Setup 1, or Setup 1 is empty, or station is not vertical device, the system displays input of date of birth unmasked, follow all existing handling and end of operation.
- The feature must take effect on mobile POS only (configuration by workstation or by station group).
