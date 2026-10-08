# HERO-69309 Test Report

## Functional Testing

### F1.Message Queue Shell Stability After Repeated Preview Guest Check API Calls

#### F1.1.Reproduce The Message Queue Shell Termination After Multiple guest_check_preview_by_check_number Requests

**Prerequisite(s):**
1. The POS API portal is deployed and accessible on the test environment
2. At least one closed guest check exists in the target outlet business day so that the `guest_check_preview_by_check_number` API can return valid data
3. A valid API caller (token / credentials) is available to invoke the API portal endpoint `guest_check_preview_by_check_number`
4. The Message Queue Shell process is started and running on the local server / cloud service before the test begins
5. Test environment: Local / HQ test environment with the legacy (pre-fix) build installed for reproduction purposes

**Step(s):**
1. Note the running Message Queue Shell process id and confirm the shell is alive (for example via the local server process list or service monitor)
2. Capture a valid check number from the target outlet business day (the same check that will be queried through the API)
3. Invoke the API portal endpoint `guest_check_preview_by_check_number` once with the valid check number and confirm a valid preview payload is returned
4. Repeat the `guest_check_preview_by_check_number` API request several times in succession (at least 5–10 consecutive calls) using the same or different valid check numbers
5. Monitor the Message Queue Shell process status during and immediately after the repeated API calls
6. Attempt one more `guest_check_preview_by_check_number` API call after the shell has been observed to terminate (if it terminates)

**Reproduction Result(s):**
1. Before the repeated API calls, the Message Queue Shell process is running normally
2. During the series of `guest_check_preview_by_check_number` calls, the Message Queue Shell terminates randomly — the process is no longer alive in the server process list / service monitor
3. Subsequent `guest_check_preview_by_check_number` API calls fail or return no preview data because the supporting Message Queue Shell is no longer running
4. The termination is intermittent and occurs after several preview guest check API requests, matching the reported defect symptom

#### F1.2.Verify The Fix Prevents Message Queue Shell Termination Under Repeated Preview Guest Check API Calls

**Prerequisite(s):**
1. The fixed POS API portal build is deployed and accessible on the test environment
2. At least one closed guest check exists in the target outlet business day so that the `guest_check_preview_by_check_number` API can return valid data
3. A valid API caller (token / credentials) is available to invoke the API portal endpoint `guest_check_preview_by_check_number`
4. The Message Queue Shell process is started and running before the test begins
5. Test environment: Local / HQ test environment with the fixed build installed

**Step(s):**
1. Note the running Message Queue Shell process id and confirm the shell is alive before the test
2. Invoke the API portal endpoint `guest_check_preview_by_check_number` once with a valid check number and confirm a valid preview payload is returned
3. Repeat the `guest_check_preview_by_check_number` API request at least 10 consecutive times using the same or different valid check numbers
4. Monitor the Message Queue Shell process status throughout and immediately after the repeated API calls
5. Invoke one additional `guest_check_preview_by_check_number` API call after the repeated sequence and confirm the response is still valid
6. (Stress check) Repeat the consecutive calls across multiple business days / outlets / check numbers to cover different data scenarios

**Fix Result(s):**
1. The Message Queue Shell process remains alive throughout the entire sequence of `guest_check_preview_by_check_number` API calls and does not terminate
2. Each `guest_check_preview_by_check_number` API call returns the expected guest check preview payload without error
3. The additional API call after the repeated sequence succeeds, confirming the Message Queue Shell is still functional
4. No errors or unexpected terminations are observed in the local server / Message Queue Shell logs after the repeated API calls

#### F1.3.Regression — Verify Preview Guest Check API Output Integrity After The Fix

**Prerequisite(s):**
1. The fixed POS API portal build is deployed and accessible on the test environment
2. Closed guest checks exist in the target outlet business day with known item, payment, service charge, tax and discount data so that the preview payload can be validated
3. A valid API caller (token / credentials) is available to invoke the API portal endpoint `guest_check_preview_by_check_number`
4. The Message Queue Shell process is running before the test begins

**Step(s):**
1. Identify a closed guest check with known item, payment, service charge, tax and discount data
2. Invoke the API portal endpoint `guest_check_preview_by_check_number` with the target check number
3. Inspect the returned preview payload fields (check number, business day, items, payments, service charges, taxes, discounts, totals, etc.)
4. Compare the returned preview payload against the source check data on the local server
5. Repeat the API call for several different check numbers covering different payment types, discount scenarios and revenue / non-revenue checks
6. Verify the Message Queue Shell remains alive after the sequence of preview API calls

**Fix Result(s):**
1. The preview payload returned by `guest_check_preview_by_check_number` matches the source check data (items, payments, service charges, taxes, discounts and totals are consistent)
2. Preview API calls for different check numbers, payment types, discount scenarios and revenue / non-revenue checks all return valid and consistent preview payloads
3. The Message Queue Shell remains alive after the sequence of preview API calls — no regression in shell stability is introduced by the fix
4. No data corruption, missing fields, or unexpected errors are observed in the preview payload compared to the pre-fix behaviour

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69309):**

- Message Queue Shell terminates randomly after executing several preview guest check API (`guest_check_preview_by_check_number`) requests.
- After the fix, the Message Queue Shell must remain alive and the `guest_check_preview_by_check_number` API must continue to return valid preview data after repeated consecutive calls.
- No regression in the preview guest check API output integrity.
