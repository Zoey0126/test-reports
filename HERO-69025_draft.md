# HERO-69025 Test Report

## Functional Testing

### F1.Patron Inquiry — Sensitive Information Masking On Mobile View

#### F1.1.Verify Patron Number Masking In Patron Inquiry On Vertical Mobile View

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. A GEMS gaming interface is configured on Infrasys Cloud platform > Interface Control > Interfaces (vendor key: gems) with the station group setup for masking sensitive information (Setup 1) populated with the target station group
3. The current station belongs to the configured station group and the device is set to vertical mobile view
4. A patron with a known patron number and points balance exists on the GEMS external party for enquiry
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a check on the POS client on the vertical mobile device
2. Click the function "Patron Inquiry"
3. Input the patron number
4. Click the button "Enquiry"
5. Verify the system triggers the external party API for enquiry and displays the patron details returned in the response
6. Verify the patron number field is masked — first 3 characters displayed, remaining characters masked with asterisks (e.g. 12345678 → 123*****)

**Test Result(s):**
1. The "Patron Inquiry" function opens the enquiry screen on the vertical mobile device
2. After inputting the patron number and clicking "Enquiry", the system triggers the external party API for enquiry and displays the patron details returned in the response
3. The patron number field is masked — the first 3 characters are displayed and all remaining characters are masked with asterisks (e.g. 12345678 → 123*****), matching the required mask format

#### F1.2.Verify Point Balance Masking In Patron Inquiry On Vertical Mobile View

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device
2. A GEMS gaming interface is configured with the station group setup for masking sensitive information (Setup 1) populated with the target station group
3. The current station belongs to the configured station group and the device is set to vertical mobile view
4. Patrons with various point balances (e.g. 0, 5, 12, 120, 1200, 12000, 12345.67, -12345.67) exist on the GEMS external party for enquiry
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a check on the POS client on the vertical mobile device
2. Click the function "Patron Inquiry"
3. Input the patron number for a patron with a known point balance (e.g. 12345.67)
4. Click the button "Enquiry"
5. Verify the point balance field is masked — first 2 digits displayed, remaining digits masked with asterisks, decimal places omitted (e.g. 12345.67 → 12***)
6. Repeat the enquiry for patrons with different point balances (0, 5, 12, 120, 1200, 12000, -12345.67) and verify each masked value matches the expected format

**Test Result(s):**
1. The point balance field is masked consistently — first 2 digits displayed, remaining digits masked with asterisks and decimal places omitted
2. The masking results match the expected format for each test value:
   - 0 → 0
   - 5 → 5
   - 12 → 12
   - 120 → 12*
   - 1200 → 12**
   - 12000 → 12***
   - 12345.67 → 12***
   - -12345.67 → -12***
3. The negative sign is preserved when the point balance is negative

#### F1.3.Verify Point Department / Point Earned Today Masking In Patron Inquiry On Vertical Mobile View

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device
2. A GEMS gaming interface is configured with the station group setup for masking sensitive information (Setup 1) populated with the target station group
3. The current station belongs to the configured station group and the device is set to vertical mobile view
4. Patrons with various point department / point earned today values (e.g. 123.45, 12.34, 1.23, 0.12, 1234.56, -1234.56, 12345.67, -12345.67, 123456.78, -123456.78) exist on the GEMS external party for enquiry
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a check on the POS client on the vertical mobile device
2. Click the function "Patron Inquiry"
3. Input the patron number for a patron with a known point department / point earned today value
4. Click the button "Enquiry"
5. Verify the point department / point earned today fields are masked per the integer-portion digit-count rule:
   - 3 or fewer digits before decimal: first 2 digits displayed, remaining integer digits masked with asterisks; decimal portion shown as two asterisks (**) regardless of actual decimal places (e.g. 123.45 → 12*.**; 12.34 → 12.**; 1.23 → 1.**; 0.12 → 0.**)
   - 4 digits before decimal: first 2 digits displayed, remaining integer digit masked with one asterisk; only 1 decimal place shown, masked with one asterisk (e.g. 1234.56 → 12*.*; -1234.56 → -12*.*)
   - 5 or more digits before decimal: first 2 digits displayed, remaining integer digits masked with two asterisks; only 1 decimal place shown, masked with one asterisk (e.g. 12345.67 → 12**.*; -12345.67 → -12**.*; 123456.78 → 12**.*; -123456.78 → -12**.*)
6. Repeat the enquiry for patrons with different point department / point earned today values to cover each digit-count branch

**Test Result(s):**
1. The point department / point earned today fields are masked consistently per the integer-portion digit-count rule
2. The masking results match the expected format for each test value, including the decimal-portion handling and the negative sign preservation
3. The masking is applied uniformly across the patron inquiry response fields on the vertical mobile view

### F2.Post Online Comp — Sensitive Information Masking On Mobile View

#### F2.1.Verify Sensitive Information Masking In Post Online Comp Enquiry On Vertical Mobile View

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device
2. A GEMS gaming interface is configured with the station group setup for masking sensitive information (Setup 1) populated with the target station group
3. The current station belongs to the configured station group and the device is set to vertical mobile view
4. A printed check exists for which the Post Online Comp payment will be applied
5. A patron with known patron number, point balance, point department and point earned today exists on the GEMS external party
6. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client on the vertical mobile device
2. Click the function "Paid" to go to the cashier
3. Select the GEMS "Post Online Comp" payment method
4. Input the patron number
5. Click the button "Enquiry"
6. Verify the system triggers the external party API for enquiry and displays the patron details returned in the response
7. Verify the patron number, point balance, point department and point earned today fields are masked per the rules defined in F1.1, F1.2 and F1.3

**Test Result(s):**
1. The "Post Online Comp" enquiry is triggered successfully and the patron details are returned in the response
2. The patron number, point balance, point department and point earned today fields are masked per the rules defined in F1.1, F1.2 and F1.3 — consistent with the Patron Inquiry masking behaviour
3. The masking takes effect only when the current station belongs to the configured station group and the device is set to vertical mobile view

### F3.Non-Configured Station / Non-Mobile Device Behaviour

#### F3.1.Verify Sensitive Information Is Unmasked When Station Is Not In The Configured Station Group Or Device Is Not Vertical Mobile

**Prerequisite(s):**
1. The fixed POS client build is deployed on a horizontal Workstation (Windows 11 Enterprise) AND on a vertical mobile device whose station is not in the configured station group
2. A GEMS gaming interface is configured with the station group setup for masking sensitive information (Setup 1) populated with a station group that does NOT include the current station (or Setup 1 is empty)
3. A patron with known patron number and points balance exists on the GEMS external party
4. Test environment: Local / HQ test environment

**Step(s):**
1. On the horizontal Workstation, open a check, click "Patron Inquiry", input the patron number and click "Enquiry"
2. Verify the patron number, point balance, point department and point earned today fields are NOT masked — they are displayed in plain text following the existing handling
3. On the vertical mobile device whose station is not in the configured station group, repeat the Patron Inquiry flow
4. Verify the sensitive information fields are NOT masked (because the station does not belong to the configured station group, or Setup 1 is empty)

**Test Result(s):**
1. On the horizontal Workstation, the patron number, point balance, point department and point earned today fields are displayed in plain text — not masked — even when the station belongs to the configured station group (because the device is not vertical mobile)
2. On the vertical mobile device whose station is not in the configured station group (or when Setup 1 is empty), the sensitive information fields are displayed in plain text — not masked
3. The system follows the existing handling and ends the operation when the masking conditions are not met

### F4.Regression — Existing GEMS Enquiry And Redemption Reporting

#### F4.1.Verify Existing GEMS Enquiry And Redemption Reports Are Preserved After Sensitive Information Masking

**Prerequisite(s):**
1. The fixed POS client build is deployed on a vertical mobile device
2. A GEMS gaming interface is configured with the station group setup for masking sensitive information (Setup 1) populated with the target station group
3. The current station belongs to the configured station group and the device is set to vertical mobile view
4. Test environment: Local / HQ test environment

**Step(s):**
1. Open a printed check on the POS client on the vertical mobile device
2. Click the function "Paid" to go to the cashier
3. Select the GEMS "Post Online Comp" payment method, input the patron number and click "Enquiry"
4. Verify the enquiry response is displayed with the sensitive information masked
5. Proceed with the Post Online Comp payment (using the masked display) and close the check
6. Open the Comp Report (CUS0272) / TA And Voucher Redemption Report (POS0163) / Coupon Redemption Details Report (CUS0126) on the local server
7. Verify the redemption records are correctly reflected in the reports — the masking on the enquiry screen does not impact the underlying data captured in the redemption records

**Test Result(s):**
1. The GEMS "Post Online Comp" payment enquiry and payment flow completes successfully with the sensitive information masked on the enquiry screen
2. The Post Online Comp payment is applied to the check and the check is closed correctly
3. The Comp Report (CUS0272) / TA And Voucher Redemption Report (POS0163) / Coupon Redemption Details Report (CUS0126) reflect the redemption records correctly — the sensitive information masking on the enquiry screen does not impact the underlying data captured in the redemption records
4. No regression in the redemption reporting is introduced by the sensitive information masking feature

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69025):**

- Available for GEMS gaming interface; available for vertical mobile view ONLY.
- System supports to mask sensitive patron information in "Patron Inquiry" and "Post Online Comp" payment enquiry screen when the current station belongs to a station group configured in Setup [1] and the device is set to vertical mobile view.
- Mask formats:
  - Patron Number: Display the first 3 characters and mask all remaining characters with asterisks (*). Example: 12345678 → 123*****
  - Point balance: Display the first 2 digits and mask all remaining digits with asterisks (*); omit the decimal places. Examples: 12345.67 → 12***; -12345.67 → -12***
  - Point department / point earned today: Masking rule depends on the number of digits before the decimal separator:
    - 3 or fewer digits: first 2 digits shown, remaining integer digits masked; decimal portion shown as two asterisks (**). Examples: 123.45 → 12*.**; 12.34 → 12.**; 1.23 → 1.**; 0.12 → 0.**
    - 4 digits: first 2 digits shown, remaining integer digit masked with one asterisk; only 1 decimal place shown, masked with one asterisk. Examples: 1234.56 → 12*.*; -1234.56 → -12*.*
    - 5 or more digits: first 2 digits shown, remaining integer digits masked with two asterisks; only 1 decimal place shown, masked with one asterisk. Examples: 12345.67 → 12**.*; -12345.67 → -12**.*; 123456.78 → 12**.*; -123456.78 → -12**.*
- For negative values, the system displays the negative sign as is.
- When the current station does not belong to the configured station group, or Setup [1] is empty, or the device is not vertical mobile view, the system displays the sensitive information unmasked, following the existing handling.
