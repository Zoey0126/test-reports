# HERO-70007 Test Report - API Portal Mark/Unmark Delivery on Workstation-Locked Check

## Background

It is available for API portal. When the target check is locked by a workstation, the second request of mark delivery and unmark delivery is done successfully even though the first request correctly returns an error.

Workflow for mark delivery: login to a workstation, open an old check with some items (e.g. table 123), fire a mark delivery API portal request for table 123 which is locked by the station, and the system returns an error message that the table is locked by station xxxx. When the same mark delivery request is fired again, the request is being handled and returns success, while the item status in the POS client does not change. The same behaviour occurs for unmark delivery on a check with delivered status items. The expected result is that both requests should return an error.

## Functional Testing

### 1. Mark Delivery via API Portal on a Workstation-Locked Check

#### 1.1. Verify the First Mark Delivery Request on a Locked Check Returns an Error

**Prerequisite(s):**
1. POS client is installed and a normal workstation is available
2. API portal is available and the tester can fire a mark delivery request for a target table
3. An old check with some items exists (e.g. table 123)

**Step(s):**
1. Login POS client on the normal workstation
2. Open the old check (e.g. table 123) on the workstation so that the check is locked by this station
3. Fire a mark delivery API portal request for table 123
4. Check the response of the request

**Reproduction Result(s):**
1. Before the fix, the first request is rejected and the system returns an error message showing that the table was locked by another station (this part behaves correctly)

**Fix Result(s):**
1. The first request is rejected and the system returns an error message showing that the table was locked by another station
2. The request is not proceeded and the item status in the POS client does not change

#### 1.2. Verify the Repeated Mark Delivery Request on a Locked Check Returns an Error

**Prerequisite(s):**
1. POS client is installed and a normal workstation is available
2. API portal is available and the tester can fire a mark delivery request for a target table
3. An old check with some items exists (e.g. table 123)

**Step(s):**
1. Login POS client on the normal workstation
2. Open the old check (e.g. table 123) on the workstation so that the check is locked by this station
3. Fire a mark delivery API portal request for table 123
4. Confirm the system returns an error message showing that the table was locked by another station
5. Fire the same mark delivery API portal request for table 123 again
6. Check the response of the second request
7. Check the item status of the check in the POS client

**Reproduction Result(s):**
1. Before the fix, the second request is being handled and returns success without any error message, even though the check is still locked by the workstation
2. The item status in the POS client does not change, causing inconsistency between the API portal result and the POS client

**Fix Result(s):**
1. The second request is not proceeded
2. The system returns an error message showing that the table was locked by another station
3. Both the first and the second requests return an error
4. The item status in the POS client does not change

### 2. Unmark Delivery via API Portal on a Workstation-Locked Check

#### 2.1. Verify the First Unmark Delivery Request on a Locked Check Returns an Error

**Prerequisite(s):**
1. POS client is installed and a normal workstation is available
2. API portal is available and the tester can fire an unmark delivery request for a target table
3. An old check with some delivered status items exists (e.g. table 123)

**Step(s):**
1. Login POS client on the normal workstation
2. Open the old check with delivered status items (e.g. table 123) on the workstation so that the check is locked by this station
3. Fire an unmark delivery API portal request for table 123
4. Check the response of the request

**Reproduction Result(s):**
1. Before the fix, the first request is rejected and the system returns an error message showing that the table was locked by another station (this part behaves correctly)

**Fix Result(s):**
1. The first request is rejected and the system returns an error message showing that the table was locked by another station
2. The request is not proceeded and the delivered status of the items in the POS client does not change

#### 2.2. Verify the Repeated Unmark Delivery Request on a Locked Check Returns an Error

**Prerequisite(s):**
1. POS client is installed and a normal workstation is available
2. API portal is available and the tester can fire an unmark delivery request for a target table
3. An old check with some delivered status items exists (e.g. table 123)

**Step(s):**
1. Login POS client on the normal workstation
2. Open the old check with delivered status items (e.g. table 123) on the workstation so that the check is locked by this station
3. Fire an unmark delivery API portal request for table 123
4. Confirm the system returns an error message showing that the table was locked by another station
5. Fire the same unmark delivery API portal request for table 123 again
6. Check the response of the second request
7. Check the delivered status of the items in the POS client

**Reproduction Result(s):**
1. Before the fix, the second request is being handled and returns success without any error message, even though the check is still locked by the workstation
2. The item status in the POS client does not change, causing inconsistency between the API portal result and the POS client

**Fix Result(s):**
1. The second request is not proceeded
2. The system returns an error message showing that the table was locked by another station
3. Both the first and the second requests return an error
4. The delivered status of the items in the POS client does not change

### 3. Regression - API Portal Delivery Requests and POS Check Locking

#### 3.1. Verify Mark and Unmark Delivery Requests Succeed on an Unlocked Check

**Prerequisite(s):**
1. POS client is installed and a normal workstation is available
2. API portal is available and the tester can fire mark delivery and unmark delivery requests
3. An old check with some items exists and the check is NOT opened or locked by any workstation

**Step(s):**
1. Confirm the target check is not opened on any workstation
2. Fire a mark delivery API portal request for the target check
3. Check the response of the request and the item status of the check in the POS client
4. Fire an unmark delivery API portal request for the same check
5. Check the response of the request and the item status of the check in the POS client

**Reproduction Result(s):**
1. Before the fix, mark delivery and unmark delivery requests on an unlocked check are handled successfully (the bug is not triggered in this scenario)

**Fix Result(s):**
1. The mark delivery request on the unlocked check succeeds and the item status is updated accordingly
2. The unmark delivery request on the unlocked check succeeds and the item delivered status is updated accordingly
3. The normal mark/unmark delivery flow via API portal behaves exactly as before the fix

#### 3.2. Verify POS Workstation Check Locking Behaviour Is Not Affected

**Prerequisite(s):**
1. POS client is installed and two normal workstations are available
2. An old check with some items exists (e.g. table 123)

**Step(s):**
1. Login POS client on workstation A and open the old check (e.g. table 123)
2. Login POS client on workstation B and try to open the same check
3. Check the behaviour on workstation B
4. Close the check on workstation A and open the same check on workstation B

**Reproduction Result(s):**
1. Before the fix, the check locking behaviour between workstations works normally (the bug only affects the API portal mark/unmark delivery requests)

**Fix Result(s):**
1. Workstation B cannot open the check while it is locked by workstation A, and the related prompt behaves as before
2. After the check is closed on workstation A, workstation B can open the check normally
3. The POS check locking behaviour is exactly the same as before the fix

### 4. Regression Scope

The following related modules and functions must be retested after the fix:

- API portal mark delivery request, for checks locked by a workstation and for unlocked checks
- API portal unmark delivery request, for checks locked by a workstation and for unlocked checks
- Repeated/consecutive API portal requests for the same target check
- Error message returned by API portal when the target check is locked by another station
- Item status and delivered status display in the POS client after API portal mark/unmark delivery requests
- POS client check locking and unlocking behaviour between workstations

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-70007 |
| Summary | API Portal - The second request of mark delivery and unmark delivery done successfully even if target check locked by a workstation |
| Sprint | POS - 20261019-20261030 |
| Template | infrasys |
| Issue Type | Bug |
| QC Tester | Kiet Zou (kiet.zou@shijigroup.com) |
| QC Reviewer | Alisa Chen (alisa.chen@shijigroup.com) |
| Test Target | API Portal - Mark delivery / Unmark delivery requests on workstation-locked checks |
| Client Environment | Infrasys POS client on normal workstation |
| API Channel | API Portal (mark delivery, unmark delivery) |
| New Setup | None |
| Verification Points | Error message for locked check on both first and repeated requests; item status in POS client unchanged when requests are rejected |
