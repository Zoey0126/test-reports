# TMS2.0/TMSgAPI - When only special character values in certain fields can successfully save the reservation Test Report

## Functional Testing

### 1.Open API Reservation Count Validation

#### 1.1.Open API create reservation with adults less than 1 is rejected

**Prerequisite(s):**
1. Access to the TMSgAPI Open API (version 6.8.0) with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Open API new reservation endpoint with adults set to 0 (and another call with a negative value), keeping all other fields valid.

**Reproduction Result(s):**
1. The API returns success and unexpectedly creates a reservation with 0 adults.

**Fix Result(s):**
1. The API rejects the request with a validation error indicating that adults must be greater than or equal to 1.
2. No reservation is created in the system.

#### 1.2.Open API create reservation with children less than 0 is rejected

**Prerequisite(s):**
1. Access to the TMSgAPI Open API (version 6.8.0) with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Open API new reservation endpoint with children set to -1, keeping all other fields valid.

**Reproduction Result(s):**
1. The API returns success and creates the reservation with the invalid children value.

**Fix Result(s):**
1. The API rejects the request with a validation error indicating that children must be greater than or equal to 0.
2. No reservation is created in the system.

#### 1.3.Open API create reservation with non-numeric or emoji values is rejected

**Prerequisite(s):**
1. Access to the TMSgAPI Open API (version 6.8.0) with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Open API new reservation endpoint filling non-numeric symbols (letters, characters, or emoji) in the adults, children, and gender fields.

**Reproduction Result(s):**
1. The API returns success and unexpectedly creates a reservation with 0 adults.

**Fix Result(s):**
1. The API rejects the request with a validation error for the invalid adults, children, or gender values.
2. No reservation containing special character values is created in the system.

#### 1.4.Open API create reservation with boundary valid values succeeds

**Prerequisite(s):**
1. Access to the TMSgAPI Open API (version 6.8.0) with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Open API new reservation endpoint with adults set to 1 and children set to 0, keeping all other fields valid.
2. Retrieve the created reservation and check the adults and children values.

**Reproduction Result(s):**
1. Not applicable - the valid payload flow was not affected by the reported issue.

**Fix Result(s):**
1. The API returns success and the reservation is created with adults = 1 and children = 0.

### 2.Operation API Reservation Count Validation

#### 2.1.Operation API create reservation with adults less than 1 is rejected

**Prerequisite(s):**
1. Access to the TMSgAPI Operation API with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Operation API new reservation endpoint with adults set to 0, keeping all other fields valid.

**Reproduction Result(s):**
1. The API returns success and creates a reservation with 0 adults.

**Fix Result(s):**
1. The API rejects the request with a validation error indicating that adults must be greater than or equal to 1.
2. No reservation is created in the system.

#### 2.2.Operation API create reservation with special character values is rejected

**Prerequisite(s):**
1. Access to the TMSgAPI Operation API with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Operation API new reservation endpoint filling letters, characters, or emoji in the adults, children, or gender fields.

**Reproduction Result(s):**
1. The API returns success and creates the reservation with invalid field values.

**Fix Result(s):**
1. The API rejects the request with a validation error for the invalid field values.
2. No reservation containing special character values is created in the system.

#### 2.3.Operation API create reservation with valid counts succeeds

**Prerequisite(s):**
1. Access to the TMSgAPI Operation API with valid API credentials.
2. A valid shop, outlet, and reservation payload template are available.

**Step(s):**
1. Call the Operation API new reservation endpoint with valid counts (adults >= 1 and children >= 0).

**Reproduction Result(s):**
1. Not applicable - the valid payload flow was not affected by the reported issue.

**Fix Result(s):**
1. The API returns success and the reservation is created with the submitted adults and children values.

### 3.Reservation Import Validation Exception

**Prerequisite(s):**
1. Tester account has access to Table Management System > Reservation Tools > Import and Export.
2. TMS reservation data is prepared, including records with audit = 0.

**Step(s):**
1. Go to Table Management System > Reservation Tools > Import and Export.
2. Import the reservation file containing records with audit = 0.
3. Download the result file after the import completes.

**Reproduction Result(s):**
1. Not applicable - the import path behavior is unchanged by this fix.

**Fix Result(s):**
1. Import reservation still allows audit = 0 and the import completes successfully.
2. The downloaded result file shows the imported reservation records as expected.

### 4.Regression Scope

1. TMSgAPI Open API new reservation with normal valid payloads (adults >= 1, children >= 0).
2. TMSgAPI Operation API new reservation with normal valid payloads.
3. Reservation creation from the TMS Operation UI with normal adults/children values.
4. Reservation import and export flow in Table Management System > Reservation Tools > Import and Export.
5. Display of existing reservations with their adults and children values in the reservation list.
