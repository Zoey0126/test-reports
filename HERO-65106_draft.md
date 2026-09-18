# Display Control (Kitchen Display) - Quantity for items with open description modifier increased wrongly after applying pantry message Test Report

## Functional Testing

### 1.Pantry Message Setup Verification

#### 1.1.Action print queue and print format configuration for pantry message

**Prerequisite(s):**
1. The build containing the HERO-65106 fix is deployed to the test POS environment
2. Access to Infrasys POS platform POS System > Printing Setup is available
3. A kitchen display (display control) is configured and reachable

**Step(s):**
1. Go to POS System > Printing Setup > Action Print Queues and add or open an action print queue record
2. Set Action Type = "Pantry Message" and select the target action slip print format
3. Save the record
4. Go to POS System > Printing Setup > Print Formats and open the action slip print format
5. Set Item Grouping Method = "Combine same item to one line"
6. Save the record

**Reproduction Result(s):**
1. This is a setup prerequisite and is not directly affected by the bug

**Fix Result(s):**
1. The action print queue and action slip print format are saved successfully with the configured values
2. The pantry message action type and the combine same item to one line grouping method are persisted correctly

### 2.Item With Open Description Modifier

#### 2.1.Quantity and pantry message display on kitchen display for items with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65106 fix is deployed to the test POS environment
2. Pantry message action print queue is configured as in scenario 1.1
3. An item that supports open description modifier is available for ordering (for example Apple Juice with modifier Drink - Sugar 0%)
4. The kitchen display (display control) is online and visible

**Step(s):**
1. Open a new check on the POS workstation
2. Order 2 of the same item separately, each with an open description modifier, and enter different descriptions for the modifiers (for example Item 1: Apple Juice, modifier: Drink - Sugar 0%11, quantity 1; Item 2: Apple Juice, modifier: Drink - Sugar 0%22, quantity 1)
3. Click function "Send Check"
4. Check the kitchen display screen and verify that 2 separate items are displayed with the correct quantity of 1 each
5. Return to the POS workstation and open the check that was sent
6. Click function "Pantry Message"
7. Select the target pantry message and apply it to both items
8. Check the kitchen display screen and verify the item quantity and the pantry message display for both items

**Reproduction Result(s):**
1. After applying the pantry message, the kitchen display screen shows the wrong quantity for the first item, increasing from 1 to 2
2. The pantry message is only displayed for the first item on the kitchen display
3. The second item does not show the pantry message even though it was selected

**Fix Result(s):**
1. The kitchen display screen displays both items with the correct quantity of 1 each
2. The pantry message is displayed for both items as selected, for example "Apple Juice, modifier: Drink - Sugar 0%11, quantity: 1, <Pantry message>" and "Apple Juice, modifier: Drink - Sugar 0%22, quantity: 1, <Pantry message>"
3. The items with different open description modifiers are not combined into a single item on the kitchen display

#### 2.2.Action slip does not combine items with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65106 fix is deployed to the test POS environment
2. Pantry message action print queue is configured as in scenario 1.1
3. The action slip print format has Item Grouping Method = "Combine same item to one line"
4. An item that supports open description modifier is available for ordering

**Step(s):**
1. Open a new check on the POS workstation
2. Order 2 of the same item separately, each with an open description modifier with different descriptions (for example Item 1: Apple Juice, modifier: Drink - Sugar 0%11; Item 2: Apple Juice, modifier: Drink - Sugar 0%22)
3. Click function "Send Check"
4. Open the check that was sent and click function "Pantry Message"
5. Select the target pantry message and apply it to both items
6. Inspect the printed action slip

**Reproduction Result(s):**
1. The printed action slip wrongly combines the two items with different open description modifiers into a single item line
2. The two different modifiers are merged into one modifier on the action slip

**Fix Result(s):**
1. The printed action slip prints the two items on separate lines
2. Each item is printed with its own open description modifier and the selected pantry message, for example "Apple Juice, modifier: Drink - Sugar 0%11, quantity: 1, <Pantry message>" and "Apple Juice, modifier: Drink - Sugar 0%22, quantity: 1, <Pantry message>"
3. The system does not combine items with different open description modifiers into one item on the action slip

### 3.Set Menu With Child Item Open Description Modifier

#### 3.1.Quantity and pantry message display on kitchen display for set menu items with child item open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65106 fix is deployed to the test POS environment
2. Pantry message action print queue is configured as in scenario 1.1
3. A set menu with a child item that supports open description modifier is available for ordering (for example Test Set Menu with child item Apple and child item modifier Drink - Sugar 0%)
4. The kitchen display (display control) is online and visible

**Step(s):**
1. Open a new check on the POS workstation
2. Order 2 of the same set menu item separately, each with a child item open description modifier, and enter different descriptions for the child item modifiers (for example Set Menu 1: child item Apple, modifier: Drink - Sugar 0%11, quantity 1; Set Menu 2: child item Apple, modifier: Drink - Sugar 0%22, quantity 1)
3. Click function "Send Check"
4. Check the kitchen display screen and verify that 2 separate set menu items are displayed with the correct quantity of 1 each
5. Return to the POS workstation and open the check that was sent
6. Click function "Pantry Message"
7. Select the target pantry message and apply it to both set menu items
8. Check the kitchen display screen and verify the set menu item quantity, the child item quantity and the pantry message display for both set menu items and their child items

**Reproduction Result(s):**
1. After applying the pantry message, the kitchen display screen shows the wrong quantity for the first set menu item and its child item, increasing from 1 to 2
2. The pantry message is only displayed for the first set menu item and its child item on the kitchen display

**Fix Result(s):**
1. The kitchen display screen displays both set menu items with the correct quantity of 1 each
2. The child items are also displayed with the correct quantity of 1 each
3. The pantry message is displayed for both set menu items and their child items, for example "Test Set Menu, quantity: 1, <Pantry message>" and child item "Apple, modifier: Drink - Sugar 0%11, quantity: 1, <Pantry message>" for the first set menu, and "Test Set Menu, quantity: 1, <Pantry message>" and child item "Apple, modifier: Drink - Sugar 0%22, quantity: 1, <Pantry message>" for the second set menu

#### 3.2.Action slip does not combine child items with different open description modifiers

**Prerequisite(s):**
1. The build containing the HERO-65106 fix is deployed to the test POS environment
2. Pantry message action print queue is configured as in scenario 1.1
3. The action slip print format has Item Grouping Method = "Combine same item to one line"
4. A set menu with a child item that supports open description modifier is available for ordering

**Step(s):**
1. Open a new check on the POS workstation
2. Order 2 of the same set menu item separately, each with a child item open description modifier with different descriptions
3. Click function "Send Check"
4. Open the check that was sent and click function "Pantry Message"
5. Select the target pantry message and apply it to both set menu items
6. Inspect the printed action slip

**Reproduction Result(s):**
1. The printed action slip wrongly combines the child items of the two set menu items with different open description modifiers into a single item line
2. The two different child item modifiers are merged into one modifier on the action slip

**Fix Result(s):**
1. The printed action slip prints the two set menu items separately
2. Each child item is printed with its own open description modifier and the selected pantry message, and child items with different open description modifiers are not combined into one item

### 4.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Kitchen display (display control) for items and set menu items without open description modifiers after applying pantry message
- Kitchen display (display control) for items and set menu items with the same open description modifier after applying pantry message
- Action slip print format with Item Grouping Method = "Combine same item to one line" for items without open description modifiers
- Action slip print format with Item Grouping Method = "Combine same item based on item sorting method 1"
- Pantry message workflow for checks with mixed item types (items with and without open description modifiers)
- Send Check and reprint workflows for checks containing items with open description modifiers
