# HERO-69160 Bug Fix Test Report - Membership No. Not Displayed After Search Member in Online Reservation

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Search Member in Online Reservation

#### F1.1.Verify Membership No. Is Displayed After Searching Member by Member No.
**Prerequisite(s):**
1. TMS Backend is accessible and the user can log in with an admin account for the outlet under test
2. In Table Management System > Other Settings > Config by Location, "Online Reservation using Member Verification" is enabled for the outlet
3. In the same configuration, "Auto Member Profile Matching" is enabled for the outlet
4. At least one member exists with a valid Member No. and Mobile No.
5. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Log in to TMS Backend and open the online reservation page of the outlet
2. Enter the reservation information, such as date, time, party size and table
3. Click the "Search Member" button
4. Enter a valid "Member No." of the existing member
5. Click the "Confirm" button
6. Review the member information returned on the online reservation page

**Reproduction Result(s):**
1. The member search returns without any error message
2. The "Membership No." field is displayed as empty
3. The member information related to the entered Member No. is not shown

**Fix Result(s):**
1. The member search completes successfully after clicking the "Confirm" button
2. The "Membership No." field shows the corresponding value of the searched member
3. The member information is mapped to the reservation and can be used for the subsequent booking steps

#### F1.2.Verify Membership No. Is Displayed After Searching Member by Mobile No.
**Prerequisite(s):**
1. "Online Reservation using Member Verification" and "Auto Member Profile Matching" are enabled for the outlet
2. At least one member exists with a valid Mobile No. registered
3. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet
2. Enter the reservation information and click the "Search Member" button
3. Enter the valid "Mobile No." of the existing member
4. Click the "Confirm" button
5. Review the member information returned on the online reservation page

**Reproduction Result(s):**
1. The member search returns without any error message
2. The "Membership No." field is displayed as empty
3. The member found by Mobile No. cannot be identified on the reservation page

**Fix Result(s):**
1. The member search completes successfully after clicking the "Confirm" button
2. The "Membership No." field shows the corresponding value of the member matched by Mobile No.
3. The matched member profile is displayed and applied to the reservation

#### F1.3.Verify Member Search When Auto Member Profile Matching Is Disabled
**Prerequisite(s):**
1. "Online Reservation using Member Verification" is enabled for the outlet
2. "Auto Member Profile Matching" is disabled for the outlet
3. At least one member exists with a valid Member No. and Mobile No.
4. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet
2. Enter the reservation information and click the "Search Member" button
3. Enter the valid "Member No." and "Mobile No." of the existing member
4. Click the "Confirm" button
5. Review the member information returned on the online reservation page

**Reproduction Result(s):**
1. The member search returns without any error message
2. The "Membership No." field is displayed as empty
3. The verification result cannot be linked to the searched member

**Fix Result(s):**
1. The member search completes successfully after clicking the "Confirm" button
2. The "Membership No." field shows the corresponding value of the searched member
3. The member profile is applied to the reservation only when the entered Member No. and Mobile No. belong to the same member

#### F1.4.Verify Member Search With Only Member No. or Only Mobile No. Entered
**Prerequisite(s):**
1. "Online Reservation using Member Verification" and "Auto Member Profile Matching" are enabled for the outlet
2. At least one member exists with a valid Member No. and Mobile No.
3. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet and click the "Search Member" button
2. Enter only the "Member No." and leave the "Mobile No." empty
3. Click the "Confirm" button and review the displayed membership information
4. Repeat the search by entering only the "Mobile No." and leaving the "Member No." empty
5. Click the "Confirm" button and review the displayed membership information

**Reproduction Result(s):**
1. The search is processed without validation feedback
2. The "Membership No." field is displayed as empty in both searches
3. The member cannot be identified from either the Member No. or the Mobile No.

**Fix Result(s):**
1. The member search completes successfully with the single identifier entered
2. The "Membership No." field shows the corresponding value of the matched member
3. The member profile returned is consistent with the identifier entered and no empty membership information is shown

#### F1.5.Verify Member Search With Invalid Member No. or Mobile No.
**Prerequisite(s):**
1. "Online Reservation using Member Verification" and "Auto Member Profile Matching" are enabled for the outlet
2. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet and click the "Search Member" button
2. Enter a non-existing "Member No." and a valid "Mobile No."
3. Click the "Confirm" button
4. Repeat the search by entering a valid "Member No." and a non-existing "Mobile No."
5. Click the "Confirm" button

**Reproduction Result(s):**
1. The search is processed without a clear validation message
2. The "Membership No." field is displayed as empty
3. The user cannot tell whether the member is not found or the search has failed

**Fix Result(s):**
1. The member search returns a clear message indicating that the member cannot be found
2. The "Membership No." field is not populated with incorrect or partial membership information
3. The reservation page remains usable and the user can correct the input and search again

#### F1.6.Verify Reservation Is Saved With the Verified Member
**Prerequisite(s):**
1. "Online Reservation using Member Verification" and "Auto Member Profile Matching" are enabled for the outlet
2. At least one member exists with a valid Member No. and Mobile No.
3. The online theme used for the online reservation page is configured with the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet
2. Enter the reservation information and search the member by Member No. or Mobile No.
3. Confirm the member search and verify the displayed Membership No.
4. Complete the online reservation
5. Open the created reservation in TMS Backend and review the member information linked to the reservation

**Reproduction Result(s):**
1. The Membership No. is empty after the member search
2. The created reservation is saved without the member information
3. The reservation cannot be linked to the searched member in TMS Backend

**Fix Result(s):**
1. The Membership No. is displayed after the member search
2. The reservation is saved with the verified member information
3. The Member No. and member profile shown in TMS Backend match the searched member

#### F1.7.Verify Search Member Button Is Not Available When Not Configured in the Online Theme
**Prerequisite(s):**
1. "Online Reservation using Member Verification" is enabled for the outlet
2. The online theme used for the online reservation page is configured without the "Search Member" button field

**Step(s):**
1. Open the online reservation page of the outlet
2. Enter the reservation information
3. Review the fields available on the online reservation page

**Reproduction Result(s):**
1. The member information is not displayed because the member search is not available on the page
2. No empty Membership No. field is rendered without a way to perform the search

**Fix Result(s):**
1. The "Search Member" button is not displayed when the field is not configured in the online theme
2. The online reservation can still be completed without member verification
3. No error or empty membership information is shown on the page

#### F1.8.Verify Member Search Configuration Applied Per Location
**Prerequisite(s):**
1. At least two locations exist and both are accessible from TMS Backend
2. "Online Reservation using Member Verification" and "Auto Member Profile Matching" are configured differently for the two locations in Table Management System > Other Settings > Config by Location
3. The online theme used for the online reservation page is configured with the "Search Member" button field
4. At least one member exists with a valid Member No. and Mobile No.

**Step(s):**
1. Open the online reservation page of the location where member verification is enabled and search a member
2. Verify the Membership No. displayed for the searched member
3. Open the online reservation page of the location where member verification is disabled and search a member
4. Verify the behavior of the member search for that location

**Reproduction Result(s):**
1. The Membership No. is displayed as empty for the location where member verification is enabled
2. The member search behavior is not consistent with the configuration maintained for each location

**Fix Result(s):**
1. The Membership No. is displayed correctly for the location where member verification is enabled
2. The member search follows the configuration of the location being used for the online reservation
3. The configuration of one location does not affect the member search behavior of the other location

### F2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Table Management System > Other Settings > Config by Location for "Online Reservation using Member Verification" and "Auto Member Profile Matching"
- Online theme configuration of the "Search Member" button field
- Online reservation page member search by Member No. and by Mobile No.
- Member profile display and member information mapping on the online reservation page
- Creation and update of online reservations with verified member information
- Member verification behavior across different locations
- Existing online reservation flow without member verification
