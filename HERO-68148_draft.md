# HKJC - TMS - Support setting up Reservation Listing Columns by User in frontend - Dev - 2/2 Test Report

## Functional Testing

### 1.Custom Header Normal Flow

#### 1.1.Reorder Columns And Save In Pending Tab
**Prerequisite(s):**
1. The HKJC operation environment is accessible
2. A testing user account is available
3. The Reservation List is accessible in Current View -> Pending

**Step(s):**
1. Go to HKJC operation and click the Settings (gear) menu
2. Select "Custom Header"
3. Verify that the "Reservation List - Edit Columns" dialog opens
4. Drag a column (for example "First Name") to a new position
5. Click "Save"
6. Return to Current View -> Pending
7. Confirm the column order matches the change

**Test Result(s):**
1. The Settings (gear) menu opens correctly
2. The "Reservation List - Edit Columns" dialog opens when "Custom Header" is selected
3. The dragged column is moved to the new position
4. After clicking "Save", the dialog closes and the column order in Current View -> Pending matches the change

#### 1.2.Hide And Unhide Columns
**Prerequisite(s):**
1. The "Reservation List - Edit Columns" dialog is open
2. At least one column is available for hiding

**Step(s):**
1. Click the "Hide" button of a column
2. Verify that the column turns grey and shows the "Unhide" button
3. Click "Save"
4. Return to the reservation list and verify the hidden column is not displayed
5. Reopen the "Custom Header" dialog
6. Click the "Unhide" button of the same column
7. Click "Save"
8. Return to the reservation list and verify the column is displayed again

**Test Result(s):**
1. Clicking "Hide" turns the column grey and changes the button to "Unhide"
2. After saving, the hidden column is moved to the bottom of the list and is not displayed on the reservation list
3. Clicking "Unhide" restores the column to its normal state
4. After saving, the unhidden column is displayed on the reservation list again

#### 1.3.Copy Column Arrangement To Other Tabs Within The Same Set
**Prerequisite(s):**
1. The "Reservation List - Edit Columns" dialog is open on the Pending tab
2. The target tabs (Host Arrived, Guest Arrived, No Show) are part of Set 1 and allow copy

**Step(s):**
1. Reorder or hide a column in the Pending tab
2. Click the "Copy to" button
3. Keep the other status tabs selected (Host Arrived, Guest Arrived, No Show)
4. Click "Confirm"
5. Click "Save"
6. Switch to Current View -> Host Arrived
7. Verify that the same column set appears
8. Switch to Current View -> Guest Arrived
9. Verify that the same column set appears
10. Switch to Current View -> No Show
11. Verify that the same column set appears

**Test Result(s):**
1. The "Copy to" button opens the copy target selection
2. The other status tabs (Host Arrived, Guest Arrived, No Show) are selected by default within Set 1
3. After confirming and saving, the same column set appears in Host Arrived, Guest Arrived and No Show tabs
4. The Waitlist tab does not have a Copy button (Set 2 is separate)
5. The Cancelled and All tabs are a separate copy set (Set 3)

#### 1.4.Copy Function Between Search View And Member View
**Prerequisite(s):**
1. The "Custom Header" dialog is open
2. The Search tab and Member tab are available

**Step(s):**
1. Switch to the Search tab in the Custom Header dialog
2. Hide or reorder a column
3. Click "Save"
4. Open the Search view and verify the list follows the new headers
5. Reopen the Custom Header dialog
6. Switch to the Member tab
7. Hide or reorder a column
8. Click "Save"
9. Open the Member view and verify the list follows the new headers

**Test Result(s):**
1. The Search tab in Custom Header allows column hide or reorder
2. After saving, the Search view list follows the new headers
3. The Member tab in Custom Header allows column hide or reorder
4. After saving, the Member view list follows the new headers
5. The Copy function also supports copying between Search View and Member View

#### 1.5.Checkbox Edit And Duplicate Stay On The List
**Prerequisite(s):**
1. The reservation list is open in any view
2. The Custom Header dialog is open

**Step(s):**
1. Open the Custom Header dialog
2. Verify that the Checkbox, Edit and Duplicate columns are not editable in Custom Header
3. Save the dialog and return to the reservation list
4. Verify that the Checkbox, Edit and Duplicate columns remain on the list

**Test Result(s):**
1. The Checkbox, Edit and Duplicate columns cannot be edited in Custom Header
2. The Checkbox, Edit and Duplicate columns stay on the reservation list after saving the Custom Header configuration

#### 1.6.Reset To Default Per Tab
**Prerequisite(s):**
1. The Custom Header dialog is open on a tab that has been customized

**Step(s):**
1. Reopen the Custom Header dialog
2. Click "Reset to Default" on the target tab
3. Click "Save"
4. Return to the corresponding view and verify the column list

**Test Result(s):**
1. Clicking "Reset to Default" on a tab restores the original columns for that tab only
2. After saving, the list returns to the original columns for that tab only
3. Other tabs are not affected by the reset action

### 2.Current View And Calendar View Sharing

#### 2.1.Verify Current View And Calendar View Share Same Column Settings
**Prerequisite(s):**
1. A testing user account is available
2. The Current View and Calendar View are accessible

**Step(s):**
1. Open Current View -> Pending
2. Open the Custom Header dialog and reorder a column
3. Save the configuration
4. Open Current View -> Pending and verify the change
5. Open Calendar View -> Pending
6. Verify that the column set matches the change made in Current View

**Test Result(s):**
1. The column changes made in Current View are reflected in Calendar View
2. Current View and Calendar View share the same column settings

### 3.Special Fields Testing

#### 3.1.Waitlist Preference On Race Day With Waitlist Quota
**Prerequisite(s):**
1. A Race Day outlet (outletType = s) with waitlist quota control on is selected
2. The Custom Header dialog is accessible for the Waitlist tab (also test Cancelled and All)

**Step(s):**
1. Open Custom Header -> Waitlist (or Cancelled / All)
2. Keep the "Waitlist Preference" column visible
3. Click "Save"
4. Open Current View, same outlet -> Waitlist (or Cancelled / All)

**Test Result(s):**
1. The "Waitlist Preference" column is available in Custom Header for Waitlist, Cancelled and All tabs on a Race Day outlet with waitlist quota control on
2. After saving, the "Waitlist Preference" column is displayed on the reservation list in the corresponding view

#### 3.2.Round No For Owner Box Or Horse Owner Syndicate Outlet
**Prerequisite(s):**
1. An owner-box or horse-owner syndicate outlet is selected in the admin setting
2. The Custom Header dialog is accessible for the Waitlist tab

**Step(s):**
1. Open Custom Header -> Waitlist
2. Keep the "Round No." column visible
3. Click "Save"
4. Open Current View, same outlet -> Waitlist

**Test Result(s):**
1. The "Round No." column is available in Custom Header -> Waitlist for owner-box or horse-owner syndicate outlets
2. After saving, the "Round No." column is displayed on the reservation list in Current View -> Waitlist

#### 3.3.Preference Column For Waitlist Cancelled And All Tabs
**Prerequisite(s):**
1. Any outlet (7-Day or Race Day) is selected
2. The Custom Header dialog is accessible for the Waitlist tab (also Cancelled and All)

**Step(s):**
1. Open Custom Header -> Waitlist (also test Cancelled and All)
2. Keep the "Preference" column visible
3. Click "Save"
4. Open Current View -> Waitlist (or Cancelled / All)

**Test Result(s):**
1. The "Preference" column is available in Custom Header for Waitlist, Cancelled and All tabs
2. After saving, the "Preference" (Table Size Wait For) column is displayed on the reservation list in the corresponding view
3. The Race Day setting is irrelevant for the Preference column

#### 3.4.Priority Column In Waitlist Pool
**Prerequisite(s):**
1. Any outlet is selected
2. The Custom Header dialog is accessible for the Waitlist tab

**Step(s):**
1. Open Custom Header -> Waitlist
2. Keep the "Priority" column visible
3. Click "Save"
4. Open Current View -> Waitlist

**Test Result(s):**
1. The "Priority" column is available in Custom Header -> Waitlist (not marked with *)
2. The "Priority" column is not available in other Current/Calendar tabs because it only exists in the Waitlist pool
3. After saving, the "Priority" column is displayed on the reservation list in Current View -> Waitlist

#### 3.5.PDR Table Priority Booking Table And Disallow App Cancel Amend
**Prerequisite(s):**
1. Any outlet is selected
2. The Custom Header dialog is accessible for Pending (also Cancelled or All) and Search and Member tabs

**Step(s):**
1. Open Custom Header -> Pending (also test Cancelled or All)
2. Keep all three columns "PDR Table", "Priority Booking Table" and "Disallow App Cancel/Amend" visible
3. Click "Save"
4. Open Current View -> Pending (or Cancelled / All)

**Test Result(s):**
1. The "PDR Table", "Priority Booking Table" and "Disallow App Cancel/Amend" columns are not in the Waitlist pool but are available in Pending, Host, Guest, No Show, Cancelled, All and Search and Member tabs
2. The columns are not marked with * in those tabs
3. After saving, the columns are displayed on the reservation list in the corresponding view
4. On Race Day the cell may show "No" but the column still appears

### 4.Asterisk Marked Columns And Save Validation

#### 4.1.Save With At Least One Non Asterisk Column Visible
**Prerequisite(s):**
1. The Custom Header dialog is open on any tab
2. At least one column without an asterisk (*) is visible

**Step(s):**
1. Ensure at least one header that is not marked with an asterisk (*) is shown (not hidden)
2. Click "Save"

**Test Result(s):**
1. The header list is saved successfully
2. No alert popup is displayed
3. The reservation list displays the saved columns

#### 4.2.Save With All Non Asterisk Columns Hidden
**Prerequisite(s):**
1. The Custom Header dialog is open on any tab
2. All columns without an asterisk (*) are hidden

**Step(s):**
1. Hide all headers that are not marked with an asterisk (*)
2. Click "Save"

**Test Result(s):**
1. The system shows an alert popup with the message "Please select to show at least 1 column."
2. The save action is blocked until at least one non-asterisk column is shown

### 5.Per User Settings Isolation

#### 5.1.Verify Settings Are Per User
**Prerequisite(s):**
1. Two testing user accounts are available
2. Both users have access to the HKJC operation environment

**Step(s):**
1. Sign in as User A
2. Open Custom Header and reorder or hide a column
3. Save the configuration
4. Sign out from User A
5. Sign in as User B
6. Open the same reservation view and verify the column set
7. Verify that User B still sees the original columns

**Test Result(s):**
1. The column configuration saved by User A is applied only to User A
2. User B sees the original columns and is not affected by User A's configuration
3. The settings are isolated per user account

### 6.Regression Scope

The following related modules and scenarios must be retested after the change:

- Custom Header dialog opens correctly in Current, Calendar, Search and Member Views
- Column reorder, hide and unhide operations in all tabs
- Copy function within Set 1 (Pending, Host Arrived, Guest Arrived, No Show)
- Copy function within Set 3 (Cancelled, All)
- Copy function between Search View and Member View
- Reset to Default per tab
- Special fields (Waitlist Preference, Round No., Preference, Priority, PDR Table, Priority Booking Table, Disallow App Cancel/Amend)
- Asterisk marked columns and save validation
- Per user settings isolation
- Checkbox, Edit and Duplicate columns remain on the list and are not editable in Custom Header
