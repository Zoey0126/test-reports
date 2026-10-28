# HERO-69912 Test Report - TMS 2.0 Skip Redundant Table Unlock When Closing Reservation Confirmation

## Functional Testing

### 1.Reservation Save and Confirmation Close - Table Lock Behavior

#### 1.1.Verify Closing the Confirmation Dialog After Save Does Not Unlock Tables
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. The user has the authority to create a reservation
3. At least one available table exists in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and complete the required reservation information
3. Save the reservation
4. Review the table lock state after the reservation is saved
5. Close the confirmation dialog
6. Review the table lock state again and check the table status of the selected table

**Test Result(s):**
1. The selected table is unlocked when the reservation is saved
2. Closing the confirmation dialog does not send another table unlock request
3. The selected table is not released again after the confirmation dialog is closed
4. The reservation is saved correctly and displayed on the selected table
5. No unexpected table unlock log is recorded for the confirmation close

#### 1.2.Verify Save and New Does Not Conflict With the Table Lock of the Next Reservation
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. At least two available tables exist in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select a table and save the reservation by using "Save and New"
3. Immediately select the same table for the next reservation in the new form
4. Save the second reservation
5. Review the table lock state and the reservation records of the table

**Test Result(s):**
1. The late unlock from the confirmation close does not run at the same time as the table lock of the next reservation
2. The same table can be locked again for the next reservation without conflict
3. Both reservations are saved correctly
4. The table status reflects the latest reservation without unexpected unlocking
5. No error message or lock conflict is displayed

#### 1.3.Verify Removing a Table Still Unlocks the Table
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. An available table exists in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and review the table lock state
3. Remove the selected table from the reservation form
4. Review the table lock state and the table status

**Test Result(s):**
1. The removed table is unlocked immediately
2. The table status returns to available
3. The table can be selected by another reservation afterwards

#### 1.4.Verify Changing Outlet, Date or Period Still Unlocks the Previously Selected Table
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. Available tables exist in more than one outlet, date or period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and review the table lock state
3. Change the outlet, then the date, then the period in the reservation form
4. Review the lock state of the previously selected table after each change

**Test Result(s):**
1. The previously selected table is unlocked after the outlet is changed
2. The previously selected table is unlocked after the date is changed
3. The previously selected table is unlocked after the period is changed
4. The newly selected table is locked correctly

#### 1.5.Verify Switching to Waitlist Still Unlocks the Selected Table
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. At least one available table exists in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table
3. Switch the reservation to Waitlist
4. Review the table lock state and the table status

**Test Result(s):**
1. The selected table is unlocked when the reservation is switched to Waitlist
2. The table status returns to available
3. The waitlist reservation is saved without a table assignment

#### 1.6.Verify Clearing the Form Still Unlocks the Table
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. At least one available table exists in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and enter reservation information
3. Clear the reservation form
4. Review the table lock state and the table status

**Test Result(s):**
1. The selected table is unlocked when the form is cleared
2. The table status returns to available
3. The reservation form is reset to the default state

#### 1.7.Verify Leaving Without Saving Still Unlocks the Table
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. At least one available table exists in the selected outlet, date and period

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and enter reservation information
3. Leave the New Reservation page without saving
4. Review the table lock state and the table status

**Test Result(s):**
1. The selected table is unlocked when the page is left without saving
2. The table status returns to available
3. No reservation record is created

#### 1.8.Verify Service Locks Are Released When the Form Resets
**Prerequisite(s):**
1. TMS 2.0 Operation is available and the user can log in
2. An available table exists and service lock is applied when the table is selected

**Step(s):**
1. Go to TMS 2.0 Operation > New Reservation
2. Select an available table and confirm the service lock is applied
3. Trigger a form reset, such as removing the table or clearing the form
4. Review the service lock state of the table

**Test Result(s):**
1. The service lock is released when the form is reset
2. The table status returns to available
3. The service lock can be applied again when the table is selected in a new reservation

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- New Reservation save and confirmation dialog close
- Save and New with consecutive reservations on the same table
- Table lock and table unlock for the reservation form
- Removing a table, changing outlet, date or period in the reservation form
- Switching a reservation to Waitlist
- Clearing the reservation form and leaving without saving
- Service lock release when the reservation form resets
- Table status display in TMS 2.0 Operation after reservation save
