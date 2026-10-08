# HERO-68375 Test Report

## Functional Testing

### F1.Reproduce Crash When Inputting Invalid Pattern in Search by Check No.

#### F1.1.Reproduce Crash on Search by Check No. with Invalid Pattern

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. At least one check exists in the system so the Check Listing function returns data
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. In one of the Check Listing tabs, click the "Search by Check No." button
6. Input an invalid pattern in the search criteria (e.g. special characters, extremely long string, alphabetic string where numeric is expected, blank spaces only, or other malformed patterns)
7. Trigger the search action

**Reproduction Result(s):**
1. After submitting the invalid pattern, the system throws an unhandled error
2. The POS application crashes after step 6/7
3. The user is forced to relaunch POS to continue working
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, inputting the same invalid patterns no longer causes a crash
2. The system handles the invalid input gracefully with a friendly validation message or returns an empty result set as appropriate
3. POS continues to operate normally without being forced to relaunch
4. Valid Check No. patterns still return matching checks correctly after the fix

#### F1.2.Reproduce Crash on Search by Member No. with Invalid Pattern

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. At least one check with a member number exists in the system
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. In one of the Check Listing tabs, click the "Search by Member No." button
6. Input an invalid pattern in the search criteria (e.g. special characters, extremely long string, malformed patterns)
7. Trigger the search action

**Reproduction Result(s):**
1. After submitting the invalid pattern, the system throws an unhandled error
2. The POS application crashes after step 6/7
3. The user is forced to relaunch POS to continue working
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, inputting the same invalid patterns no longer causes a crash
2. The system handles the invalid input gracefully
3. POS continues to operate normally without being forced to relaunch
4. Valid Member No. patterns still return matching checks correctly after the fix

#### F1.3.Reproduce Crash on Search by Room No. with Invalid Pattern

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. At least one check associated with a room number exists in the system
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. In one of the Check Listing tabs, click the "Search by Room No." button
6. Input an invalid pattern in the search criteria (e.g. special characters, extremely long string, malformed patterns)
7. Trigger the search action

**Reproduction Result(s):**
1. After submitting the invalid pattern, the system throws an unhandled error
2. The POS application crashes after step 6/7
3. The user is forced to relaunch POS to continue working
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, inputting the same invalid patterns no longer causes a crash
2. The system handles the invalid input gracefully
3. POS continues to operate normally without being forced to relaunch
4. Valid Room No. patterns still return matching checks correctly after the fix

#### F1.4.Reproduce Crash on Search by Table No. with Invalid Pattern

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. At least one check associated with a table number exists in the system
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. In one of the Check Listing tabs, click the "Search by Table No." button
6. Input an invalid pattern in the search criteria (e.g. special characters, extremely long string, malformed patterns)
7. Trigger the search action

**Reproduction Result(s):**
1. After submitting the invalid pattern, the system throws an unhandled error
2. The POS application crashes after step 6/7
3. The user is forced to relaunch POS to continue working
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, inputting the same invalid patterns no longer causes a crash
2. The system handles the invalid input gracefully
3. POS continues to operate normally without being forced to relaunch
4. Valid Table No. patterns still return matching checks correctly after the fix

#### F1.5.Reproduce Crash on Search by Criteria with Invalid Pattern

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. At least one check exists in the system so the Check Listing function returns data
4. The previous (unfixed) POS build is available for reproducing the original defect

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. In one of the Check Listing tabs, click the "Search by Criteria" button
6. Input an invalid pattern in the search criteria (e.g. special characters, extremely long string, malformed patterns, unsupported operators)
7. Trigger the search action

**Reproduction Result(s):**
1. After submitting the invalid pattern, the system throws an unhandled error
2. The POS application crashes after step 6/7
3. The user is forced to relaunch POS to continue working
4. The defect is reproducible on the previous (unfixed) build

**Fix Result(s):**
1. After applying the fix, inputting the same invalid patterns no longer causes a crash
2. The system handles the invalid input gracefully
3. POS continues to operate normally without being forced to relaunch
4. Valid Criteria patterns still return matching checks correctly after the fix

### F2.Verify Valid Search Patterns Continue to Work After Fix

#### F2.1.Verify All Valid Search Patterns Return Correct Results After Fix

**Prerequisite(s):**
1. POS Client with the fix applied is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. Multiple checks exist in the system covering valid Check No., Member No., Room No., Table No. and Criteria patterns

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Click "Search by Check No." and input a valid Check No. pattern, trigger the search and observe results
6. Click "Search by Member No." and input a valid Member No. pattern, trigger the search and observe results
7. Click "Search by Room No." and input a valid Room No. pattern, trigger the search and observe results
8. Click "Search by Table No." and input a valid Table No. pattern, trigger the search and observe results
9. Click "Search by Criteria" and input a valid Criteria pattern, trigger the search and observe results

**Fix Result(s):**
1. The "Search by Check No." function returns the matching check(s) for the valid Check No. pattern
2. The "Search by Member No." function returns the matching check(s) for the valid Member No. pattern
3. The "Search by Room No." function returns the matching check(s) for the valid Room No. pattern
4. The "Search by Table No." function returns the matching check(s) for the valid Table No. pattern
5. The "Search by Criteria" function returns the matching check(s) for the valid Criteria pattern
6. No crashes or unhandled errors occur for valid patterns after the fix

### F3.Regression Scope - Other Check Listing Functions and Surrounding Features

#### F3.1.Verify Other Check Listing Operations Remain Functional After Fix

**Prerequisite(s):**
1. POS Client with the fix applied is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. User is able to log in to a station with valid credentials and has Admin access
3. Multiple checks exist in the system across different business days and outlets

**Step(s):**
1. Log in to a station
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Without using any search criteria, verify the default check listing loads correctly
6. Verify other actions available on the Check Listing page (e.g. open check, view details, filter by date/outlet) function correctly
7. Combine search criteria operations with other check listing actions and verify no regression is introduced

**Fix Result(s):**
1. The default Check Listing view loads correctly without crashes
2. Other actions on the Check Listing page (open check, view details, filter by date/outlet) function correctly
3. Combining search criteria operations with other actions does not introduce new crashes or errors
4. No regression in Check Listing related functionality is observed after the fix

## Test Environment Information

- **Test Environment**: Local test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS
- **Issue Type**: Bug
- **QC Tester**: Mike Lin (mike.lin@shijigroup.com)
- **QC Reviewer**: Eric Fang (eric.fang@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-68375)

Workflow to simulate the problem:

1. Login to a station
2. Click button "Admin"
3. Select tab "Check Operation"
4. Click function "Check Listing"
5. In either of the check listing tab, click button "Search by Check No." / "Search by Member No." / "Search by Room No." / "Search by Table No."
6. Input the searching criteria with invalid pattern
7. System may crash after step 6

Expected result:

- System should not crash after step 6

Fixing scope:

- Cover all kind of searching criteria in step 5
- Fix the crashing problem
- While fixing the crashing problem, all possible valid pattern should still be able to search after fixing. Need to think is it possible to gather all valid pattern of Check No, Member No., Room No., Table No.

Release Notes:

- Bug Fixing: [Jira-68375] System may encounter problem if inputting invalid pattern in searching criteria of function "Check Listing" (Simon Li)
- System may encounter errors and even crashes when users input invalid content as searching criteria in below functions in Check Listing page: Search by Member No., Search by Check No., Search by Criteria
- Workflow for Check Listing functions:
  1. Login to a station
  2. Click button "Admin"
  3. Select tab "Check Operation"
  4. Click function "Check Listing"
  5. Click either function button "Search by Check No." / "Search by Criteria" / "Search by Member No."
  6. Input the searching criteria with invalid pattern
  7. System may crash after step 6
- Expected Result: System should allow all input patterns from users, while the system will not crash after searching
- It is fixed in current version
- Setup: None
