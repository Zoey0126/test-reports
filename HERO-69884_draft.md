# HERO-69884 Test Report - POS Function - Support Waive Tax When Running the Print-Check Function (Dev 2/2)

## Background

Based on HERO-65715, it was found that the system only stores the time period of the first printed check and uses it for all subsequent tax determinations. The "Print Check" function runs the tax checking only once at the initial print, and subsequent prints do not re-validate the tax status based on the current time. This enhancement adds the "waive-tax" workflow so that the system performs a fresh tax validation at each print, based on the current timestamp. The feature is available for fine dining mode and bar mode on a normal workstation, supports horizontal desktop view and vertical mobile view, and is not available for API portal.

## Functional Testing

### 1.Tax Configuration in Config by Location

#### 1.1.Verify "Print Check Tax Checking" Configuration Can Be Saved and Takes Effect
**Prerequisite(s):**
1. Tester has access to the Infrasys POS platform with permission to maintain Config by Location
2. Target location, shop, outlet and station records exist for selection
3. Target item groups and target selected tax and S.C. groups exist in the platform setting

**Step(s):**
1. Go to Infrasys POS platform: POS System > Ordering Setup > Config by Location
2. Select the setup "Print Check Tax Checking"
3. Click "Add New" to add a new record
4. Select Apply To = target location (All Locations / Shop / Outlet / Station), and select the target Shop / Outlet / Station when applicable
5. Click "Add Row" to add a new mapping record
6. Set Item Group = target item group
7. Set Select Tax & S.C. Group = target selected tax and S.C. group
8. Set Start Time = target start time and End Time = target end time
9. Click "Save" to save the record
10. Login to the POS client on a normal workstation, open a new check and order item(s) of the configured item group within the configured time period
11. Click function "Print Check"

**Test Result(s):**
1. The "Print Check Tax Checking" record with the mapping record is saved successfully
2. When the current time falls into the mapping record, the system prompts the apply tax item list and applies the configured tax to the target item(s)

#### 1.2.Verify "Force Apply / Waive Print Check Tax" Configuration Can Be Saved and Takes Effect
**Prerequisite(s):**
1. Tester has access to the Infrasys POS platform with permission to maintain Config by Location
2. The setup "Print Check Tax Checking" exists with a mapping record, and a check has tax previously applied due to this setup while the current time is out of all mapping records for that tax

**Step(s):**
1. Go to Infrasys POS platform: POS System > Ordering Setup > Config by Location
2. Select the setup "Force Apply / Waive Print Check Tax"
3. Click "Add New" to add a new record
4. Select Apply To = target location (All Locations / Shop / Outlet / Station), and select the target Shop / Outlet / Station when applicable
5. Set Support = Yes
6. Click "Save" to save the record
7. On the POS client, click function "Print Check" for the check in the prerequisite
8. Observe the waive tax prompt
9. Edit the record and set Support = No, click "Save" to save the record
10. Repeat step 7 and observe the waive tax prompt

**Test Result(s):**
1. The "Force Apply / Waive Print Check Tax" record is saved successfully
2. When Support = Yes, the waive tax prompt shows the "OK" button to force the waive tax flow
3. When Support = No, the waive tax prompt shows the "Cancel" and "Waive tax" buttons for the user to choose

### 2.Print Check Tax Checking Activation

#### 2.1.Verify No Tax Checking When "Print Check Tax Checking" Configuration Does Not Exist
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" does not exist under Config by Location for the target location
2. The POS client is logged in on a normal workstation in fine dining mode

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Print Check" or "Print And Paid"
4. Observe the system behaviour

**Test Result(s):**
1. No apply tax prompt and no waive tax prompt is displayed
2. No tax is applied or waived due to "Print Check Tax Checking"
3. The system continues with the existing print check workflow

#### 2.2.Verify No Tax Checking When "Print Check Tax Checking" Configuration Has No Mapping Record
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists under Config by Location for the target location but contains no mapping record
2. The POS client is logged in on a normal workstation in fine dining mode

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Print Check" or "Print And Paid"
4. Observe the system behaviour

**Test Result(s):**
1. No apply tax prompt and no waive tax prompt is displayed
2. No tax is applied or waived due to "Print Check Tax Checking"
3. The system continues with the existing print check workflow

#### 2.3.Verify Apply Tax When Current Time Falls Into a Mapping Record
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record whose time period includes the current time
2. The POS client is logged in on a normal workstation in fine dining mode

**Step(s):**
1. Open a new check
2. Order item(s) belonging to the item group of the mapping record
3. Click function "Print Check" or "Print And Paid"
4. Observe the system behaviour and confirm the apply tax prompt

**Test Result(s):**
1. The system validates that the current time is included in a mapping record
2. The system follows the existing workflow to prompt the apply tax item list and applies the configured tax to the target item(s)
3. The system continues with the existing print check workflow after the tax is applied

### 3.Auto Waive Tax Determination

#### 3.1.Verify No Waive Tax Prompt When Current Time Is Out of All Mapping Records and No Tax Was Previously Applied
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with mapping record(s), but the current time is out of all mapping records
2. A new check is opened and no tax in the check was previously applied due to "Print Check Tax Checking"

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Print Check" or "Print And Paid"
4. Observe the system behaviour

**Test Result(s):**
1. No apply tax prompt is displayed because the current time is out of all mapping records
2. No waive tax prompt is displayed because the to-be-waived list is empty
3. The system continues with the existing print check workflow

#### 3.2.Verify Previously Applied Tax Is Kept When Current Time Still Falls Into Its Mapping Record
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check was printed within the time period of the mapping record and its tax was applied due to "Print Check Tax Checking"
3. The current time still falls into the mapping record of that tax

**Step(s):**
1. Open the check in the prerequisite
2. Click function "Print Check" or "Print And Paid" again
3. Observe the system behaviour

**Test Result(s):**
1. The system re-validates the previously applied tax against the mapping record at each print based on the current timestamp
2. The tax whose mapping record still includes the current time is not added to the to-be-waived list
3. No waive tax prompt is displayed and the tax remains on the check
4. The system continues with the existing print check workflow

#### 3.3.Verify Previously Applied Tax Is Waived When Current Time Is Out of All Its Mapping Records
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check was printed within the time period of the mapping record and its tax was applied due to "Print Check Tax Checking"
3. The current time is now out of all mapping records for that tax
4. The setup "Force Apply / Waive Print Check Tax" does not exist or is set to "No"

**Step(s):**
1. Open the check in the prerequisite
2. Click function "Print Check" or "Print And Paid" again
3. Observe the system behaviour

**Test Result(s):**
1. The system includes the tax in the to-be-waived list
2. The system prompts a list of target items with the corresponding taxes to be waived
3. The prompt provides the "Cancel" and "Waive tax" buttons

#### 3.4.Verify Only Out-of-Window Taxes Are Listed When Multiple Taxes Were Previously Applied
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with multiple mapping records for different taxes
2. A check was printed with multiple taxes applied due to "Print Check Tax Checking"
3. At the current time, at least one tax still falls into its mapping record and at least one tax is out of all its mapping records

**Step(s):**
1. Open the check in the prerequisite
2. Click function "Print Check" or "Print And Paid" again
3. Observe the waive tax prompt

**Test Result(s):**
1. The system checks each previously applied tax against its mapping record(s) one by one
2. Only the taxes whose current time is out of all their mapping records are included in the to-be-waived list
3. The taxes whose mapping record still includes the current time are kept and are not listed in the waive tax prompt
4. The waive tax prompt lists the target items with the corresponding out-of-window taxes to be waived

### 4.Waive Tax Prompt Handling

#### 4.1.Verify "Cancel" Button Continues Print Check Without Waiving Any Tax
**Prerequisite(s):**
1. The setup "Force Apply / Waive Print Check Tax" does not exist or is set to "No"
2. A check has tax in the to-be-waived list and the waive tax prompt is displayed

**Step(s):**
1. On the waive tax prompt, click button "Cancel"
2. Observe the system behaviour
3. Review the tax of the target item(s) on the check

**Test Result(s):**
1. The system continues print check without waiving any tax
2. All taxes previously applied due to "Print Check Tax Checking" remain on the target item(s)
3. No waive tax action log or audit log is written

#### 4.2.Verify "Waive Tax" Button Waives Taxes From Target Items
**Prerequisite(s):**
1. The setup "Force Apply / Waive Print Check Tax" does not exist or is set to "No"
2. A check has tax in the to-be-waived list and the waive tax prompt is displayed

**Step(s):**
1. On the waive tax prompt, click button "Waive tax"
2. Observe the system behaviour
3. Review the tax of the target item(s) on the check

**Test Result(s):**
1. The system waives the tax(es) from the target item(s) listed in the prompt
2. The waived tax is no longer applied to the target item(s) on the check
3. The system continues with the existing print check workflow

#### 4.3.Verify "OK" Button Forces Waive Tax When "Force Apply / Waive Print Check Tax" Is Yes
**Prerequisite(s):**
1. The setup "Force Apply / Waive Print Check Tax" exists and is set to "Yes"
2. A check has tax in the to-be-waived list and the waive tax prompt is displayed

**Step(s):**
1. On the waive tax prompt, confirm that the "OK" button is displayed
2. Click button "OK"
3. Observe the system behaviour
4. Review the tax of the target item(s) on the check

**Test Result(s):**
1. The waive tax prompt shows the "OK" button instead of the "Cancel" and "Waive tax" choice
2. After clicking "OK", the system proceeds to waive the tax(es) from the target item(s)
3. The waived tax is no longer applied to the target item(s) on the check
4. The system continues with the existing print check workflow

### 5.Waive Tax Logging

#### 5.1.Verify Action Log Is Written After Waive Tax
**Prerequisite(s):**
1. A waive tax operation has been completed via the "Waive tax" or "OK" button on the waive tax prompt

**Step(s):**
1. Complete a waive tax operation during print check
2. Open the action log of the check
3. Locate the log entry for the waive tax operation

**Test Result(s):**
1. An action log is written with action as "Waive Tax"
2. The remark of the action log contains "Print Check Auto Waive Tax"

#### 5.2.Verify Audit Log Is Written After Waive Tax
**Prerequisite(s):**
1. A waive tax operation has been completed via the "Waive tax" or "OK" button on the waive tax prompt

**Step(s):**
1. Complete a waive tax operation during print check
2. Open the audit log
3. Locate the log entry for the waive tax operation

**Test Result(s):**
1. An audit log is written with type as "Waive SC & Tax"
2. The description of the audit log contains "Print Check Auto Waive Tax"

### 6.Function Trigger and Availability

#### 6.1.Verify Auto Waive Tax When Running "Print Check" Function
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check was printed within the time period and its tax was applied due to "Print Check Tax Checking", and the current time is now out of all mapping records for that tax
3. The POS client is logged in on a normal workstation in fine dining mode

**Step(s):**
1. Open the check in the prerequisite
2. Click function "Print Check"
3. Observe the system behaviour

**Test Result(s):**
1. The auto waive tax workflow is triggered when running the "Print Check" function
2. The waive tax prompt with the target items and corresponding taxes to be waived is displayed

#### 6.2.Verify Auto Waive Tax When Running "Print And Paid" Function
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check was printed within the time period and its tax was applied due to "Print Check Tax Checking", and the current time is now out of all mapping records for that tax
3. The POS client is logged in on a normal workstation in fine dining mode

**Step(s):**
1. Open the check in the prerequisite
2. Click function "Print And Paid"
3. Observe the system behaviour

**Test Result(s):**
1. The auto waive tax workflow is triggered when running the "Print And Paid" function
2. The waive tax prompt with the target items and corresponding taxes to be waived is displayed

#### 6.3.Verify Auto Waive Tax Is Available in Fine Dining Mode and Bar Mode
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check with tax in the to-be-waived list exists in fine dining mode and another one exists in bar mode

**Step(s):**
1. Login to the POS client on a normal workstation in fine dining mode
2. Open the target check and click function "Print Check"
3. Observe the system behaviour
4. Login to the POS client on a normal workstation in bar mode
5. Open the target check and click function "Print Check"
6. Observe the system behaviour

**Test Result(s):**
1. The auto waive tax workflow is available in fine dining mode
2. The auto waive tax workflow is available in bar mode
3. The waive tax prompt is displayed correctly in both modes

#### 6.4.Verify Auto Waive Tax Is Not Available for API Portal
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check has tax previously applied due to "Print Check Tax Checking" and the current time is out of all mapping records for that tax
3. API portal access is available for the target environment

**Step(s):**
1. Through the API portal, perform the available print check related operation for the target check
2. Observe the API portal response and the check status

**Test Result(s):**
1. The auto waive tax workflow is not available for API portal
2. No waive tax prompt or waive tax processing is triggered through the API portal
3. The tax previously applied on the check remains unchanged through the API portal operation

#### 6.5.Verify Auto Waive Tax Is Available in Horizontal Desktop View and Vertical Mobile View
**Prerequisite(s):**
1. The setup "Print Check Tax Checking" exists with a mapping record
2. A check has tax in the to-be-waived list
3. A normal workstation supports horizontal desktop view and a mobile device supports vertical mobile view

**Step(s):**
1. Login to the POS client with horizontal desktop view
2. Open the target check and click function "Print Check"
3. Observe the waive tax prompt
4. Login to the POS client with vertical mobile view
5. Open the target check and click function "Print Check"
6. Observe the waive tax prompt

**Test Result(s):**
1. The auto waive tax workflow is available in horizontal desktop view
2. The auto waive tax workflow is available in vertical mobile view
3. The waive tax prompt is displayed correctly in both views

## Test Environment

- **Test Environment**: Local test environment
- **Test Devices**: Normal workstation (horizontal desktop view) and mobile device (vertical mobile view), fine dining mode and bar mode
- **Component**: POS
- **Issue Type**: Improvement
- **Sprint**: POS - 20261019-20261030
- **QC Tester**: Arsinoe Xie (arsinoe.xie@shijigroup.com)
- **QC Reviewer**: Kiet Zou (kiet.zou@shijigroup.com)
