# Open description modifiers are combined wrongly into same modifier in guest check and receipt Test Report

## Functional Testing

### 1.Print Format And Item Setup Verification

#### 1.1.Guest check and receipt print format with combine same item to one line

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. Access to Infrasys POS platform POS System > Printing Setup and Menu Management > Item Properties > Items is available
3. An item that supports open description modifier is available for ordering

**Step(s):**
1. Go to POS System > Printing Setup > Print Formats and add or open a guest check or receipt format
2. Under section "Special Settings", set Item Grouping Method = "Combine same item to one line"
3. Save the record
4. Go to Menu Management > Item Properties > Items and open the item used for testing
5. Under section "Ordering Control", set Input Item Name to one of "Allow to change item name", "Allow to append text after item name", "Allow to append text after item name (By click panel button)" or "Allow to change item name and append text after item name"
6. Save the record

**Reproduction Result(s):**
1. This is a setup prerequisite and is not directly affected by the bug

**Fix Result(s):**
1. The print format and item setup are saved successfully with the configured values
2. The Item Grouping Method and Input Item Name settings are persisted correctly

### 2.Guest Check Printing With Open Description Modifiers

#### 2.1.Guest check does not combine items with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. The guest check print format is configured with Item Grouping Method = "Combine same item to one line" as in scenario 1.1
3. The item used for testing is configured with one of the open description Input Item Name options as in scenario 1.1
4. A new check can be created on the POS workstation

**Step(s):**
1. Open a new check on the POS workstation
2. Order one item with an open description modifier
3. Enter a description for the modifier, for example "a1"
4. Order another new item which is the same item as in step 2
5. Order an open description modifier for the above item
6. Enter a different description from step 3 for the modifier, for example "b2"
7. Click function "Print and Paid"
8. Inspect the printed guest check

**Reproduction Result(s):**
1. The printed guest check wrongly combines the two items with different open description modifiers into a single item line
2. The two different modifiers are merged into one modifier and the item is shown with quantity 2 (for example A (modifier: m111) with quantity 2 instead of A (modifier: m111) and A (modifier: m222) on separate lines)

**Fix Result(s):**
1. The printed guest check prints the two items on separate lines
2. Each item is printed with its own open description modifier, for example "A (modifier: m111)" and "A (modifier: m222)"
3. The system does not combine items with different open description modifiers into one item

#### 2.2.Guest check does not combine child items of set menu with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. The guest check print format is configured with Item Grouping Method = "Combine same item to one line" as in scenario 1.1
3. A set menu with a child item that supports open description modifier is available for ordering

**Step(s):**
1. Open a new check on the POS workstation
2. Order one set menu item with a child item open description modifier
3. Enter a description for the child item modifier, for example "a1"
4. Order another set menu item which is the same as in step 2
5. Order a child item open description modifier for the above set menu item
6. Enter a different description from step 3 for the child item modifier, for example "b2"
7. Click function "Print and Paid"
8. Inspect the printed guest check

**Reproduction Result(s):**
1. The printed guest check wrongly combines the two set menu child items with different open description modifiers into a single child item line
2. The two different child item modifiers are merged into one modifier

**Fix Result(s):**
1. The printed guest check prints the two set menu items separately
2. Each child item is printed with its own open description modifier, and child items with different open description modifiers are not combined into one item

#### 2.3.Guest check does not combine child item modifiers of set menu with different descriptions

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. The guest check print format is configured with Item Grouping Method = "Combine same item to one line" as in scenario 1.1
3. A set menu with a child item that has an open description modifier is available for ordering

**Step(s):**
1. Open a new check on the POS workstation
2. Order one set menu item with a child item open description modifier
3. Enter a description for the child item modifier, for example "a1"
4. Order another set menu item which is the same as in step 2
5. Order a child item open description modifier for the above set menu item
6. Enter a different description from step 3 for the child item modifier, for example "b2"
7. Click function "Print and Paid"
8. Inspect the printed guest check, focusing on the child item modifier lines

**Reproduction Result(s):**
1. The printed guest check wrongly combines the two child item modifiers with different descriptions into a single modifier line under the same child item

**Fix Result(s):**
1. The printed guest check prints the two child item modifiers separately with their respective descriptions
2. The system does not combine child item modifiers with different descriptions

### 3.Receipt Printing With Open Description Modifiers

#### 3.1.Receipt does not combine items with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. The receipt print format is configured with Item Grouping Method = "Combine same item to one line" as in scenario 1.1
3. A printed check exists that contains either 2 or more items of the same type, each with the same type of open description modifier but different descriptions, or 2 or more set menu items of the same type whose child items have the same type of open description modifier but different descriptions

**Step(s):**
1. Open the printed check described in the prerequisites on the POS workstation
2. Click function "Paid" to go to the cashier panel
3. Select the target payment method
4. Follow the existing workflow to settle the check
5. Inspect the printed receipt

**Reproduction Result(s):**
1. The printed receipt wrongly combines the modifiers or child items or modifiers of child items with different descriptions into 1 item, similar to the printed check

**Fix Result(s):**
1. The printed receipt prints the items, child items and child item modifiers separately
2. The system does not combine items, child items or child item modifiers with different descriptions into one item on the receipt

### 4.Action Slip (Pantry Message) With Open Description Modifiers

#### 4.1.Action slip does not combine items or child item modifiers with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. An action slip print format is configured with Item Grouping Method = "Combine same item to one line" or "Combine same item based on item sorting method 1"
3. An old check exists that contains either 2 or more items of the same type each with the same type of open description modifier but different descriptions, or 2 or more set menu items of the same type whose child items have the same type of open description modifier but different descriptions

**Step(s):**
1. Open the old check described in the prerequisites on the POS workstation
2. Click function "Pantry Message"
3. Select the target items that match the condition (items or set menu child items with the same modifier type but different descriptions)
4. Click "Confirm"
5. Inspect the printed action slip

**Reproduction Result(s):**
1. The printed action slip wrongly combines the modifiers or modifiers of child items with different descriptions into 1 item

**Fix Result(s):**
1. The printed action slip prints the items and modifiers separately
2. The system does not combine items, child items or child item modifiers with different open description modifiers into one item on the action slip

### 5.Combine Same Item Based On Item Sorting Method 1

#### 5.1.Guest check does not combine items with different open description modifiers when using sorting method 1

**Prerequisite(s):**
1. The build containing the HERO-65092 fix is deployed to the test POS environment
2. The guest check print format is configured with Item Grouping Method = "Combine same item based on item sorting method 1"
3. The item used for testing is configured with one of the open description Input Item Name options as in scenario 1.1

**Step(s):**
1. Open a new check on the POS workstation
2. Order one item with an open description modifier and enter description "a1"
3. Order another new item which is the same item as in step 2
4. Order an open description modifier and enter a different description "b2"
5. Click function "Print and Paid"
6. Inspect the printed guest check

**Reproduction Result(s):**
1. The printed guest check wrongly combines the two items with different open description modifiers into a single item line, the same behaviour as the "Combine same item to one line" grouping method

**Fix Result(s):**
1. The printed guest check prints the two items on separate lines
2. The fix applies to both "Combine same item to one line" and "Combine same item based on item sorting method 1" grouping methods

### 6.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Guest check and receipt printing for items without open description modifiers
- Guest check and receipt printing for items with the same open description modifier
- Action slip printing for pantry message for items without open description modifiers
- Set menu printing for child items without open description modifiers
- All four Input Item Name options (Allow to change item name, Allow to append text after item name, Allow to append text after item name (By click panel button), Allow to change item name and append text after item name)
- Both Item Grouping Method options ("Combine same item to one line" and "Combine same item based on item sorting method 1")
- Reprint and reprint-after-settlement workflows for checks containing items with open description modifiers
