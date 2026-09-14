# TMS 2.0 - Incorrect table number arrived after mark arrival Test Report

## Functional Testing

### 1.Mark Arrival with Concurrent Table Change (Single Table)

#### 1.1.Mark arrival after table changed in another tab via edit booking

**Prerequisite(s):**
1. Two browser tabs are open on TMS Operation for the same outlet and current date
2. A reservation for the current date exists with a single table assigned (e.g. Table 5)

**Step(s):**
1. In tab A, open Mark Arrival for the reservation and hold the dialog
2. In tab B, edit the booking of the same reservation and change the table to Table 8
3. After the table is changed, return to tab A and confirm Mark Arrival

**Reproduction Result(s):**
1. The reservation is marked arrival with the old table (Table 5) instead of the changed table (Table 8)

**Fix Result(s):**
1. Alert dialog with message "The reserved table number has been changed. Please check again." pops up
2. The Mark Arrival dialog is closed and no arrival is marked with the stale table

#### 1.2.Mark arrival after table changed via quick change

**Prerequisite(s):**
1. Two browser tabs are open on TMS Operation for the same outlet and current date
2. A reservation for the current date exists with a single table assigned (e.g. Table 3)

**Step(s):**
1. In tab A, open Mark Arrival for the reservation and hold the dialog
2. In tab B, change the table of the reservation using quick change (e.g. to Table 9)
3. Return to tab A and confirm Mark Arrival

**Reproduction Result(s):**
1. The reservation is marked arrival with the table before the change (Table 3)

**Fix Result(s):**
1. The alert message "The reserved table number has been changed. Please check again." is displayed
2. The Mark Arrival dialog closes and the arrival is not marked with the outdated table

### 2.Mark Arrival with Multiple Tables

#### 2.1.One of multiple tables changed before mark arrival

**Prerequisite(s):**
1. Two browser tabs are open on TMS Operation for the same outlet and current date
2. A reservation for the current date exists with multiple tables assigned (e.g. Table 1 and Table 2)

**Step(s):**
1. In tab A, open Mark Arrival for the reservation and hold the dialog
2. In tab B, edit the booking and change one of the assigned tables (e.g. Table 2 to Table 7)
3. Return to tab A and confirm Mark Arrival

**Reproduction Result(s):**
1. The reservation is marked arrival using the old table combination (Table 1 and Table 2)

**Fix Result(s):**
1. The alert message "The reserved table number has been changed. Please check again." is displayed and the Mark Arrival dialog closes
2. No arrival is marked with the outdated table combination

#### 2.2.Table number with table extension changed before mark arrival

**Prerequisite(s):**
1. Two browser tabs are open on TMS Operation for the same outlet and current date
2. A reservation for the current date exists with a table number plus table extension assigned

**Step(s):**
1. In tab A, open Mark Arrival for the reservation and hold the dialog
2. In tab B, change the reservation table including the table extension
3. Return to tab A and confirm Mark Arrival

**Reproduction Result(s):**
1. The reservation is marked arrival with the table number and extension before the change

**Fix Result(s):**
1. The mismatch is detected and the alert message "The reserved table number has been changed. Please check again." is displayed
2. The Mark Arrival dialog closes without marking arrival

#### 2.3.Only table extension changed before mark arrival

**Prerequisite(s):**
1. Two browser tabs are open on TMS Operation for the same outlet and current date
2. A reservation for the current date exists with only a table extension assigned

**Step(s):**
1. In tab A, open Mark Arrival for the reservation and hold the dialog
2. In tab B, change the table extension of the reservation
3. Return to tab A and confirm Mark Arrival

**Reproduction Result(s):**
1. The reservation is marked arrival with the table extension before the change

**Fix Result(s):**
1. The alert message "The reserved table number has been changed. Please check again." is displayed
2. The Mark Arrival dialog closes and the stale extension is not used for arrival

### 3.Normal Mark Arrival Flow

#### 3.1.Mark arrival without concurrent table change

**Prerequisite(s):**
1. TMS Operation is accessible and a reservation for the current date exists with a table assigned
2. No other user changes the reservation during the test

**Step(s):**
1. Open Mark Arrival for the reservation
2. Confirm Mark Arrival without any concurrent table change

**Fix Result(s):**
1. The reservation is marked arrival with the assigned table as expected
2. No unexpected alert message is displayed and the arrival is recorded correctly

### 4.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Mark arrival flow with single table and multiple tables
- Table assignment combinations: table number, table number plus table extension, only table extension
- Edit booking table change
- Quick change table
- Reservation change log after mark arrival
- Normal mark arrival without concurrent modification remains unaffected
