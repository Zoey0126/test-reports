# [Tech] TMS2.0 - TMS - Fix error.log with Undefined array key 0 for TMS Module - V113 Test Report

## Functional Testing

### 1.Error Reproduction - Concurrent Reservation Operations

#### 1.1.Two-page concurrent operation triggers Undefined array key 0 warning

**Prerequisite(s):**
1. The TMS 2.0 environment with the unfixed build of Plugin/Tms/Controller/Component/TmsApiReservationComponent.php is available.
2. The TMS Operation > New Reservation page is reachable.
3. Access to the server error.log is available.
4. A test reservation with a table number assigned is prepared or can be created.

**Step(s):**
1. Go to TMS Operation > New Reservation page and create a reservation with a table number assigned.
2. Open two identical reservation edit pages (Page A and Page B) for the same reservation.
3. On Page A, clear the "table no." field and save the reservation.
4. On Page B, right-click the same reservation and execute the "Mark Arrived" action simultaneously with the save on Page A.
5. Observe the behavior of Page B after the arrived action completes.
6. Open error.log and search for "Undefined array key 0" entries pointing to TmsApiReservationComponent.php, line 4532.

**Reproduction Result(s):**
1. Page B performs the arrived action but triggers a PHP warning during processing because the table number array key 0 is no longer present after Page A cleared it.
2. error.log contains the warning "Undefined array key 0" at ../Plugin/Tms/Controller/Component/TmsApiReservationComponent.php, line 4532.

**Fix Result(s):**
1. Page B handles the concurrent table-no. change gracefully and prompts a clear error message to the user instead of triggering an undefined array key warning.
2. No "Undefined array key 0" warning is written to error.log at line 4532 of TmsApiReservationComponent.php.

#### 1.2.Reservation state remains consistent after concurrent table-no. clear and arrived

**Prerequisite(s):**
1. The TMS 2.0 environment with the unfixed build is available.
2. A test reservation with a table number assigned exists.
3. Two identical reservation edit pages (Page A and Page B) are open for the same reservation.

**Step(s):**
1. On Page A, clear the "table no." field and save the reservation.
2. On Page B, execute the "Mark Arrived" action at the same time.
3. Reload both pages and verify the reservation's current table number and status.

**Reproduction Result(s):**
1. The reservation's table number and arrived status become inconsistent between Page A and Page B, and the warning is logged to error.log during the concurrent save.

**Fix Result(s):**
1. The reservation state (table number and arrived status) stays consistent after the concurrent operations, and the user is correctly informed of the conflict through a visible error prompt on Page B.

### 2.Fix Verification - No Error After Concurrent Operations

#### 2.1.Page B shows a clear error prompt after concurrent table-no. clear and arrived

**Prerequisite(s):**
1. The build with the fix for TmsApiReservationComponent.php is deployed to the TMS 2.0 test environment.
2. A test reservation with a table number assigned is prepared.
3. Two identical reservation edit pages (Page A and Page B) are open for the same reservation.

**Step(s):**
1. On Page A, clear the "table no." field and save the reservation.
2. On Page B, right-click the reservation and execute the "Mark Arrived" action simultaneously.
3. Observe the prompt shown on Page B.
4. Verify the reservation's table number and arrived status on both pages after reload.

**Reproduction Result(s):**
1. Before the fix, Page B triggered the "Undefined array key 0" warning at TmsApiReservationComponent.php line 4532 and did not show a user-friendly error.

**Fix Result(s):**
1. After the fix, Page B prompts a clear error indicating the reservation cannot be marked arrived due to the concurrent table-no. change, instead of producing a PHP warning.
2. The reservation's table number and arrived status remain consistent on both pages after reload.

#### 2.2.Arrived action works normally when there is no concurrent table-no. change

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. A test reservation with a table number assigned is prepared.
3. A single reservation edit page is open (no concurrent operations).

**Step(s):**
1. Open the reservation on a single page.
2. Right-click the reservation and execute the "Mark Arrived" action.
3. Verify the reservation status changes to Arrived.
4. Check error.log for any "Undefined array key 0" entries.

**Reproduction Result(s):**
1. Before the fix, the arrived action completed but, in edge cases where the table array was empty, the warning could still be triggered at line 4532.

**Fix Result(s):**
1. The arrived action completes successfully and the reservation status is updated to Arrived.
2. No "Undefined array key 0" warning is written to error.log.

### 3.Error Log Verification - Check error.log After Operations

#### 3.1.error.log is clean of Undefined array key 0 warnings after the concurrent scenario

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. Access to error.log on the server is available.
3. A test reservation with a table number assigned is prepared.
4. Two identical reservation edit pages (Page A and Page B) are open for the same reservation.

**Step(s):**
1. On Page A, clear the "table no." field and save the reservation.
2. On Page B, execute the "Mark Arrived" action at the same time.
3. Open error.log and search for "Undefined array key 0" entries produced during the operation.
4. Confirm the location of any matching entries.

**Reproduction Result(s):**
1. Before the fix, error.log contained the warning "Undefined array key 0" at ../Plugin/Tms/Controller/Component/TmsApiReservationComponent.php, line 4532 after the concurrent operation.

**Fix Result(s):**
1. No "Undefined array key 0" warning entries are written to error.log after the concurrent operation.
2. The error.log remains clean for the TmsApiReservationComponent.php code path.

#### 3.2.error.log is clean after a full TMS reservation regression run

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. Access to error.log on the server is available.
3. The TMS reservation regression flows are prepared (create, update, clear table no., mark arrived).

**Step(s):**
1. Run the TMS reservation regression flows, including reservation creation, table number assignment, table-no. clearing, and mark arrived actions.
2. Open error.log and search for "Undefined array key 0" entries produced during the regression run.
3. Confirm that no entries point to TmsApiReservationComponent.php line 4532.

**Reproduction Result(s):**
1. Before the fix, warning entries pointing to TmsApiReservationComponent.php line 4532 appeared in error.log during the regression run, especially in concurrent scenarios.

**Fix Result(s):**
1. No new "Undefined array key 0" occurrences appear in error.log after the regression run.
2. The reservation flows complete without introducing any new warnings or notices in error.log.

### 4.Edge Cases - Other Concurrent Operations on Same Reservation

#### 4.1.Concurrent table-no. change and other reservation status updates

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. A test reservation with a table number assigned is prepared.
3. Two identical reservation edit pages (Page A and Page B) are open for the same reservation.

**Step(s):**
1. On Page A, clear the "table no." field and save the reservation.
2. On Page B, perform another reservation status update (e.g., cancel or no-show) at the same time.
3. Observe the prompt on Page B.
4. Check error.log for "Undefined array key 0" entries.

**Reproduction Result(s):**
1. Before the fix, the concurrent status update on Page B could trigger the "Undefined array key 0" warning at TmsApiReservationComponent.php line 4532 when the table array key was missing.

**Fix Result(s):**
1. The concurrent status update is handled gracefully and Page B shows a clear error or proceeds correctly based on the reservation state.
2. No "Undefined array key 0" warning is written to error.log.

#### 4.2.Concurrent arrived action with table-no. reassignment instead of clearing

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. A test reservation with a table number assigned is prepared.
3. Two identical reservation edit pages (Page A and Page B) are open for the same reservation.
4. A second available table number exists for reassignment.

**Step(s):**
1. On Page A, reassign the "table no." field to a different table number and save the reservation.
2. On Page B, execute the "Mark Arrived" action at the same time.
3. Observe the prompt on Page B.
4. Verify the reservation's final table number and arrived status on both pages after reload.
5. Check error.log for "Undefined array key 0" entries.

**Reproduction Result(s):**
1. Before the fix, the concurrent table reassignment and arrived action could leave the reservation in an inconsistent state and trigger the "Undefined array key 0" warning at line 4532.

**Fix Result(s):**
1. The concurrent reassignment and arrived action is handled gracefully with a clear error prompt on Page B when there is a conflict.
2. The reservation's final table number and arrived status remain consistent on both pages after reload.
3. No "Undefined array key 0" warning is written to error.log.

#### 4.3.Sequential (non-concurrent) table-no. clear and arrived action

**Prerequisite(s):**
1. The build with the fix is deployed to the TMS 2.0 test environment.
2. A test reservation with a table number assigned is prepared.
3. A single reservation edit page is open.

**Step(s):**
1. On the single page, clear the "table no." field and save the reservation.
2. After the save completes, right-click the reservation and execute the "Mark Arrived" action.
3. Observe the reservation status and table number.
4. Check error.log for "Undefined array key 0" entries.

**Reproduction Result(s):**
1. Before the fix, in some cases where the table array was empty after the clear, the sequential arrived action could still trigger the "Undefined array key 0" warning at line 4532.

**Fix Result(s):**
1. The sequential table-no. clear and arrived action completes without triggering the warning.
2. The reservation status is updated correctly based on business rules.
3. No "Undefined array key 0" warning is written to error.log.
