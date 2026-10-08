# HERO-66603 Test Report

## Functional Testing

### F1.Display Payment Membership Number in Paid Check Listing (Current Station)

#### F1.1.Verify "Payment Member" Column Appears and Shows Membership Number When Setup 4 = Yes

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: A payment method with Type = Member is configured under POS System > Payment Setup > Payment Methods
3. Setup 2: Member Profile (For Membership in Member Module) is configured under POS System > Ordering Setup > Config by Location with Support = Yes for the target outlet/station
4. Setup 3: A target membership interface is selected under the Member Profile setup
5. Setup 4: Check Listing Setting is configured under Config by Location with Display Membership Number in Check Listing = Yes for the target outlet/station
6. A check paid with the Member payment method (whose payment membership number is captured) exists in the system

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing (Current Station)"
6. Verify the listing UI displays a new "Payment Member" column right after the "Payment Method" column
7. Locate the row corresponding to the check paid with the Member payment method
8. Verify the membership number is displayed in the "Payment Member" column for that row

**Test Result(s):**
1. The Check Listing page (Paid Check Listing - Current Station) loads correctly
2. The new "Payment Member" column appears in the listing, positioned immediately after the "Payment Method" column
3. The membership number of the check paid with the Member payment method is displayed correctly in the new "Payment Member" column
4. The remaining columns and data in the Check Listing are unaffected by the new column

#### F1.2.Verify Empty Value in "Payment Member" Column When Setups 1, 2, or 3 Are Not Fulfilled

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 4: Display Membership Number in Check Listing = Yes is configured for the target outlet/station
3. A check paid with a payment method that is NOT Type = Member exists in the system (so Setups 1, 2, 3 are not fulfilled for that payment)

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing (Current Station)"
6. Verify the new "Payment Member" column appears after the "Payment Method" column
7. Locate a row corresponding to a check paid with a non-Member payment method
8. Verify the "Payment Member" column is empty for that row

**Test Result(s):**
1. The Check Listing page (Paid Check Listing - Current Station) loads correctly
2. The new "Payment Member" column appears in the listing
3. The "Payment Member" column is empty for checks paid with payment methods that do not fulfill Setups 1, 2, 3
4. The remaining columns and data in the Check Listing are unaffected

#### F1.3.Verify No "Payment Member" Column When Setup 4 = No or Does Not Exist

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setups 1, 2, 3 are configured for the Member payment method
3. Setup 4: Display Membership Number in Check Listing = No (or the Check Listing Setting record does not exist) for the target outlet/station
4. A check paid with the Member payment method (whose payment membership number is captured) exists in the system

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing (Current Station)"
6. Verify the standard listing UI is shown WITHOUT the "Payment Member" column

**Test Result(s):**
1. The Check Listing page (Paid Check Listing - Current Station) loads correctly
2. The standard listing UI is shown without the "Payment Member" column
3. No new column is added when Setup 4 = No or does not exist
4. The remaining columns and data in the Check Listing are unaffected

### F2.Display Payment Membership Number in Paid Check Listing

#### F2.1.Verify "Payment Member" Column Appears and Shows Membership Number in Paid Check Listing

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: A payment method with Type = Member is configured under POS System > Payment Setup > Payment Methods
3. Setup 2: Member Profile (For Membership in Member Module) is configured under Config by Location with Support = Yes for the target outlet
4. Setup 3: A target membership interface is selected under the Member Profile setup
5. Setup 4: Check Listing Setting is configured with Display Membership Number in Check Listing = Yes for the target outlet
6. Multiple checks paid with the Member payment method (with membership numbers captured) exist across different business days

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing"
6. Verify the listing UI displays a new "Payment Member" column right after the "Payment Method" column
7. Locate the rows corresponding to checks paid with the Member payment method
8. Verify the membership numbers are displayed in the "Payment Member" column for those rows

**Test Result(s):**
1. The Check Listing page (Paid Check Listing) loads correctly
2. The new "Payment Member" column appears in the listing, positioned immediately after the "Payment Method" column
3. The membership numbers of the checks paid with the Member payment method are displayed correctly in the new "Payment Member" column
4. The remaining columns and data in the Check Listing are unaffected

#### F2.2.Verify Empty Value in "Payment Member" Column When Member Payment Membership Number Was Not Captured

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 4: Display Membership Number in Check Listing = Yes is configured for the target outlet
3. A check paid with the Member payment method exists but the membership number was NOT captured during the payment (Setups 1, 2, 3 not fulfilled at the time of payment)

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing"
6. Verify the new "Payment Member" column appears after the "Payment Method" column
7. Locate the row corresponding to the check paid with the Member payment method whose membership number was not captured
8. Verify the "Payment Member" column is empty for that row

**Test Result(s):**
1. The Check Listing page (Paid Check Listing) loads correctly
2. The new "Payment Member" column appears in the listing
3. The "Payment Member" column is empty for the check whose membership number was not captured
4. The remaining columns and data in the Check Listing are unaffected

#### F2.3.Verify No "Payment Member" Column in Paid Check Listing When Setup 4 = No or Does Not Exist

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setups 1, 2, 3 are configured for the Member payment method
3. Setup 4: Display Membership Number in Check Listing = No (or the Check Listing Setting record does not exist) for the target outlet
4. A check paid with the Member payment method (whose membership number is captured) exists in the system

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button
3. Select the "Check Operation" tab
4. Click the "Check Listing" function
5. Select the tab "Paid Check Listing"
6. Verify the standard listing UI is shown WITHOUT the "Payment Member" column

**Test Result(s):**
1. The Check Listing page (Paid Check Listing) loads correctly
2. The standard listing UI is shown without the "Payment Member" column
3. No new column is added when Setup 4 = No or does not exist
4. The remaining columns and data in the Check Listing are unaffected

### F3.Regression Scope - Other Check Listing Tabs and Behavior

#### F3.1.Verify Other Check Listing Tabs and Functions Remain Unaffected

**Prerequisite(s):**
1. POS Client with the fix applied is installed and running on a Workstation (Windows 11 Enterprise)
2. Setup 4: Display Membership Number in Check Listing = Yes is configured
3. Multiple checks exist in the system across different business days, including checks paid with various payment methods

**Step(s):**
1. Log in to POS Client
2. Click the "Admin" button, select the "Check Operation" tab, click the "Check Listing" function
3. Open the "Paid Check Listing (Current Station)" and "Paid Check Listing" tabs and verify the new column is added correctly
4. Open other Check Listing tabs (e.g. Open Check Listing, Void Check Listing) and verify no unexpected changes are introduced
5. Perform search actions (Search by Check No., Search by Member No., etc.) and verify they continue to function correctly

**Test Result(s):**
1. The "Paid Check Listing (Current Station)" and "Paid Check Listing" tabs display the new "Payment Member" column correctly
2. Other Check Listing tabs (Open, Void, etc.) remain unaffected by the new column addition
3. Search actions continue to function correctly
4. No regression in Check Listing functionality is observed after the fix

## Compatibility Testing

### C1.Workstation and Network Compatibility

#### C1.1.Verify Payment Membership Number Display Works Across Networks

**Prerequisite(s):**
1. POS Client is installed on a Workstation (Windows 11 Enterprise)
2. Setups 1, 2, 3, 4 are configured for the target outlet
3. POS backend is reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations
4. Checks paid with the Member payment method exist in the system

**Step(s):**
1. Connect the POS Client to Cloud network, open "Paid Check Listing (Current Station)" and verify the "Payment Member" column displays correctly
2. Repeat under Local, OnPremise, Wireless and Wired networks

**Test Result(s):**
1. The "Payment Member" column displays correctly across all network configurations
2. The membership numbers are displayed correctly in the new column in each network configuration
3. No network-related compatibility issues are introduced by the new feature

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS
- **Issue Type**: Improvement
- **Module**: Check Listing, Member Module
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **QC Tester**: N/A
- **QC Reviewer**: N/A

## Appendix

### Acceptance Criteria (from JIRA HERO-66603)

Background: Currently, it can only show the payment method for member payment and needs to enhance to display payment membership number for the member payment in Check Listing.

Enhancement Requests: It is needed to enhance in displaying the payment membership number for individual POS Check in Check Listing for:

- Paid Check Listing (Current Station)
- Paid Check Listing

The design is similar to payment method "Room Charge" with "Room#" next to it.

Acceptance Criteria:

1. It can display the payment membership for "Member" payment method in "Paid Check Listing (Current Station)" in Check Listing
2. It can display the payment membership for "Member" payment method in "Paid Check Listing" in Check Listing

Release Notes:

- System supports display payment membership number in below check listing pages: Paid Check Listing (Current Station), Paid Check Listing
- The new column "Payment Member" has been added after the column "Payment Method"

Workflow:

1. Login to POS client
2. Click button "Admin"
3. Select tab "Check Operation"
4. Click function "Check Listing"
5. Select the tab "Paid Check Listing (Current Station)" / "Paid Check Listing"
6. If Setup 4 does not exist or has the value "No", system shows the standard listing UI without membership number
7. If Setup 4 has the value "Yes", system adds the column for "Payment Member" to the listing
   - If Setups 1, 2, 3 were fulfilled when payments were saved, the payment membership number of the checks will be displayed in the new column
   - Otherwise, the value will be empty

Setup:

- Setup 1: Payment method with Type = Member (POS System > Payment Setup > Payment Methods)
- Setup 2: Member Profile Support = Yes (POS System > Ordering Setup > Config by Location > Member Profile - For Membership in Member Module)
- Setup 3: Target membership interface (under the Member Profile setup)
- Setup 4: Display Membership Number in Check Listing = Yes (POS System > Ordering Setup > Config by Location > Check Listing Setting)
