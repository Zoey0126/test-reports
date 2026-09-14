# Unexpected set menu parent item added to ordering basket if continuously fast clicking parent item in menu lookup page Test Report

## Functional Testing

### 1.Set Menu Selection Flow In Mobile View

#### 1.1.Normal Selection Of Self-Select Set Menu
**Prerequisite(s):**
1. POS client is available in mobile view
2. A self-select set menu parent item is added to a menu and the menu button is placed on the ordering display panel page

**Step(s):**
1. Login POS client in mobile view
2. Open a check
3. Click a Menu button to open the menu lookup page
4. Tap the set menu parent item once
5. Select the target child item and click button "OK"

**Reproduction Result(s):**
1. Baseline scenario - normal single tap selection did not trigger the duplicated parent item before the fix

**Fix Result(s):**
1. The set menu lookup page prompts for child item selection as normal
2. Exactly one set menu parent item with the selected child item is added to the ordering basket

#### 1.2.Fast Click On Regular Non Set Menu Item
**Prerequisite(s):**
1. POS client is logged in in mobile view with a check opened
2. The menu lookup page contains regular items and set menu items

**Step(s):**
1. Open the menu lookup page from the Menu button
2. Continuously fast click a regular (non set menu) item several times
3. Check the ordering basket

**Reproduction Result(s):**
1. Control scenario - rapid clicking a regular item did not produce the set menu parent duplication defect

**Fix Result(s):**
1. Regular items are added according to the actual taps with no unexpected duplicated item
2. The behavior of regular items is unchanged after the fix

### 2.Rapid Click On Set Menu Parent Item

#### 2.1.Continuous Fast Click Before Child Selection Page Prompted
**Prerequisite(s):**
1. POS client is logged in in mobile view with a check opened
2. A self-select set menu parent item is available on the menu lookup page

**Step(s):**
1. Click a Menu button to open the menu lookup page
2. Continuously fast click the set menu parent item before the set menu lookup page prompts for child item selection
3. Select the target child item and click button "OK"
4. Check the ordering basket

**Reproduction Result(s):**
1. With the previous version, a duplicated (redundant) set menu parent item was added to the ordering basket together with the properly selected set menu

**Fix Result(s):**
1. Only one set menu parent item is added to the ordering basket
2. No redundant set menu parent item appears after continuous fast clicking

#### 2.2.Fast Click On Different Set Menu Items Repeatedly
**Prerequisite(s):**
1. POS client is logged in in mobile view with a check opened
2. At least two self-select set menu parent items are available on the menu lookup page

**Step(s):**
1. Open the menu lookup page
2. Fast click the first set menu parent item, then immediately fast click the second set menu parent item
3. Select child items for each prompted set menu and click button "OK"
4. Check the ordering basket

**Reproduction Result(s):**
1. With the previous version, unexpected duplicated set menu parent items appeared in the ordering basket

**Fix Result(s):**
1. Each selected set menu is added exactly once with its selected child items
2. No redundant set menu parent item is present in the ordering basket

### 3.Ordering Basket Integrity After Rapid Click
**Prerequisite(s):**
1. POS client is logged in in mobile view with a check opened
2. A self-select set menu parent item is available on the menu lookup page

**Step(s):**
1. Continuously fast click the set menu parent item on the menu lookup page
2. Select the target child item and click button "OK"
3. Check the item count and the amount of the check in the ordering basket

**Reproduction Result(s):**
1. With the previous version, the redundant set menu parent item inflated the item count and the check amount

**Fix Result(s):**
1. The ordering basket contains exactly the selected set menu
2. The item count and the check amount reflect only one set menu parent item with its child items

### 4.Regression Scope
1. Menu lookup page - normal item selection in mobile view
2. Self-select set menu child item selection flow
3. Set menu parent item display and pricing in the ordering basket
4. Regular (non set menu) item ordering in mobile view
5. Set menu selection in desktop (non mobile) view
