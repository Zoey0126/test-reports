# TMS 2.0 - Display Pending Task in Operation Mode Test Report

## Functional Testing

### 1.Pending Reservation List Page

#### 1.1.Verify Pending Reservation List Shows All Unpaid Pending Tasks
**Prerequisite(s):**
1. The TMS function pending_reservation_view exists in the function authority list
2. The logged-in user is granted with the authority of pending_reservation_view for the target outlet
3. Pending reservations with an unpaid deposit or third-party payment exist for the outlet

**Step(s):**
1. Log in to TMS Operation and select an outlet with the new authority
2. Open "Pending Reservation List" from the Operation navigator
3. Review the listed reservation records
4. Compare the list with the existing Reservation List

**Test Result(s):**
1. The dedicated "Pending Reservation List" page is available in TMS Operation Mode
2. All reservations requiring a deposit payment that has not yet been received are listed
3. Each task shows reservation information and payment information in separate columns
4. The existing Reservation List is unchanged and does not mix with the pending tasks

#### 1.2.Verify Empty State When No Pending Task Exists
**Prerequisite(s):**
1. The logged-in user is granted with pending_reservation_view authority
2. No unpaid pending reservation task exists for the outlet

**Step(s):**
1. Log in to TMS Operation and open "Pending Reservation List"
2. Review the page content
3. Review the navigator entry

**Test Result(s):**
1. The page shows an empty state with no rows
2. The navigator badge is hidden or shows zero
3. No error is shown on the page

#### 1.3.Verify Pending Task Counter Badge
**Prerequisite(s):**
1. The logged-in user is granted with pending_reservation_view authority for multiple outlets
2. Unpaid pending tasks exist for the outlets that the user is allowed to view

**Step(s):**
1. Log in to TMS Operation and review the badge of the "Pending Reservation List" navigator entry
2. Open the page and count the unpaid pending tasks
3. Create a new pending reservation through online reservation or open API
4. Return to the page and review the badge again
5. Resolve one pending task by receiving the deposit
6. Review the badge again

**Test Result(s):**
1. The badge shows the count of unpaid pending tasks for outlets the user is allowed to view
2. The badge count matches the number of records in the list
3. The badge updates automatically when a task is added
4. The badge updates automatically when a task is resolved
5. The badge is hidden or shows zero when no open pending task exists

#### 1.4.Verify Search and Filter Pending Tasks
**Prerequisite(s):**
1. Pending tasks with different outlets, statuses, payment methods, period types and dates exist
2. The user is on the Pending Reservation List page

**Step(s):**
1. Filter the list by outlet (multi-select)
2. Filter the list by status: All, Success, Fail, Cancel, Timeout and Expired
3. Filter the list by payment method and by period type
4. Filter the list by date range with create date/time mode, then with reservation date mode
5. Filter the list by guest last name, first name, phone (with type checkboxes) and email
6. Apply a combination of parameters that matches no record

**Test Result(s):**
1. The result filters by standard reservation fields and payment-specific fields including payment status, payment method and deposit amount
2. The default list view shows unpaid tasks only with Pending status
3. The status filter switches the list to All, Success, Fail, Cancel, Timeout or Expired correctly
4. The date range filter works for both create date/time mode and reservation date mode
5. The guest filters return the matching records
6. An empty result set is shown when no record matches the search parameters

#### 1.5.Verify Sorting of Pending Tasks
**Prerequisite(s):**
1. Multiple pending tasks exist with different create times, reservation dates and deposit amounts
2. The user is on the Pending Reservation List page

**Step(s):**
1. Review the default sort order of the list
2. Click each displayed column header to sort the list
3. Sort by reservation fields and by payment fields
4. Switch the sort direction

**Test Result(s):**
1. The default sort order is create time descending
2. The list can be sorted by all displayed columns, including reservation and payment fields
3. The sort direction switches correctly between ascending and descending

#### 1.6.Verify Void or Refund Permission by Role
**Prerequisite(s):**
1. A manager or supervisor account and an operator account are available
2. Both accounts are granted with pending_reservation_view authority
3. An unpaid pending task exists

**Step(s):**
1. Log in as an operator and open the Pending Reservation List
2. Open a pending task detail and verify the availability of the void and refund buttons
3. Log in as a manager or supervisor and open the same pending task
4. Click the void or refund action and confirm the operation
5. Review the task in the list after the operation

**Test Result(s):**
1. All operators can view the Pending Reservation List page
2. The void and refund buttons are not available for the operator account
3. The void or refund action completes for the manager or supervisor account
4. The resolved task moves to the completed section of the same page

#### 1.7.Verify Cancel Task and Partial Batch Result
**Prerequisite(s):**
1. A manager or supervisor account with pending_reservation_view authority is available
2. At least two pending tasks exist, and one task is set to fail on cancel

**Step(s):**
1. Open the Pending Reservation List and select multiple pending tasks
2. Click the cancel action and confirm in the Cancel Confirm dialog
3. Review the result of the batch cancel
4. Trigger a cancel failure for a single task
5. Review the error message

**Test Result(s):**
1. The Cancel Confirm dialog is displayed before the cancel is performed
2. The successfully cancelled tasks move to the completed section
3. A partial batch success shows the cancel_partial_success_summary with the failure reasons
4. When the cancel fails, an Error dialog is displayed with the failure reason

#### 1.8.Verify Resolved Tasks Move to Completed Section
**Prerequisite(s):**
1. An unpaid pending task exists
2. The deposit payment can be received through the payment gateway

**Step(s):**
1. Open the Pending Reservation List and locate the unpaid task
2. Complete the deposit payment so the task is resolved
3. Review the pending list and the completed section
4. Open the completed task for audit review

**Test Result(s):**
1. The resolved task moves from the pending list to the completed section within the same page
2. The completed task remains visible for audit purposes
3. The reservation and payment information of the completed task is displayed correctly

#### 1.9.Verify Outlet Scope and Function Authority Enforcement
**Prerequisite(s):**
1. Two outlets exist and the user is granted with pending_reservation_view for only one outlet
2. Pending tasks exist in both outlets

**Step(s):**
1. Log in to TMS Operation with the user
2. Open the Pending Reservation List and review the listed outlets
3. Attempt to open the list, count, detail and cancel API with the outlet that is not granted
4. Remove the pending_reservation_view authority and log in again

**Test Result(s):**
1. Only the pending tasks of the granted outlet are listed
2. The list, count, detail and cancel APIs enforce the outlet scope for the logged-in user
3. The navigator entry is not available when the user has no pending_reservation_view authority
4. No data of the non-granted outlet is returned

#### 1.10.Verify List Refresh Keeps Row Selection
**Prerequisite(s):**
1. The WebSocket event ReservationPendingTaskUpdate is configured and enabled
2. Multiple pending tasks exist and row checkboxes are displayed

**Step(s):**
1. Open the Pending Reservation List and select several rows with the checkbox
2. Trigger a list refresh by the WebSocket event
3. Review the row checkbox selection after the refresh

**Test Result(s):**
1. The list refreshes when the ReservationPendingTaskUpdate event is received
2. The row checkbox selection is preserved after the refresh where applicable
3. The refreshed list shows the latest pending task data

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Existing Reservation List and its search, sort and pagination
- Third Party Payment List workflow
- Access Right Groups and Function Authorities setup for TMS functions
- Online reservation and open API pending reservation creation
- Deposit payment, void and refund workflows
- Operation navigator entries and badges for other modules
- WebSocket event handling in TMS Operation

## Compatibility Testing

### 1.Browser Compatibility For TMS Operation
#### 1.1.Verify Pending Reservation List In Supported Browsers
**Prerequisite(s):**
1. TMS is deployed in the local, HQ and HKJC test environments
2. Pending tasks exist for the target outlet

**Step(s):**
1. Open the Pending Reservation List in Chrome, Firefox, Microsoft Edge and Safari
2. Apply search filters and sort the list
3. Open a task detail, then perform a cancel as a manager or supervisor
4. Review the navigator badge

**Test Result(s):**
1. The page layout, the badge and the columns are displayed correctly in all supported browsers
2. Search, sort and cancel operations work in all supported browsers
3. No console error occurs in any supported browser

## Test Environment Information

- Local test environment
- HQ test environment
- HKJC test environment
