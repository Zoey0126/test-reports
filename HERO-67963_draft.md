# POS Function - Duty Meal / On Credit Limits - Allow Decimal Values for Payment Method Limit Balance Test Report

## Functional Testing

### 1.Duty Meal Limit Settings Accept Decimal Values

#### 1.1.Duty Meal User Group Payment Method Limit
**Prerequisite(s):**
1. User is logged in with authority to edit Config by Location Duty Meal settings
2. Duty Meal User Group Payment Method Limit setting is accessible

**Step(s):**
1. Open Config by Location > Duty Meal User Group Payment Method Limit
2. Enter a positive decimal value such as 10.50
3. Save the setting
4. Re-open the setting and verify the displayed value

**Test Result(s):**
1. The decimal value is accepted and saved exactly as entered
2. The value 10.50 is displayed correctly after save
3. The value is processed correctly in POS Duty Meal payment flows

#### 1.2.Duty Meal Shop Limit, Outlet Limit And Check Limit
**Prerequisite(s):**
1. User is logged in with authority to edit Config by Location Duty Meal Shop Limit, Outlet Limit and Check Limit

**Step(s):**
1. Open Duty Meal Shop Limit and enter 10.50, then save
2. Open Duty Meal Outlet Limit and enter 25.75, then save
3. Open Duty Meal Check Limit and enter 10.999, then save
4. Re-open each setting and verify the saved values

**Test Result(s):**
1. Shop Limit saves 10.50 exactly
2. Outlet Limit saves 25.75 exactly
3. Check Limit saves 10.999 exactly with no cap on decimal places
4. No error is shown when saving valid positive decimal values

### 2.On Credit Limit Settings Accept Decimal Values

#### 2.1.On Credit User Group Payment Method Limit
**Prerequisite(s):**
1. User is logged in with authority to edit Config by Location On Credit settings
2. On Credit User Group Payment Method Limit setting is accessible

**Step(s):**
1. Open On Credit User Group Payment Method Limit
2. Enter a positive decimal value such as 25.75
3. Save the setting
4. Re-open the setting and verify the displayed value

**Test Result(s):**
1. The decimal value is accepted and saved exactly as entered
2. The value 25.75 is displayed correctly after save
3. The value is processed correctly in POS On Credit payment flows

#### 2.2.On Credit Shop Limit, Outlet Limit And Check Limit
**Prerequisite(s):**
1. User is logged in with authority to edit Config by Location On Credit Shop Limit, Outlet Limit and Check Limit

**Step(s):**
1. Open On Credit Shop Limit and enter 10.50, then save
2. Open On Credit Outlet Limit and enter 25.75, then save
3. Open On Credit Check Limit and enter 10.999, then save
4. Re-open each setting and verify the saved values

**Test Result(s):**
1. Shop Limit saves 10.50 exactly
2. Outlet Limit saves 25.75 exactly
3. Check Limit saves 10.999 exactly with no cap on decimal places
4. No error is shown when saving valid positive decimal values

### 3.User Module Duty Meal And On Credit Limits
**Prerequisite(s):**
1. User Management is accessible
2. A POS user record is available for edit

**Step(s):**
1. Go to Users > Module Information > POS System > Duty Meal Limit
2. Enter a positive decimal value such as 10.50 and save
3. Go to Users > Module Information > POS System > On Credit Limit
4. Enter a positive decimal value such as 25.75 and save
5. Re-open both fields and verify the saved values

**Test Result(s):**
1. Duty Meal Limit in User Module Information accepts and saves 10.50
2. On Credit Limit in User Module Information accepts and saves 25.75
3. Values are displayed and processed correctly throughout the POS application

### 4.Existing Integer Values And Invalid Input

#### 4.1.Positive Integer Remains Supported
**Prerequisite(s):**
1. Any of the eight Duty Meal / On Credit limit settings is open

**Step(s):**
1. Enter a positive integer such as 10
2. Save the setting
3. Re-open and verify the displayed value

**Test Result(s):**
1. The integer value is accepted and saved without changes
2. Existing integer configurations continue to work

#### 4.2.Zero, Negative And Non-Numeric Values Are Rejected
**Prerequisite(s):**
1. Any of the eight Duty Meal / On Credit limit settings is open

**Step(s):**
1. Enter 0 and attempt to save
2. Enter a negative value such as -10.50 and attempt to save
3. Enter non-numeric text such as ABC and attempt to save

**Test Result(s):**
1. Zero is rejected and not saved
2. Negative values are rejected and not saved
3. Non-numeric text is rejected and not saved
4. The previous valid value remains unchanged

### 5.Unrestricted Decimal Places Match Existing CBL Pattern
**Prerequisite(s):**
1. Existing CBL settings Duty Meal/On Credit User Group Shop Limit Setting and User Shop Limit Setting are available as the reference pattern
2. One of the newly enhanced limit settings is open

**Step(s):**
1. Enter a value with more than two decimal places such as 10.9999 in a newly enhanced setting
2. Save and re-open the setting
3. Compare the accepted decimal-place behavior with Duty Meal/On Credit User Group Shop Limit Setting

**Test Result(s):**
1. The value is saved exactly as entered with no cap on decimal places
2. The behavior matches the existing User Group Shop Limit Setting and User Shop Limit Setting

### 6.Payment Amount Still Follows Outlet Rounding
**Prerequisite(s):**
1. Duty Meal or On Credit limit is set to 999.9999
2. Outlet Settings payment decimal places = 2 and rounding = round down

**Step(s):**
1. Open a check and settle using Duty Meal or On Credit up to the configured limit
2. Observe the allowed payment amount after outlet rounding

**Test Result(s):**
1. Limit setting stores 999.9999
2. The actual payment amount is still limited by Outlet Settings rounding and becomes 999.99
3. Existing payment workflows are not broken

## Compatibility Testing

### 1.Windows POS Client Decimal Input
**Prerequisite(s):**
1. Windows POS / Platform client with the enhancement deployed is accessible

**Step(s):**
1. Repeat decimal save and payment verification for Duty Meal Check Limit and On Credit Check Limit on the Windows client

**Test Result(s):**
1. Decimal values can be entered, saved and used for payment on the Windows client
2. No client-side validation blocks valid decimal input

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome, Firefox, Edge
- **Environment**: HQ Test Environment

## Appendix

### Acceptance Criteria(from JIRA)
1. Enter decimal value for Duty Meal limit settings (User Group Payment Method, Shop, Outlet, or Check); value is accepted and saved exactly as entered and processed correctly throughout POS
2. Enter decimal value for On Credit limit settings (User Group Payment Method, Shop, Outlet, or Check); value is accepted and saved exactly as entered and processed correctly throughout POS
3. Unrestricted decimal places: value such as 10.999 is saved exactly as entered, matching existing User Group Shop Limit Setting and User Shop Limit Setting
4. Existing integer values remain supported; negative values, non-numeric text and zero are rejected
