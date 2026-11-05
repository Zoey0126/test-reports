# HERO-63090 Test Report - POS Feature - Allergens - Create New Setup in Infrasys Frontend (POS Deploy)

## Background

This is the POS deployment case for the Allergen feature. The system supports new allergen related functions in the Infrasys POS frontend, including the "Assign Allergens To Check" function on the ordering panel, a new "Allergens" tab in the whole check view and in the item detail page, check allergen display in the ordering basket extra information section, allergen icons and allergen group icons on display panel buttons, and allergen violation checking during send check, merge check and split check. This case is related to tickets HERO-63720, HERO-66559 and HERO-67671.

## Functional Testing

### F1.Allergen Configuration in Config by Location

#### F1.1.Verify Allergens Control Configuration Can Be Saved and Takes Effect

**Prerequisite(s):**
1. Tester has access to the Infrasys POS platform with permission to maintain Config by Location
2. Target location, shop, outlet and station records exist for selection

**Step(s):**
1. Go to Infrasys POS platform: POS System > Ordering Setup > Config by Location
2. Select the configuration "Allergens Control" under the "System" section
3. Click "Add New" to add a new record
4. Select Apply To = target location (All Locations / Shop / Outlet / Station), and select the target Shop / Outlet / Station when applicable
5. Set Support = Yes
6. Set Row And Column For Selecting Allergens In Horizontal View = a target Row:Column value
7. Set Row And Column For Selecting Allergens In Vertical View = a target Row:Column value
8. Set Alert Ordered Item With Violating Assigned Allergen Rules When Send Check = Yes
9. Set Item Details Page Supports Allergen = Yes
10. Click "Save" to save the record
11. Login to the POS client and open a check on the ordering panel

**Test Result(s):**
1. The "Allergens Control" record is saved successfully with all configured values
2. The function "Assign Allergens To Check" is available on the ordering panel
3. The configured row and column values are applied to the allergen selection screen in horizontal desktop view and vertical mobile view respectively

#### F1.2.Verify Display Check Extra Information in Ordering Basket Configuration With Check Allergens

**Prerequisite(s):**
1. Tester has access to the Infrasys POS platform with permission to maintain Config by Location
2. Allergens exist in the platform setting and can be assigned to a check

**Step(s):**
1. Go to Infrasys POS platform: POS System > Ordering Setup > Config by Location
2. Select the configuration "Display Check Extra Information in Ordering Basket"
3. Click "Add New" to add a new record
4. Select Apply To = target location, and select the target Shop / Outlet / Station when applicable
5. Set Support = Yes
6. Set Display Information = Check Allergens
7. Click "Save" to save the record
8. Open a check on the POS client and assign allergens to the check

**Test Result(s):**
1. The record is saved successfully with Display Information = Check Allergens
2. The ordering basket extra information section displays the "Check Allergens" information for the check

#### F1.3.Verify Whole Check Tab Control Configuration With Allergens Display Information

**Prerequisite(s):**
1. Tester has access to the Infrasys POS platform with permission to maintain Config by Location
2. An old check exists on the POS client

**Step(s):**
1. Go to Infrasys POS platform: POS System > Ordering Setup > Config by Location
2. Select the configuration "Whole Check Tab Control"
3. Click "Add New" to add a new record
4. Select Apply To = target location, and select the target Shop / Outlet / Station when applicable
5. Set Support = Yes
6. Set Display Information = Allergens
7. Click "Save" to save the record
8. Open an old check on the POS client and click the "Whole Check" button

**Test Result(s):**
1. The record is saved successfully with Display Information = Allergens
2. The "Allergens" tab is displayed in the whole check view

### F2.Assign Allergens To Check Function

#### F2.1.Verify Function Is Blocked When Allergens Control Is Set to No

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = No under Config by Location
2. A check is opened on the ordering panel of the POS client

**Step(s):**
1. Open a check on the ordering panel
2. Click the function "Assign Allergens To Check"
3. Observe the system behaviour

**Test Result(s):**
1. The system prompts the error message "Function related to allergens is not allowed"
2. The action aborts and no allergen selection screen is displayed

#### F2.2.Verify Allergen Selection Screen Layout and Tabs

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = Yes
2. Allergens and allergen groups are maintained in the platform setting
3. Some allergens are already assigned to the opened check

**Step(s):**
1. Open a check on the ordering panel
2. Click the function "Assign Allergens To Check"
3. Observe the displayed selection screen
4. Switch between the tab "All" and the tab "Selected (n)"

**Test Result(s):**
1. The system displays the "Assign allergens to check" selection screen with the title "Assign allergens to check"
2. Tab "All" shows all available allergens (selected and unselected), grouped by their allergen groups
3. Tab "Selected (n)" shows only the currently selected allergens with their groups, where n equals the count of selected allergens

#### F2.3.Verify Allergen Group List Display Rules on the Selection Screen

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. Platform setting contains allergen groups with at least one allergen, and at least one allergen that is not assigned to any group
3. The number of groups exceeds one page so that pagination can be verified

**Step(s):**
1. Open a check and click the function "Assign Allergens To Check"
2. Observe the allergen group list on the selection screen
3. Navigate through the group list pages
4. Check the first entry of the group list and the content of each group row

**Test Result(s):**
1. Only groups containing at least one allergen from the platform setting are displayed
2. The group list supports pagination
3. The "Ungrouped" group is shown as the first entry and includes all ungrouped allergens
4. The number of rows (columns for mobile view) per page follows the configuration "Row And Column For Selecting Allergens In Horizontal View" or "Row And Column For Selecting Allergens In Vertical View"
5. Each group row shows the group name and the selected count of allergens under that group

#### F2.4.Verify Allergen List Pagination and Select All Checkbox

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes with row and column values configured for horizontal and vertical view
2. The selected allergen group contains more allergens than one page can display

**Step(s):**
1. Open a check and click the function "Assign Allergens To Check"
2. Select an allergen group and observe the allergen list of the currently selected group
3. Navigate through the allergen list pages
4. Tick the checkbox "Select All" and then untick it

**Test Result(s):**
1. The allergen list of the currently selected group supports pagination
2. The number of rows and columns follows the configuration "Row And Column For Selecting Allergens In Horizontal View" or "Row And Column For Selecting Allergens In Vertical View"
3. Ticking "Select All" selects all allergens in the current selected group
4. Unticking "Select All" deselects all allergens in the current selected group

#### F2.5.Verify Closing the Selection Screen With Button X Discards Changes

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. A check is opened with some allergens already assigned

**Step(s):**
1. Open a check and click the function "Assign Allergens To Check"
2. Change the selected allergens on the selection screen
3. Click button "X" to close the screen
4. Reopen the selection screen and check the assigned allergens of the check

**Test Result(s):**
1. The selection screen is closed after clicking button "X"
2. No changes are saved and the action aborts
3. The allergens assigned to the check remain unchanged

#### F2.6.Verify Confirm and Assign Updates the Allergens Assigned to the Check

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. A check is opened on the ordering panel

**Step(s):**
1. Open a check and click the function "Assign Allergens To Check"
2. Select the target allergen(s) on the selection screen
3. Click the button "Confirm and assign"
4. Reopen the selection screen and verify the assigned allergens

**Test Result(s):**
1. The system updates and assigns the selected allergens to the check
2. The newly assigned allergens are shown as selected when the selection screen is reopened

#### F2.7.Verify Audit Log Written for Assign Allergen to Check

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. A check is opened with allergens A and B already assigned

**Step(s):**
1. Open the check and click the function "Assign Allergens To Check"
2. Unselect allergen B, keep allergen A selected, and newly select allergen C
3. Click the button "Confirm and assign"
4. Check the audit log record written for this operation

**Test Result(s):**
1. The system writes an audit log with Type = Assign allergen to check
2. The audit log content contains Description: Assign allergen to <check number>
3. The audit log extra information shows Removed Allergens = allergen B (previously assigned but unselected this time)
4. The audit log extra information shows Newly Selected Allergens = allergen C (newly added this time)
5. The audit log extra information shows Remaining Allergens = allergen A (stays throughout this operation)

#### F2.8.Verify Ordering Basket Extra Information Is Updated With Check Allergens

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Display Check Extra Information in Ordering Basket" exists and is configured to include "Check Allergens"
3. A check is opened on the ordering panel

**Step(s):**
1. Open a check and click the function "Assign Allergens To Check"
2. Select the target allergen(s) and click the button "Confirm and assign"
3. Observe the extra information section of the ordering basket

**Test Result(s):**
1. The system updates the check extra information section in the ordering basket
2. The "Check Allergens" information in the ordering basket reflects the allergens currently assigned to the check

#### F2.9.Verify Allergen Group Icons and Allergen Icons on Display Panel Buttons

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. Allergen group icons and allergen icons are configured with sequence settings in the platform setting
3. Menu items, modifiers, menus, menu lookup items and self-select set menu child items are attached with allergens and allergen groups

**Step(s):**
1. Assign allergens to a check via the function "Assign Allergens To Check" and confirm
2. Observe the display panel buttons for Items, Modifier, Menu Lookup, Modifier Lookup and Setup Menu
3. Open a menu lookup and observe the buttons of items or sub-menus inside the menu lookup
4. Open a self-select set menu and observe the buttons of child items

**Test Result(s):**
1. The system updates the display panel buttons with their respective allergen group icons and allergen icons for Items, Modifier, Menu Lookup, Modifier Lookup, Setup Menu, items or sub-menus inside the menu lookup, and child items inside the self-select set menu
2. Up to three highest-priority allergen group icons and allergen icons are shown on the top-left corner of each button
3. Allergen group icons are prioritized according to the sequence setting and allergen icons according to the sequence setting
4. All allergen group icons are displayed before allergen icons

### F3.Allergens Tab in Whole Check View

#### F3.1.Verify Allergens Tab Is Displayed When Whole Check Tab Control Includes Allergens

**Prerequisite(s):**
1. The configuration "Whole Check Tab Control" exists and its Display Information includes "Allergens"
2. The configuration "Allergens Control" exists with Support = Yes
3. An old check exists with allergens assigned via the function "Assign Allergens To Check", and contains ordered items attached with allergens

**Step(s):**
1. Open the old check on the POS client
2. Click the button "Whole Check"
3. Observe the tab list of the whole check view
4. Open the "Allergens" tab and check the content of the first section and the second section
5. Repeat with a check that has no assigned allergens and no ordered items with allergens

**Test Result(s):**
1. The system displays the new tab "Allergens", inserted after the "Basic Information" tab by default
2. The first section shows Title = Check Allergens with the list of all allergens currently assigned to the check; if no allergens are assigned, an empty value is displayed
3. The second section shows each row consisting of individual items with their respective allergens assigned in setting; if no ordered items in the check have the allergen, an empty value is displayed

#### F3.2.Verify Allergens Tab Is Not Displayed When Whole Check Tab Control Excludes Allergens

**Prerequisite(s):**
1. The configuration "Whole Check Tab Control" exists and its Display Information does NOT include "Allergens"
2. An old check exists with assigned allergens

**Step(s):**
1. Open the old check on the POS client
2. Click the button "Whole Check"
3. Observe the tab list of the whole check view

**Test Result(s):**
1. The system does not display the tab "Allergens" in the whole check view
2. Other existing tabs of the whole check view remain unchanged

### F4.Allergens Tab in Item Detail Page

#### F4.1.Verify Existing Item Detail Behaviour When Allergens Control Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = No
2. The configuration "Item Details Page Supports Allergen" exists with value Yes
3. A check with ordered items exists

**Step(s):**
1. Open the check on the POS client
2. Right click on any item in the check
3. Select the option "Item Detail"

**Test Result(s):**
1. The system displays the item detail with existing behaviour without tabs
2. No "Allergens" tab is displayed

#### F4.2.Verify Existing Item Detail Behaviour When Item Details Page Supports Allergen Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Item Details Page Supports Allergen" does not exist, or is set to No
3. A check with ordered items exists

**Step(s):**
1. Open the check on the POS client
2. Right click on any item in the check
3. Select the option "Item Detail"

**Test Result(s):**
1. The system displays the item detail with existing behaviour without tabs
2. No "Allergens" tab is displayed

#### F4.3.Verify Item Detail Tabs Are Displayed When Both Configurations Are Yes

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Item Details Page Supports Allergen" exists with value Yes
3. A check exists containing an item attached with multiple allergens, and another item whose allergen description exceeds 5 lines

**Step(s):**
1. Open the check on the POS client
2. Right click on an item and select the option "Item Detail"
3. Observe the tabs displayed on the item detail page
4. Open the tab "Allergens" and check the displayed allergen content
5. Repeat with the item whose allergen content exceeds 5 lines

**Test Result(s):**
1. The system displays the tabs "Basic Information", "Discount Info" and "Allergens"
2. Tab "Basic Information" displays all existing item detail fields from the original Basic Information section
3. Tab "Discount Info" displays all existing discount-related information from the original Discount Info section
4. Tab "Allergens" displays a comma-separated list of all allergens assigned to the menu item for item description and reference
5. The display is limited to a maximum of 5 lines and a vertical scrollbar is provided when the content exceeds the display limit

### F5.Allergen Violation Checking When Sending Check

#### F5.1.Verify No Violation Checking When Allergens Control Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = No
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists with assigned allergens and ordered items that violate the assigned allergens

**Step(s):**
1. Open the check and order new item(s)
2. Click the function "Print Check" / "Send Check" / "Print And Paid" / "Partial Send Check" / "Paid" (in fast food mode or simple kiosk mode)

**Test Result(s):**
1. No allergen violation checking is performed
2. No warning dialog box is prompted
3. The system continues with the existing send check operation

#### F5.2.Verify No Violation Checking When No Allergens Are Assigned to the Check

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists WITHOUT any assigned allergens

**Step(s):**
1. Open the check and order new item(s)
2. Click the function "Send Check"

**Test Result(s):**
1. No allergen violation checking is performed because there are no allergens assigned to the check
2. The system continues with the existing send check operation without any warning

#### F5.3.Verify No Warning When No New Item Is Included

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists with assigned allergens, and all ordered items were already sent before (no new item)

**Step(s):**
1. Open the check without ordering any new item
2. Click the function "Print Check"

**Test Result(s):**
1. No allergen violation warning is prompted because no new item is included
2. The system continues with the existing print check operation

#### F5.4.Verify No Warning When Alert Configuration Is Set to No

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to No
3. A check exists with assigned allergens and a new ordered item that violates the assigned allergens

**Step(s):**
1. Open the check and order the new item that violates the assigned allergens
2. Click the function "Send Check"

**Test Result(s):**
1. No warning dialog box is prompted
2. The system continues with the existing send check operation

#### F5.5.Verify No Warning When No Ordered Items Violate the Assigned Allergens

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists with assigned allergens, and all new and old ordered items (normal item, parent set menu item, set menu child item and modifier) do NOT violate the assigned allergens

**Step(s):**
1. Open the check and order new item(s) that do not violate the assigned allergens
2. Click the function "Send Check"

**Test Result(s):**
1. The system checks all new and old ordered items against the check assigned allergens and finds no violation
2. No warning dialog box is prompted
3. The system continues with the existing send check operation

#### F5.6.Verify Warning Dialog and Cancel Action When Violating Items Exist

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists with assigned allergens, containing new and/or old ordered items (normal item, parent set menu item, set menu child item or modifier) that violate the assigned allergens

**Step(s):**
1. Open the check and order a new item that violates the assigned allergens
2. Click the function "Send Check"
3. Observe the prompted dialog box
4. Click the button "Cancel"

**Test Result(s):**
1. The system prompts a warning dialog box with the message "Some items in check attached allergens"
2. Clicking "Cancel" cancels the send check / print / paid action and returns to the ordering panel
3. The check is not sent and remains on the ordering panel

#### F5.7.Verify Continue Action Writes Station Log and Proceeds With Send Check

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. A check exists with assigned allergens and new ordered items that violate the assigned allergens, while old ordered items also contain violating items

**Step(s):**
1. Open the check and order a new item that violates the assigned allergens
2. Click the function "Send Check"
3. On the warning dialog box, click the button "Continue"
4. Check the station log record and the check status

**Test Result(s):**
1. The system writes a station log with the current user and the corresponding violated NEW items only (old violating items are not logged)
2. The system continues with the existing send check operation
3. The check is sent successfully

### F6.Allergen Handling in Merge Check

#### F6.1.Verify Existing Merge Workflow When Allergens Control Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = No
2. An old check with assigned allergens exists as the current check, and another old check exists as the merge target

**Step(s):**
1. Open the current check
2. Click the function "Merge Table" or "Merge Multiple Table"
3. Follow the existing workflow to perform the merge table operation

**Test Result(s):**
1. No allergen violation checking is performed
2. No allergen union update is performed
3. The system continues with the existing merge table workflow

#### F6.2.Verify Allergen Union Without Warning When Alert Configuration Is No

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to No
3. The current check has assigned allergens, and the target check has different assigned allergens, with items violating each other's allergens

**Step(s):**
1. Open the current check and click the function "Merge Table"
2. Follow the existing workflow to merge the target check into the current check
3. After merging, check the assigned allergens of the current check

**Test Result(s):**
1. No allergen violation warning is prompted during the merge
2. The system updates the current check with the union of its current allergens and all allergens from the target check(s)
3. The merge table workflow completes successfully

#### F6.3.Verify Allergen Union Without Warning When No Violation Is Detected

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and no items on either check violate the other check's assigned allergens

**Step(s):**
1. Open the current check and click the function "Merge Table"
2. Follow the existing workflow to merge the target check into the current check
3. After merging, check the assigned allergens of the current check

**Test Result(s):**
1. The system validates all items from the current check against the target check's assigned allergens and all items from the target check against the current check's assigned allergens
2. No warning is prompted because no violation is detected
3. The system updates the current check with the union of its current allergens and all allergens from the target check(s)

#### F6.4.Verify Merge Warning and No Action Aborts the Merge When Violations Exist

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and items on at least one side violate the other check's assigned allergens

**Step(s):**
1. Open the current check and click the function "Merge Table" or "Merge Multiple Table"
2. Follow the existing workflow to select the target check(s)
3. Observe the prompted merge confirmation dialog
4. Click the button "No"

**Test Result(s):**
1. The system displays the merge confirmation "Confirm to merge table from X?" followed by the warning "Some items in check attached allergens" with the violating items and table list
2. Clicking "No" aborts the action and the merge is not performed
3. The allergens of both checks remain unchanged

#### F6.5.Verify Merge Yes Writes Station Log and Updates Allergen Union

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and violating items exist

**Step(s):**
1. Open the current check and click the function "Merge Table"
2. Follow the existing workflow to select the target check
3. On the merge confirmation dialog with the allergen warning, click the button "Yes"
4. Check the station log record and the assigned allergens of the merged current check

**Test Result(s):**
1. The system writes a station log with the current user and the corresponding violating items
2. The system updates the current check with the union of its current allergens and all allergens from the target check(s)
3. The system continues with the existing merge table workflow and the merge completes

### F7.Allergen Handling in Split Check

#### F7.1.Verify Existing Split Workflow When Allergens Control Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = No
2. An old check with assigned allergens exists

**Step(s):**
1. Open the old check
2. Click one of the split-related functions: Split Check With Function List / Split To Other Table / Split Item To Other Table / Split Item With Quantity / Split Table
3. Follow the existing workflow to select items and perform the split operation to another existing table

**Test Result(s):**
1. No allergen violation checking is performed
2. No allergen union update is performed on the target check
3. The system continues with the respective existing split operation workflow

#### F7.2.Verify Allergen Union Without Warning When Alert Configuration Is Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" does not exist, or is set to No
3. An old check with assigned allergens exists, and the target check has assigned allergens with items violating each other's allergens

**Step(s):**
1. Open the old check and click the function "Split To Other Table"
2. Follow the existing workflow to select items and perform the split operation
3. After splitting, check the assigned allergens of the target check

**Test Result(s):**
1. No allergen violation warning is prompted during the split
2. The system updates each target check with the union of its current allergens and all allergens from the current check
3. The split operation completes successfully

#### F7.3.Verify Allergen Union Without Warning When No Violation Is Detected in Split

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and no items on either side violate the other check's assigned allergens

**Step(s):**
1. Open the old check and click the function "Split Item To Other Table"
2. Follow the existing workflow to select items and perform the split operation
3. After splitting, check the assigned allergens of the target check

**Test Result(s):**
1. The system validates the selected items from the current check against the target check's assigned allergens and the items in the target check against the current check's assigned allergens
2. No warning is prompted because no violation is detected
3. The system updates each target check with the union of its current allergens and all allergens from the current check

#### F7.4.Verify Split Warning and Cancel Action Aborts the Split When Violations Exist

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and the selected items or target check items violate the other check's assigned allergens

**Step(s):**
1. Open the old check and click the function "Split To Other Table"
2. Follow the existing workflow to select items and the target table
3. Observe the prompted warning message
4. Click the button "Cancel"

**Test Result(s):**
1. The system displays the warning message "Some items in check attached allergens" with the list of involved tables
2. Clicking "Cancel" aborts the action and the split is not performed
3. The allergens of the current check and the target check remain unchanged

#### F7.5.Verify Split Continue Updates Allergen Union on Target Check

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. The current check and the target check have assigned allergens, and violating items exist

**Step(s):**
1. Open the old check and click the function "Split Item With Quantity"
2. Follow the existing workflow to select items and the target table
3. On the warning message, click the button "Continue"
4. After splitting, check the assigned allergens of the target check

**Test Result(s):**
1. The system updates each target check with the union of its current allergens and all allergens from the current check
2. The system continues with the respective split operation workflow and the split completes

#### F7.6.Verify Allergens Are Copied to Every New Check When Splitting to New Table

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. An old check with multiple assigned allergens exists

**Step(s):**
1. Open the old check
2. Click one of the split-related functions that creates new check(s): Split And Pay / Split Check By Options / Split Check With Function List / Split Check / Equal Quantity By Parties / Specific Percentage / Specific Amount / Split Table by Party
3. Follow the existing workflow to perform the split operation
4. After splitting, open each new check and verify the assigned allergens

**Test Result(s):**
1. The system assigns all allergens from the current check to every new check
2. The system continues with the respective split operation workflow and the split completes

#### F7.7.Verify No Allergen Copying When Splitting to New Table With Allergens Control Missing or No

**Prerequisite(s):**
1. The configuration "Allergens Control" does not exist, or exists with Support = No
2. An old check with assigned allergens exists

**Step(s):**
1. Open the old check
2. Click the function "Split Check" to split the check to new table(s)
3. Follow the existing workflow to perform the split operation
4. After splitting, open each new check and verify the assigned allergens

**Test Result(s):**
1. No allergens are assigned to the new check(s)
2. The system continues with the respective existing split operation workflow

### F8.Availability and Display Scope

#### F8.1.Verify Assign Allergens To Check Is Available Only on the Ordering Panel

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The POS client is logged in

**Step(s):**
1. Open a check and check the ordering panel function list for the function "Assign Allergens To Check"
2. Check other panels and function lists outside the ordering panel for the function "Assign Allergens To Check"

**Test Result(s):**
1. The function "Assign Allergens To Check" is available on the ordering panel
2. The function "Assign Allergens To Check" is not available on any panel other than the ordering panel

#### F8.2.Verify Allergen Features in Horizontal Desktop View, Vertical Mobile View and Day/Night Mode

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes with row and column values configured for both horizontal and vertical views
2. Both a horizontal desktop POS client and a vertical mobile POS client are available
3. A check with assigned allergens and ordered items attached with allergens exists

**Step(s):**
1. On the horizontal desktop view, open the check, use the function "Assign Allergens To Check", view the whole check "Allergens" tab, the item detail "Allergens" tab, the ordering basket check allergens and the display panel button icons
2. Switch the client between Day mode and Night mode and repeat the checks
3. Repeat steps 1 and 2 on the vertical mobile view

**Test Result(s):**
1. All allergen features are available and displayed correctly in the horizontal desktop view
2. All allergen features are available and displayed correctly in the vertical mobile view, with the row and column layout following the vertical view configuration
3. All allergen features are displayed correctly in both Day mode and Night mode

#### F8.3.Verify Allergen Features Are Not Available for API Portal

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. API Portal access with valid credentials is available
3. A check with assigned allergens exists

**Step(s):**
1. Check the API Portal interface list for any allergen related request (assign allergens to check, check allergen enquiry)
2. Attempt to trigger allergen related operations through existing API Portal check operation requests

**Test Result(s):**
1. No allergen related function is available on the API Portal
2. Existing API Portal check operations do not include or expose the allergen assignment features

#### F8.4.Verify Allergen Groups Directly Mapped to Menus or Items Are Excluded

**Prerequisite(s):**
1. The configuration "Allergens Control" exists with Support = Yes
2. The configuration "Item Details Page Supports Allergen" is set to Yes and "Alert Ordered Item With Violating Assigned Allergen Rules When Send Check" is set to Yes
3. Allergen group "Seafood" includes allergen "Crab"; Item A is attached by the allergen group "Seafood" only, but not by the allergen "Crab" directly
4. A check with assigned allergens contains Item A as a newly ordered item

**Step(s):**
1. Open the check, right click Item A and open the item detail "Allergens" tab
2. Open the whole check view and check the "Allergens" tab content for Item A
3. Assign the allergen "Crab" to the check and click the function "Send Check"

**Test Result(s):**
1. The allergen group "Seafood" does not appear in the item detail "Allergens" tab for Item A
2. The allergen group "Seafood" does not appear in the whole check "Allergens" tab for Item A
3. The allergen group "Seafood" is not considered as an allergen violation checking target, so Item A is not treated as violating the check assigned allergen "Crab"
4. Only allergens directly assigned to menus or items are supported in the item detail page, whole check allergen tab and allergen violation checking

## Test Environment

- **Test Environment**: Local test environment, HQ test environment
- **Component**: POS
- **Issue Type**: Improvement
- **View Mode**: Horizontal desktop view, Vertical mobile view, Day/Night mode
- **QC Tester**: Viktor Peng (viktor.peng@shijigroup.com)
- **QC Reviewer**: Tian Xie (tian.xie@shijigroup.com)
