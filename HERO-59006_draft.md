# HERO-59006 Test Report

Under Menu Management > Item Properties > Menus > View "Modifier Menu", a memory issue occurred when clicking the "Item List of Modifier Menu", and the item list did not display values for extra languages or handle config zone override logic. The fix is delivered in backend version 1.2.117.0 and allows the item list to be viewed without any memory issue, with the correct translated values and overridden item names for extra languages.

## Functional Testing

### 1. Item List of Modifier Menu Dialog

#### 1.1. Item List of Modifier Menu Is Opened Without Memory Issue

**Prerequisite(s):**
1. The tester is logged in to the Infrasys Cloud backend admin portal with access to Menu Management.
2. A modifier menu with items assigned exists and is assigned to the test outlet.
3. Backend version 1.2.117.0 is deployed.

**Step(s):**
1. Open Menu Management > Item Properties > Menus.
2. View "Modifier Menu".
3. Click the magnifying glass icon on "Item List of Modifier Menu".
4. Observe the dialog box and the responsiveness of the admin portal.

**Reproduction Result(s):**
1. Before the fix, a memory issue occurred when clicking "Item List of Modifier Menu".
2. The item list dialog could not be viewed reliably, and the admin portal became unresponsive.

**Fix Result(s):**
1. The dialog box is displayed without any memory issue.
2. The admin portal remains responsive after the dialog box is opened.
3. The item list content is displayed in the dialog box.

#### 1.2. Item List Displays Translated Values for Extra Languages

**Prerequisite(s):**
1. Extra languages are enabled in the system.
2. Extra language values are configured for the items of the modifier menu.
3. The modifier menu is assigned to the test outlet.

**Step(s):**
1. Open Menu Management > Item Properties > Menus.
2. View "Modifier Menu".
3. Click the magnifying glass icon on "Item List of Modifier Menu".
4. Compare the item values displayed for the extra language with the configured values.

**Reproduction Result(s):**
1. Before the fix, the item list did not display values for extra languages.
2. The translated values of the items for the extra language were not shown in the item list.

**Fix Result(s):**
1. The item list displays the translated values for the extra language for each item.
2. The displayed translated values match the extra language values configured for the items.

#### 1.3. Overridden Item Names from Config Zone Override Are Displayed

**Prerequisite(s):**
1. A config zone is configured with item name overrides for the items of the modifier menu.
2. The modifier menu is assigned to that config zone.
3. Extra language values are configured for the items of the modifier menu.

**Step(s):**
1. Open Menu Management > Item Properties > Menus.
2. View "Modifier Menu".
3. Click the magnifying glass icon on "Item List of Modifier Menu".
4. Compare the displayed item names with the item name overrides configured in the config zone.

**Reproduction Result(s):**
1. Before the fix, the item list did not handle the config zone override logic.
2. The overridden item names configured for the config zone were not shown in the item list.

**Fix Result(s):**
1. The item list applies the config zone override logic.
2. The overridden item names configured for the config zone are displayed for the corresponding items.

#### 1.4. All Items of the Current Menu Are Listed in the Dialog Box

**Prerequisite(s):**
1. The current modifier menu contains a known set of items.
2. The items of the menu are configured with names and extra language values.
3. Backend version 1.2.117.0 is deployed.

**Step(s):**
1. Open Menu Management > Item Properties > Menus.
2. View "Modifier Menu".
3. Click the magnifying glass icon on "Item List of Modifier Menu".
4. Compare the listed items with the items assigned to the current menu.

**Reproduction Result(s):**
1. Before the fix, the memory issue occurred when clicking "Item List of Modifier Menu".
2. Because of the memory issue, the dialog box could not be used to confirm the complete item list of the current menu.

**Fix Result(s):**
1. All items belonging to the current menu are shown in the dialog box.
2. No item of the current menu is missing from the list.
3. No item that does not belong to the current menu is shown in the list.

### 2. Regression on Modifier Menu Item List

#### 2.1. Item List Dialog Can Be Opened Repeatedly Without Memory Issue

**Prerequisite(s):**
1. At least two modifier menus with items assigned exist.
2. The tester is logged in to the Infrasys Cloud backend admin portal with access to Menu Management.

**Step(s):**
1. Open Menu Management > Item Properties > Menus and view "Modifier Menu".
2. Click the magnifying glass icon on "Item List of Modifier Menu" and close the dialog box.
3. Repeat step 2 ten times for the same modifier menu.
4. Open the item list of the second modifier menu.

**Reproduction Result(s):**
1. Before the fix, the item list of a modifier menu triggered a memory issue each time it was opened.
2. Repeated opening of the item list was not possible without affecting the admin portal.

**Fix Result(s):**
1. The item list is displayed correctly on every opening without any memory issue.
2. The admin portal remains responsive after repeated opening and closing of the dialog box.
3. The second modifier menu item list is also displayed correctly.

#### 2.2. Item List of Another Modifier Menu Displays Correct Values

**Prerequisite(s):**
1. A second modifier menu with items, extra language values and config zone overrides exists.
2. The second modifier menu is assigned to the same outlet and config zone.

**Step(s):**
1. Open Menu Management > Item Properties > Menus and view "Modifier Menu".
2. Open the item list dialog of the second modifier menu.
3. Check the items, the extra language values and the overridden item names in the list.

**Reproduction Result(s):**
1. Before the fix, the same item list dialog was affected by the memory issue.
2. The item list of the second modifier menu could not be verified before the fix.

**Fix Result(s):**
1. The item list of the second modifier menu is displayed without any memory issue.
2. All items of the second menu are shown with the correct extra language values and overridden item names.

#### 2.3. Item List of a Menu Without Extra Language or Config Zone Override

**Prerequisite(s):**
1. A modifier menu without extra language values and without config zone item name overrides exists.
2. The menu is assigned to the test outlet.

**Step(s):**
1. Open Menu Management > Item Properties > Menus and view "Modifier Menu".
2. Open the item list dialog of this modifier menu.
3. Check the item names displayed in the list.

**Reproduction Result(s):**
1. Before the fix, the item list dialog was affected by the memory issue.
2. This scenario could not be verified reliably before the fix.

**Fix Result(s):**
1. The item list is displayed without any memory issue.
2. The base language item names are displayed correctly for all items of the menu.
3. No empty item name is shown in the list.

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-59006 |
| Summary | [Bug Fix] Performance Optimization for "Item List of Modifier Menu" in View Modifier Menu under Menus |
| Issue Type | Bug |
| Release Version | 1.2.117.0 |
| Test Target | Menu Management > Item Properties > Menus > View Modifier Menu > Item List of Modifier Menu |
| Test Client | Infrasys Cloud backend admin portal |
| Data Setup | Modifier menus with items, extra language values and config zone item name overrides |
| New Setup | None |
| Verification Points | Item list dialog opens without memory issue; complete item list of the current menu; extra language values; config zone overridden item names; repeated opening and other modifier menus |
