# HERO-68812 TMS2.0 - Display Existing Booking Time, Cover, Reservation Number in Reservation Alert Test Report

## Functional Testing

### 1.Reservation Reminder Popup Display - Existing Booking Details

#### 1.1.Popup lists each existing reservation with booking time, cover count and confirmation number

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. One outlet, one date, and one meal period are prepared.
3. Table T1 has one normal upcoming reservation (not share table, not arrived, not completed, not no-show).

**Step(s):**
1. Start a new booking and select table T1 (which has an existing upcoming reservation).
2. Observe the reservation reminder popup that appears.
3. Verify the popup lists the existing reservation with its booking time, cover count and confirmation number.
4. Verify all three values are shown together for the listed reservation.

**Test Result(s):**
1. The reservation reminder popup opens when selecting T1.
2. The popup lists the existing reservation with its booking time, cover count and confirmation number.
3. All three values (booking time, cover count, confirmation number) are shown together for the reservation.
4. The message count equals 1.

#### 1.2.Reservation with a missing or empty value is still displayed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A table has an upcoming reservation where one of the three values (booking time, cover count, or confirmation number) is missing or empty.

**Step(s):**
1. Start a new booking and select the table with the reservation that has a missing or empty value.
2. Observe the reservation reminder popup.
3. Verify the reservation is still listed with the available values.

**Test Result(s):**
1. The popup still displays the reservation even when one of the three values is missing or empty.
2. The available values are shown and the popup does not omit or error on that entry.
3. No exception or broken layout is shown for the incomplete reservation.

### 2.Multiple Existing Reservations on the Same Table

#### 2.1.Each reservation is listed separately and ordered by booking time ascending

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T2 has two or more upcoming normal reservations with different booking times (e.g., 18:00 and 20:00).

**Step(s):**
1. Start a new booking and select table T2.
2. Observe the reservation reminder popup.
3. Verify each reservation is listed separately with its own booking time, cover count and confirmation number.
4. Verify the reservations are ordered by booking time in ascending order.

**Test Result(s):**
1. The popup lists each reservation separately with its own booking time, cover count and confirmation number.
2. The message count equals the number of reservations on T2.
3. The reservations are ordered by booking time in ascending order.
4. The same reservation appears only once.

#### 2.2.Two reservations sharing the same booking time are both listed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A table has two upcoming reservations that share the same booking time.

**Step(s):**
1. Start a new booking and select the table with two reservations sharing the same booking time.
2. Observe the reservation reminder popup.
3. Verify both reservations are listed.

**Test Result(s):**
1. Both reservations sharing the same booking time are listed in the popup.
2. Neither reservation is hidden or merged with the other.
3. Each reservation shows its own booking time, cover count and confirmation number.

### 3.Only Upcoming Reservations Are Shown

#### 3.1.Past and completed reservations are not listed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T3 has only a completed or no-show reservation (no upcoming reservations).

**Step(s):**
1. Start a new booking and select table T3.
2. Observe whether the reservation reminder popup appears.

**Test Result(s):**
1. The reservation reminder popup does not open for table T3 because it only has completed or no-show reservations.
2. Past and completed reservations are not listed in the popup.
3. The booking flow proceeds without displaying a reservation reminder popup.

#### 3.2.In-progress or currently seated reservations are treated as upcoming and listed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A table has a reservation that is in progress or currently seated.

**Step(s):**
1. Start a new booking and select the table with the in-progress or currently seated reservation.
2. Observe the reservation reminder popup.

**Test Result(s):**
1. The reservation that is in progress or currently seated is treated as upcoming and is listed in the popup.
2. The reservation shows its booking time, cover count and confirmation number.

### 4.Only the Exact Table Being Booked Is Shown

#### 4.1.Reservations for combined or joined tables are not listed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Tables T4 and T5 have one reservation that books both tables together.

**Step(s):**
1. Start a new booking and select table T4.
2. Observe the reservation reminder popup.
3. Verify the table-no. cell lists both T4 and T5.
4. Verify the confirmation number appears only once.

**Test Result(s):**
1. Only reservations for the exact table being booked (T4) are listed.
2. The table-no. cell lists both T4 and T5 for the combined reservation.
3. The confirmation number appears only once for the combined reservation.
4. Reservations for other combined or joined tables are not listed.

### 5.No Existing Reservations on the Table

#### 5.1.No reservation reminder popup when the table has no upcoming reservations

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T10 is empty with no upcoming reservations.

**Step(s):**
1. Start a new booking and select table T10.
2. Observe whether the reservation reminder popup appears.

**Test Result(s):**
1. The reservation-full dialog does not open for table T10.
2. No existing booking details are shown in the reservation reminder.
3. The booking flow proceeds without displaying a reservation reminder popup.

### 6.New Booking Flow - Reminder Interaction

#### 6.1.Click "No" clears the table selection and closes the dialog

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has one upcoming reservation.

**Step(s):**
1. Start a new booking and select table T1.
2. When the reservation reminder popup appears, click "No".
3. Verify the table selection is cleared and the dialog is closed.

**Test Result(s):**
1. Clicking "No" clears the table selection.
2. The reservation reminder dialog closes.
3. The booking form returns to the table selection state.

#### 6.2.Click "Yes" keeps the table locked and allows saving

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has one upcoming reservation.

**Step(s):**
1. Start a new booking and select table T1.
2. When the reservation reminder popup appears, click "Yes".
3. Verify the table stays locked.
4. Complete and save the booking.

**Test Result(s):**
1. Clicking "Yes" keeps the table locked.
2. The booking can be saved successfully.
3. The new reservation is created for table T1.

### 7.Walk-in Flow - Reminder Interaction

#### 7.1.Walk-in on a table with existing reservations opens the reminder popup

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has one upcoming reservation.

**Step(s):**
1. Start a walk-in and select table T1.
2. Observe whether the reservation reminder popup appears.
3. Verify the message count equals the number of reservations on T1.

**Test Result(s):**
1. The reservation reminder popup opens for the walk-in on T1.
2. The message count equals the number of rows (reservations) on T1.
3. The existing reservation(s) with booking time, cover count and confirmation number are listed.

#### 7.2.Walk-in "No" resets the form and "Yes" continues the walk-in

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has one upcoming reservation.

**Step(s):**
1. Start a walk-in and select table T1.
2. When the reservation reminder popup appears, click "No".
3. Verify the walk-in form resets.
4. Select T1 again and click "Yes".
5. Verify the walk-in continues.

**Test Result(s):**
1. Clicking "No" resets the walk-in form.
2. Clicking "Yes" on the second attempt continues the walk-in.
3. The walk-in is created successfully for table T1.

### 8.Edit Reservation Flow - Reminder Interaction

#### 8.1.Adding a table with existing reservations opens the reminder popup

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T6 has an existing reservation being edited (with no table added yet, or the original table).
3. Table T1 has one other upcoming reservation.

**Step(s):**
1. Open the reservation on T6 for editing and do not add a table (its own reservation should not open the dialog).
2. Add table T1 to the reservation.
3. Observe the reservation reminder popup.
4. Verify the top table lists the other reservation on T1 and the yellow table below shows T6 (the original table).

**Test Result(s):**
1. Adding T1 opens the reservation reminder popup.
2. The top table in the popup lists the other reservation on T1.
3. The yellow table below shows T6 (the original table).
4. The existing reservation on T6 itself does not trigger the popup.

#### 8.2.Click "No" unlocks only the newly added conflicting table

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A reservation on T6 is being edited with T1 (full) and T7 (empty) added.

**Step(s):**
1. Open the edit view of the reservation on T6.
2. Add empty table T7, then add full table T1.
3. When the reservation reminder popup appears, click "No".
4. Verify which tables remain locked and which are unlocked.

**Test Result(s):**
1. Clicking "No" unlocks only T1 (the conflicting table).
2. T7 (the newly added empty table) remains locked.
3. T6 (the original table) remains locked.
4. Only the conflicting table is removed when "No" is clicked.

#### 8.3.Click "Yes" keeps both tables locked and allows saving

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A reservation on T6 is being edited with T1 added.

**Step(s):**
1. Open the edit view of the reservation on T6.
2. Add table T1.
3. When the reservation reminder popup appears, click "Yes".
4. Verify both T1 and T6 stay locked.
5. Save the reservation.

**Test Result(s):**
1. Clicking "Yes" keeps both T1 and T6 locked.
2. The reservation can be saved successfully with both tables.

### 9.Quick Edit Flow - Reminder Interaction

#### 9.1.Quick edit changing a reservation's table to a full table shows Yes/No popup

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has an upcoming reservation visible on the reservation list.
3. Another reservation exists that can be moved to T1.

**Step(s):**
1. On the reservation list, change another reservation's table to T1 while the existing reservation on T1 is visible.
2. Observe whether the Yes/No popup appears before saving.
3. Click "Yes" and verify the change is saved.
4. Repeat the process and click "No" to verify the change is cancelled.

**Test Result(s):**
1. The Yes/No popup appears before saving when changing a reservation onto T1 (which has an existing reservation).
2. Clicking "Yes" saves the table change.
3. Clicking "No" cancels the table change.
4. The same confirmation number appears only once, and the table-no. cell lists every table on that reservation.

#### 9.2.Quick edit popup appears even when the existing reservation is filtered out

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Table T1 has an upcoming reservation.
3. The reservation list can be filtered by status, search, or pagination.

**Step(s):**
1. Filter the reservation list so the reservation on T1 is hidden (using status filter, search, or another page in the search view).
2. Change another reservation onto T1.
3. Observe whether the Yes/No popup appears.

**Test Result(s):**
1. The Yes/No popup still appears even when the existing reservation on T1 is filtered out of the visible list.
2. The change does not save immediately without the popup confirmation.
3. The popup correctly shows the existing reservation details even when the reservation is not visible in the current list view.

### 10.Completed and No-Show Reservations Exclusion

#### 10.1.Completed and no-show reservations are not listed in the reminder popup

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. A table has both upcoming reservations and completed or no-show reservations.

**Step(s):**
1. Start a new booking and select the table that has both upcoming and completed/no-show reservations.
2. Observe the reservation reminder popup.
3. Verify which reservations are listed.

**Test Result(s):**
1. Only upcoming reservations for the current or future service period are listed in the popup.
2. Completed and no-show reservations are not listed in the popup.
3. The message count reflects only the upcoming reservations.
