# [Tech] TMS Remove un-used CakeLog and Normalization Log File Name - Dev Test Report

## Functional Testing

### 1.Log Output Verification After Cleanup

#### 1.1.Unused CakeLog debug entries are no longer written to logs

**Prerequisite(s):**
1. The build with the removed unused CakeLog calls is deployed to the TMS test environment.
2. Access to the TMS log directory (error.log and related debug log files).
3. A record of the previously observed unwanted debug log entries is available for comparison.

**Step(s):**
1. Log in to TMS and perform typical reservation operations such as creating a reservation, editing a reservation, and viewing the current view.
2. Open the log files in the TMS log directory after the operations.
3. Search the logs for the previously observed unwanted debug entries produced by the removed CakeLog calls.

**Test Result(s):**
1. Normal operations complete without any functional error.
2. The previously observed unwanted debug log entries no longer appear in the logs.
3. The overall volume of debug output is reduced compared with the previous build.

#### 1.2.Normalization log file name output is clean and consistent

**Prerequisite(s):**
1. The build with the normalization log file name change is deployed to the TMS test environment.
2. Access to the TMS log directory for checking generated file names.

**Step(s):**
1. Exercise the reservation flows that trigger logging (reservation creation, update, and status change).
2. Inspect the log directory and review the names of the log files generated during the runs.
3. Repeat the same flow several times to confirm the naming is stable.

**Test Result(s):**
1. Log file names follow the normalized naming convention consistently.
2. No stray, duplicated, or abnormally named log files are generated during the operations.

### 2.Core Reservation Function Regression

#### 2.1.Reservation creation and editing flows operate normally after log cleanup

**Prerequisite(s):**
1. The cleaned build is deployed to the TMS test environment.
2. A valid outlet with tables and periods is configured.
3. WebSocket connection is available for the current view.

**Step(s):**
1. Log in to TMS Operation and open the New Reservation page.
2. Create a new reservation with a valid period, table, and customer information, then confirm the booking.
3. Open the created reservation and edit details such as the number of people or the booked table, then save the changes.
4. Verify the reservation list and the current view both reflect the created and edited reservation.

**Test Result(s):**
1. The reservation is created and edited successfully without any error prompt.
2. The reservation list and current view display the correct reservation data after each operation.
3. No unexpected log-related exception blocks any step of the flows.

#### 2.2.Reservation status update and cancellation operate normally after log cleanup

**Prerequisite(s):**
1. At least one existing reservation is available in the TMS test environment.
2. The cleaned build is deployed and the user has permission to change reservation status.

**Step(s):**
1. Open an existing reservation in TMS Operation.
2. Change the reservation status (for example, mark arrival or change the seating status) and verify the update.
3. Cancel another existing reservation and verify the cancellation.
4. Check the reservation list and current view after each status change.

**Test Result(s):**
1. Status updates and cancellation complete successfully with correct prompts.
2. The reservation list and current view reflect the updated statuses correctly.

### 3.Error Handling Scenarios

#### 3.1.Genuine errors are still captured in error.log after cleanup

**Prerequisite(s):**
1. The cleaned build is deployed to the TMS test environment.
2. Access to the error.log file.

**Step(s):**
1. Trigger a genuine error scenario, for example submitting an invalid reservation request that the server rejects.
2. Open error.log and check the entries written during the scenario.

**Test Result(s):**
1. The genuine error is still captured in error.log with the correct message and location.
2. Only the intended unused debug entries are gone; required error logging remains functional.

#### 3.2.No new PHP warnings or notices introduced after log removal

**Prerequisite(s):**
1. The cleaned build is deployed to the TMS test environment.
2. Access to error.log and the full set of reservation test flows.

**Step(s):**
1. Run a full pass of the reservation regression flows, including creation, editing, status change, and cancellation.
2. Open error.log and search for new PHP warnings or notices such as undefined index, undefined variable, or array offset on null.

**Test Result(s):**
1. All reservation flows complete normally.
2. No new PHP warnings or notices are introduced by the log removal change.
