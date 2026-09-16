# POS API - accept API user number as employee id if request mark delivery from Shiji KDS (QA Testing) Test Report

## Functional Testing

### 1.Interface Code Parameter in Mark Delivery API

#### 1.1.New interfacecode Field Accepted by Mark Delivery API

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. A target Shiji KDS interface is configured and attached to the outlet setting.

**Step(s):**
1. Send a mark delivery request to the API portal that includes the new `interfacecode` parameter set to the target Shiji KDS interface.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The API accepts the new `interfacecode` parameter without validation errors.
2. The request is processed as a Shiji KDS interface mark delivery request.
3. No errors displayed.

#### 1.2.Request Without interfacecode Follows Existing Handling

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.

**Step(s):**
1. Send a mark delivery request to the API portal that does NOT include the `interfacecode` parameter.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The API follows the existing handling to process mark delivery (uses the `applieduserno` from the request).
2. The behaviour is unchanged compared with the pre-change API contract.
3. No errors displayed.

#### 1.3.Request With Non-Shiji-KDS interfacecode Follows Existing Handling

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. An interface of a type other than Shiji KDS is configured.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to a non-Shiji-KDS interface.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system follows the existing handling to process mark delivery (uses the `applieduserno` from the request).
2. The behaviour is unchanged compared with the pre-change API contract.
3. No errors displayed.

### 2.Default API Portal Request User No. Setup

#### 2.1.New Setup Field Available on Shiji KDS Interface Record

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The signed-in user can access Infrasys POS platform: Interface Control → Interfaces.
3. A Shiji KDS interface record exists.

**Step(s):**
1. Navigate to Interface Control → Interfaces.
2. Edit the target Shiji KDS interface record.
3. Verify that the "Default API Portal Request User No." field is available in the interface setup.
4. Enter a valid user number into the field.
5. Click "Save" to save the record.

**Test Result(s):**
1. The "Default API Portal Request User No." field is available only on Shiji KDS interface records.
2. The field is not mandatory and can be saved empty.
3. The saved value persists after the page is reopened.
4. No errors displayed.

#### 2.2.Field Description Matches Release Note

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The signed-in user can access Infrasys POS platform: Interface Control → Interfaces.

**Step(s):**
1. Open the Shiji KDS interface setup dialog.
2. Read the description of the "Default API Portal Request User No." field.

**Test Result(s):**
1. The field description matches "Default mark delivery API portal request user no. Only available for Mark Delivery function in API Portal".
2. The field is marked as not mandatory.
3. No errors displayed.

### 3.Mark Delivery With Default API Portal Request User No.

#### 3.1.System Overwrites applieduserno With Default API Portal Request User No.

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The target Shiji KDS interface is attached to the outlet setting.
3. The "Default API Portal Request User No." is set to a valid user number on the Shiji KDS interface record.
4. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to the Shiji KDS interface and a different `applieduserno` value in the request body.
2. Verify which user is used to verify the mark delivery action.

**Test Result(s):**
1. The system overwrites the value of `applieduserno` from the request with the value in "Default API Portal Request User No.".
2. The mark delivery action is verified using the default user.
3. The action succeeds if the default user is valid.
4. No errors displayed.

#### 3.2.Default API Portal Request User No. Empty Falls Back to Request applieduserno

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The target Shiji KDS interface is attached to the outlet setting.
3. The "Default API Portal Request User No." is empty on the Shiji KDS interface record.
4. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to the Shiji KDS interface and a valid `applieduserno` in the request body.
2. Verify which user is used to verify the mark delivery action.

**Test Result(s):**
1. The system follows the existing handling to process mark delivery (uses the `applieduserno` from the request).
2. The action succeeds if the request `applieduserno` is a valid user.
3. No errors displayed.

#### 3.3.Invalid Default API Portal Request User No. Returns Error

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The target Shiji KDS interface is attached to the outlet setting.
3. The "Default API Portal Request User No." is set to an invalid user number on the Shiji KDS interface record.
4. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to the Shiji KDS interface.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system returns an error message for an invalid user (following the existing handling).
2. The mark delivery action is not performed.
3. No errors displayed.

### 4.Workflow Order Verification

#### 4.1.Shiji KDS Interface With Valid Default User Reaches Step 9

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The target Shiji KDS interface is attached to the outlet setting.
3. The "Default API Portal Request User No." is set to a valid user number.
4. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` = Shiji KDS interface.
2. Verify the workflow path: step 1 → step 2 → step 5 → step 6 → step 7 → step 9 → step 10/11 → existing mark delivery handling.

**Test Result(s):**
1. The system follows the documented workflow steps 1, 2, 5, 6, 7, 9, 11.
2. The default user is used to verify the mark delivery action.
3. The mark delivery action succeeds.
4. No errors displayed.

#### 4.2.Non-Shiji-KDS Interface Skips to Step 8

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. An interface of a type other than Shiji KDS is configured.
3. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to the non-Shiji-KDS interface.
2. Verify the workflow path: step 1 → step 2 → step 4 → step 8 → step 9 → step 10/11.

**Test Result(s):**
1. The system skips step 6 and step 7 and uses the `applieduserno` from the request (step 8).
2. The mark delivery action is processed using the request `applieduserno`.
3. No errors displayed.

#### 4.3.Missing interfacecode Skips to Step 3

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request without the `interfacecode` parameter.
2. Verify the workflow path: step 1 → step 3 → existing mark delivery handling.

**Test Result(s):**
1. The system follows step 3 (existing handling to process mark delivery).
2. The `applieduserno` from the request is used.
3. No errors displayed.

### 5.Outlet Setting Verification

#### 5.1.Shiji KDS Interface Attached to Outlet Setting

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The signed-in user can access the outlet setting page.

**Step(s):**
1. Open the outlet setting.
2. Verify the target Shiji KDS interface is attached to the outlet.

**Test Result(s):**
1. The Shiji KDS interface can be attached to the outlet setting.
2. The attached interface is used by the mark delivery API when `interfacecode` matches it.
3. No errors displayed.

#### 5.2.Outlet Without Shiji KDS Interface Attached

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. An outlet without a Shiji KDS interface attached exists.

**Step(s):**
1. Send a mark delivery request with `interfacecode` = a Shiji KDS interface to the outlet without that interface attached.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system does not apply the Default API Portal Request User No. override (because the interface is not attached to the outlet).
2. The system follows the existing handling to process mark delivery.
3. No errors displayed.

### 6.Negative and Edge Cases

#### 6.1.Request With Empty interfacecode Value

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to an empty string.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system treats the empty `interfacecode` as a missing parameter and follows the existing handling.
2. No unhandled error is displayed.
3. No errors displayed.

#### 6.2.Request With Unknown interfacecode Value

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to a non-existent interface code.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system follows the existing handling (step 4 → step 8) because the interface code is not a Shiji KDS interface.
2. The mark delivery action is processed using the request `applieduserno`.
3. No errors displayed.

#### 6.3.Request With Malformed interfacecode Value

**Prerequisite(s):**
1. A build with the HERO-68437 change is deployed.
2. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request with `interfacecode` set to a malformed value (very long string, special characters).
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system handles the malformed value gracefully.
2. The request is processed as a non-Shiji-KDS interface request (step 4 → step 8).
3. No unhandled server error is displayed.

### 7.Regression Scope

- Existing mark delivery requests through API portal without the `interfacecode` parameter must continue to work.
- Existing mark delivery requests through normal POS workstations remain unaffected.
- Other API portal functions (not mark delivery) remain unaffected by the new field.
- Shiji KDS interface setup and existing KDS interface features remain unaffected.
- Other interface types configured under Interface Control → Interfaces remain unaffected.

## Test Environment

- **Version**: Infrasys POS Platform release containing the HERO-68437 change
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
