# HERO-67963 Test Report

## Functional Testing

### 1. Duty Meal Limit Settings - Decimal Value Input

#### 1.1 Duty Meal User Group Payment Method Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. A valid Duty Meal User Group is configured in the system
3. The Duty Meal User Group Payment Method Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the Duty Meal User Group Payment Method Limit setting
3. Enter a positive decimal value, e.g., "10.50"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "10.50" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "10.50" accurately
4. No rounding or truncation is applied to the decimal value

#### 1.2 Duty Meal Shop Limit - Decimal Value with Multiple Decimal Places

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. A valid Shop (Outlet) is configured in the system
3. The Duty Meal Shop Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the Duty Meal Shop Limit setting
3. Enter a value with multiple decimal places, e.g., "10.999"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The value "10.999" is accepted without validation error
2. The value is saved exactly as entered with no cap on decimal places
3. When reopened, the setting displays "10.999" accurately
4. Behavior matches the existing Duty Meal/On Credit User Group Shop Limit Setting

#### 1.3 Duty Meal Outlet Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. The Duty Meal Outlet Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the Duty Meal Outlet Limit setting
3. Enter a positive decimal value, e.g., "25.75"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "25.75" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "25.75" accurately

#### 1.4 Duty Meal Check Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. The Duty Meal Check Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the Duty Meal Check Limit setting
3. Enter a positive decimal value, e.g., "5.50"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "5.50" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "5.50" accurately

### 2. On Credit Limit Settings - Decimal Value Input

#### 2.1 On Credit User Group Payment Method Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. A valid On Credit User Group is configured in the system
3. The On Credit User Group Payment Method Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the On Credit User Group Payment Method Limit setting
3. Enter a positive decimal value, e.g., "15.25"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "15.25" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "15.25" accurately

#### 2.2 On Credit Shop Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. The On Credit Shop Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the On Credit Shop Limit setting
3. Enter a positive decimal value with multiple places, e.g., "20.888"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The value "20.888" is accepted without validation error
2. The value is saved exactly as entered with no cap on decimal places
3. When reopened, the setting displays "20.888" accurately

#### 2.3 On Credit Outlet Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. The On Credit Outlet Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the On Credit Outlet Limit setting
3. Enter a positive decimal value, e.g., "30.10"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "30.10" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "30.10" accurately

#### 2.4 On Credit Check Limit - Decimal Value Accepted

**Prerequisite(s):**
1. Access to POS System configuration > Config By Location is available
2. The On Credit Check Limit setting is accessible

**Step(s):**
1. Navigate to POS System > Config By Location
2. Locate the On Credit Check Limit setting
3. Enter a positive decimal value, e.g., "8.99"
4. Save the configuration
5. Reopen the setting to verify the saved value

**Test Result(s):**
1. The decimal value "8.99" is accepted without validation error
2. The value is saved exactly as entered
3. When reopened, the setting displays "8.99" accurately

### 3. User Module Information - Duty Meal and On Credit Limit Decimal Support

#### 3.1 Duty Meal Limit in User Module - Decimal Value Accepted

**Prerequisite(s):**
1. Access to User Management > Users is available
2. A valid user account exists
3. The user has POS System module access configured

**Step(s):**
1. Navigate to User Management > Users
2. Select and open the target user
3. Go to Module Information > POS System
4. Locate Duty Meal Limit field
5. Enter a positive decimal value, e.g., "12.34"
6. Save the user configuration
7. Reopen the user record to verify

**Test Result(s):**
1. The decimal value "12.34" is accepted in the Duty Meal Limit field
2. The value is saved exactly as entered
3. When reopened, the setting displays "12.34" accurately

#### 3.2 On Credit Limit in User Module - Decimal Value Accepted

**Prerequisite(s):**
1. Access to User Management > Users is available
2. A valid user account exists with POS System module access

**Step(s):**
1. Navigate to User Management > Users
2. Select and open the target user
3. Go to Module Information > POS System
4. Locate On Credit Limit field
5. Enter a positive decimal value, e.g., "18.56"
6. Save the user configuration
7. Reopen the user record to verify

**Test Result(s):**
1. The decimal value "18.56" is accepted in the On Credit Limit field
2. The value is saved exactly as entered
3. When reopened, the setting displays "18.56" accurately

### 4. Input Validation and Existing Behavior Preservation

#### 4.1 Negative Values and Zero Are Rejected

**Prerequisite(s):**
1. Access to Duty Meal/On Credit limit settings is available
2. One of the limit settings (e.g., Duty Meal Check Limit) is open

**Step(s):**
1. Enter a negative value, e.g., "-5.00" in the Duty Meal Check Limit field
2. Attempt to save the configuration
3. Enter a zero value "0" in the same field
4. Attempt to save the configuration
5. Enter non-numeric text, e.g., "abc" in the same field
6. Attempt to save the configuration

**Test Result(s):**
1. Negative value "-5.00" is rejected with an appropriate validation error
2. Zero value "0" is rejected with an appropriate validation error
3. Non-numeric text "abc" is rejected with an appropriate validation error
4. The original value is preserved when invalid input is rejected

#### 4.2 Existing Integer Values Remain Supported

**Prerequisite(s):**
1. Access to Duty Meal/On Credit limit settings is available
2. One of the limit settings (e.g., On Credit Shop Limit) is open

**Step(s):**
1. Enter a positive integer value, e.g., "10" in the On Credit Shop Limit field
2. Save the configuration
3. Reopen the setting to verify

**Test Result(s):**
1. The integer value "10" is accepted without validation error
2. The value is saved without changes
3. When reopened, the setting displays "10" accurately

### 5. Payment Processing with Decimal Limits

#### 5.1 Payment Amount Rounding with Decimal Limit

**Prerequisite(s):**
1. Duty Meal Check Limit is set to a decimal value, e.g., "999.9999"
2. Outlet Settings are configured with Payment Decimal Places = 2 and Rounding Method = Round Down
3. A POS workstation is available for payment testing

**Step(s):**
1. Create a check on the POS workstation with items totaling an amount that exceeds the effective limit after rounding
2. Attempt to pay with Duty Meal payment method
3. Observe the payment processing behavior
4. Check the final payment amount applied

**Test Result(s):**
1. The payment amount respects the Outlet Settings rounding configuration (2 decimal places, round down)
2. With limit 999.9999 and rounding to 2 decimals, the effective final payment amount is 999.99
3. The decimal limit value is correctly processed through the rounding logic
4. No errors or unexpected behavior occurs during payment
