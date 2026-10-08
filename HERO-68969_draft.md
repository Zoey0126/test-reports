# HERO-68969 Test Report

## Functional Testing

### F1.Print Past Date Check — Search By Table Number Under MSSQL

#### F1.1.Reproduce The Search Failure By Table Number In MSSQL

**Prerequisite(s):**
1. The POS client is deployed on a Workstation running Windows 11 Enterprise with the legacy (pre-fix) build
2. The local server (LS) is configured to use the MSSQL database backend
3. At least one closed guest check exists in a past business day with a known table number that can be searched
4. The same closed check can also be searched successfully by check number (to confirm the data exists)
5. Test environment: Local test environment (MSSQL)

**Step(s):**
1. On the POS client, click the function "Print Past Date Check"
2. Select a target start date and end date that cover the past business day containing the test check
3. Input the known table number of the test check
4. Click the button "OK"
5. Observe the search result returned by the system
6. (Cross-check) Repeat the search using the check number of the same check

**Reproduction Result(s):**
1. The "Print Past Date Check" function opens the search panel
2. After selecting the date range, inputting the table number and clicking "OK", the system returns "No results found"
3. The same check can be searched successfully by check number — confirming the data exists and the failure is specific to the table number search
4. The behaviour matches the reported defect: under MSSQL the system is unable to show the result for "Print Past Date Check" when searching by table number

#### F1.2.Verify The Fix Returns Past Date Checks When Searching By Table Number In MSSQL

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running Windows 11 Enterprise
2. The local server (LS) is configured to use the MSSQL database backend
3. At least one closed guest check exists in a past business day with a known table number that can be searched
4. The same closed check can also be searched successfully by check number
5. Test environment: Local test environment (MSSQL) with the fixed build installed

**Step(s):**
1. On the POS client, click the function "Print Past Date Check"
2. Select the target start date and end date covering the past business day containing the test check
3. Input the known table number of the test check
4. Click the button "OK"
5. Verify the system returns the list of past date checks matching the table number
6. Select the target check from the result list and verify the past date check can be previewed / printed correctly
7. Repeat the search by table number for different table numbers covering different business days / outlets

**Fix Result(s):**
1. After inputting the table number and clicking "OK", the system returns the list of past date checks matching the table number under MSSQL — the search by table number now works
2. The target check appears in the result list and can be previewed / printed correctly
3. Searches by different table numbers across different business days / outlets all return the correct list of past date checks
4. No "No results found" errors are observed when valid table numbers are searched

#### F1.3.Regression — Verify Search By Check Number And Other Search Criteria Are Preserved In MSSQL

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running Windows 11 Enterprise
2. The local server (LS) is configured to use the MSSQL database backend
3. Closed guest checks exist in past business days with known check numbers, table numbers and other searchable criteria (e.g. business date, outlet)
4. Test environment: Local test environment (MSSQL) with the fixed build installed

**Step(s):**
1. On the POS client, click the function "Print Past Date Check"
2. Search past date checks by check number and verify the result list is returned correctly
3. Search past date checks by table number and verify the result list is returned correctly (covered in F1.2)
4. Search past date checks by other supported criteria (e.g. business date range, outlet) and verify the result lists are returned correctly
5. Cross-check the Past Day Check Listing By Table (POS0012) report on the local server to validate the table-number search results are consistent with the report data

**Fix Result(s):**
1. The search by check number continues to work correctly under MSSQL after the fix — no regression is introduced
2. The search by table number works correctly under MSSQL after the fix
3. Searches by other supported criteria (business date range, outlet) continue to work correctly under MSSQL
4. The table-number search results are consistent with the Past Day Check Listing By Table (POS0012) report data on the local server — no data discrepancy is observed

#### F1.4.Verify The Fix Does Not Impact The Same Flow Under MySQL / Other Supported DB Backends

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running Windows 11 Enterprise
2. The local server (LS) is configured to use the MySQL (or other supported non-MSSQL) database backend
3. Closed guest checks exist in past business days with known check numbers and table numbers
4. Test environment: Local test environment (MySQL / other supported DB)

**Step(s):**
1. On the POS client, click the function "Print Past Date Check"
2. Search past date checks by table number under the MySQL / other supported DB backend
3. Verify the result list is returned correctly
4. Search past date checks by check number under the same DB backend
5. Verify the result list is returned correctly

**Fix Result(s):**
1. The search by table number works correctly under MySQL / other supported DB backends after the fix — no regression is introduced
2. The search by check number continues to work correctly under MySQL / other supported DB backends after the fix
3. The fix is specific to the MSSQL behaviour and does not impact the same flow on other supported DB backends

## Test Environment Information

- Local test environment (MSSQL)
- Local test environment (MySQL / other supported DB)

## Appendix

**JIRA Acceptance Criteria (HERO-68969):**

- Pre-condition: MSSQL environment.
- Workflow: 1) Click function "Print Past Date Check"; 2) Select target start and end date; 3) Input table number; 4) Click button "OK"; Actual result: No results found. But the related check can be searched through the check number.
- Expected result: System able to search the list of past date checks when search by table number under MSSQL environment.
- After the fix, the search by table number must work under MSSQL, and the search by check number / other criteria must continue to work without regression on all supported DB backends.
