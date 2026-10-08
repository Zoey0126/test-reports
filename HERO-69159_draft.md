# HERO-69159 TMS2.0 - Error occurs "function_authority_error_no_function_authority" when the Pending reservation changes to the "No Show" status Test Report

## Functional Testing

### 1.Reproduction - No Show Error with General User All Access Rights

#### 1.1.Mark past reservation as No Show with General User (All access rights)

**Prerequisite(s):**
1. A "General User" account configured with [All access rights] in TMS Operation.
2. TMS Operation module is accessible.

**Step(s):**
1. Log in to TMS Operation using the "General User" account that has [All access rights].
2. Navigate to New Reservation.
3. Make a reservation in past time.
4. Select the pending reservation and mark it as "No Show".

**Reproduction Result(s):**
1. An error message "function_authority_error_no_function_authority" appears when marking the pending reservation as "No Show".
2. The reservation status is not changed to "No Show".
3. The No Show operation fails for the General User even though the user has [All access rights].

### 2.Fix Verification - No Show Operation

#### 2.1.General User with All access rights can mark reservation as No Show without error

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. A "General User" account configured with [All access rights] in TMS Operation.
3. TMS Operation module is accessible.

**Step(s):**
1. Log in to TMS Operation using the "General User" account that has [All access rights].
2. Navigate to New Reservation.
3. Make a reservation in past time.
4. Select the pending reservation and mark it as "No Show".

**Fix Result(s):**
1. No "function_authority_error_no_function_authority" error appears.
2. The No Show operation completes successfully without any authority error.
3. The reservation status is updated to "No Show".

#### 2.2.No Show status applied correctly to the reservation

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. A "General User" account configured with [All access rights] in TMS Operation.
3. A pending reservation in past time has been created.

**Step(s):**
1. Log in to TMS Operation using the "General User" account.
2. Locate the pending reservation created in past time.
3. Mark the reservation as "No Show".
4. Verify the reservation record and status in the reservation list.

**Fix Result(s):**
1. The reservation status is changed to "No Show" and shown in the reservation list.
2. The reservation detail page reflects the "No Show" status correctly.
3. No error is thrown and the operation completes successfully.

### 3.Authority Scope Verification

#### 3.1.Other reservation status changes work for General User with All access rights

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. A "General User" account configured with [All access rights] in TMS Operation.
3. Multiple pending reservations in past time have been created.

**Step(s):**
1. Log in to TMS Operation using the "General User" account.
2. Select a pending reservation and change its status to "Cancelled".
3. Select another pending reservation and change its status to "Confirmed".
4. Select another pending reservation and mark it as "No Show".

**Fix Result(s):**
1. The "Cancelled" status change completes successfully without authority error.
2. The "Confirmed" status change completes successfully without authority error.
3. The "No Show" status change completes successfully without authority error.
4. All status changes for the General User with [All access rights] work as expected.

#### 3.2.No Show works for other user roles and permission configurations

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. User accounts with different roles (e.g., System Administrator, F&B Manager) are available.
3. TMS Operation module is accessible.

**Step(s):**
1. Log in to TMS Operation using a user account with a different role (e.g., System Administrator).
2. Create a reservation in past time.
3. Mark the pending reservation as "No Show".
4. Repeat the steps with another user role (e.g., F&B Manager).

**Fix Result(s):**
1. The System Administrator can mark a reservation as "No Show" without error.
2. The F&B Manager can mark a reservation as "No Show" without error.
3. The fix does not introduce authority errors for other user roles.

### 4.Regression - Reservation Operations

#### 4.1.New reservation creation still works for General User

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. A "General User" account configured with [All access rights] in TMS Operation.

**Step(s):**
1. Log in to TMS Operation using the "General User" account.
2. Navigate to New Reservation.
3. Create a new reservation with valid details.
4. Verify the reservation is created and shown in the reservation list.

**Fix Result(s):**
1. The new reservation is created successfully without any authority error.
2. The reservation appears in the reservation list with the correct details.
3. No regression is introduced in the new reservation creation flow.

#### 4.2.Other reservation operations (edit, search) work correctly for General User

**Prerequisite(s):**
1. The build containing the HERO-69159 fix is deployed to the TMS environment.
2. A "General User" account configured with [All access rights] in TMS Operation.
3. At least one reservation exists in the system.

**Step(s):**
1. Log in to TMS Operation using the "General User" account.
2. Search for an existing reservation using the search function.
3. Edit the reservation details (e.g., change cover, time).
4. Save the changes and verify the reservation is updated.

**Fix Result(s):**
1. The search function returns the expected reservation without authority error.
2. The reservation edit operation completes successfully.
3. The saved changes are reflected in the reservation record.
4. No regression is introduced in the reservation edit and search operations.
