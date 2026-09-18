# POS Feature - Enable "Item Reminder" on mobile/workstation view (Dev) Test Report

## Functional Testing

### 1.Item Reminder Setup Configuration

#### 1.1.Configure Item Remind Rules
**Prerequisite(s):**
1. The Infrasys POS platform is accessible
2. Items or menu types are configured for the target shop or outlet

**Step(s):**
1. Go to Infrasys POS platform: POS System -> Ordering Setup -> Item Remind Rules
2. Click "Add New" to add a new record or edit an existing record
3. Set Available in Locations to the target Shop or Outlet
4. Set Item / Menu to the item or menu type the rule applies to
5. Set Available Options to the target item or menu the rule applies to
6. Set Suggestion to "Force to Order" or "Suggest to Order"
7. Fill in the remaining details
8. Click "Save" to save the record

**Test Result(s):**
1. The Item Remind Rule is saved successfully with the selected configuration
2. The rule is available for the selected locations, items and suggestion type
3. The rule is triggered when the related function keys are used

### 2.Item Reminder In Horizontal View

#### 2.1.Force To Order Rule In Horizontal View Day Mode
**Prerequisite(s):**
1. At least one "Force to Order" Item Remind Rule is configured
2. The POS station is in horizontal desktop view with day mode enabled
3. A table is open with at least one unsatisfied force to order rule

**Step(s):**
1. Open a table
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the Item Reminder page and the ordering basket
4. Click the cart icon of the target item
5. Verify that the target item is added to the ordering basket
6. Click the "Send Or Print Check" button

**Test Result(s):**
1. The system shows the Item Reminder page and the ordering basket with the "Send Or Print Check" button and the "Must Select" section
2. Clicking the cart icon of the target item adds the item to the ordering basket
3. Clicking the "Send Or Print Check" button returns to step 3 to re-evaluate the unsatisfied rules

#### 2.2.Suggest To Order Rule In Horizontal View Day Mode
**Prerequisite(s):**
1. At least one "Suggest to Order" Item Remind Rule is configured
2. The POS station is in horizontal desktop view with day mode enabled
3. A table is open with at least one unsatisfied suggest to order rule and no unsatisfied force to order rules

**Step(s):**
1. Open a table
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the Item Reminder page
4. Click the cart icon of the target item
5. Verify that the target item is added to the ordering basket
6. Click the "Ignore And Send" button

**Test Result(s):**
1. The system shows the Item Reminder page with the "Ignore And Send" button and the "Advice To Select" section
2. Clicking the cart icon of the target item adds the item to the ordering basket
3. Clicking the "Ignore And Send" button proceeds with the existing behavior to continue the function

#### 2.3.Item Reminder In Horizontal View Night Mode
**Prerequisite(s):**
1. At least one Item Remind Rule is configured (force to order and suggest to order)
2. The POS station is in horizontal desktop view with night mode enabled

**Step(s):**
1. Switch the POS to night mode
2. Open a table
3. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
4. Verify that the Item Reminder page is displayed in night mode UI
5. Repeat the workflows for force to order and suggest to order rules

**Test Result(s):**
1. The Item Reminder page is displayed with the night mode UI in horizontal view
2. The force to order and suggest to order workflows operate correctly in night mode
3. The night mode UI matches the design specification for horizontal view

### 3.Item Reminder In Vertical Mobile View

#### 3.1.Switch Between Item List And Ordering Basket In Vertical Mobile View
**Prerequisite(s):**
1. At least one Item Remind Rule is configured
2. The POS station is in vertical mobile view with day mode enabled
3. A table is open with at least one unsatisfied rule

**Step(s):**
1. Open a table
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the Item Reminder page and the button to switch between item list and ordering basket
4. Click the "Ordered Item" button to switch to the ordering basket
5. Click the "Item List" button to switch back to the item list

**Test Result(s):**
1. The system shows the Item Reminder page with a button to switch between item list and ordering basket
2. Clicking the "Ordered Item" button shows the ordering basket
3. Clicking the "Item List" button shows the item list of the rule

#### 3.2.Force To Order Rule In Vertical Mobile View Day Mode
**Prerequisite(s):**
1. At least one "Force to Order" Item Remind Rule is configured
2. The POS station is in vertical mobile view with day mode enabled
3. A table is open with at least one unsatisfied force to order rule

**Step(s):**
1. Open a table
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the Item Reminder page with the "Send Or Print Check" button and the "Must Select" section
4. Click the cart icon of the target item
5. Verify that the target item is added to the ordering basket
6. Click the "Send Or Print Check" button

**Test Result(s):**
1. The system shows the Item Reminder page with the "Send Or Print Check" button and the "Must Select" section
2. Clicking the cart icon of the target item adds the item to the ordering basket
3. Clicking the "Send Or Print Check" button returns to step 3 to re-evaluate the unsatisfied rules

#### 3.3.Suggest To Order Rule In Vertical Mobile View Day Mode
**Prerequisite(s):**
1. At least one "Suggest to Order" Item Remind Rule is configured
2. The POS station is in vertical mobile view with day mode enabled
3. A table is open with at least one unsatisfied suggest to order rule and no unsatisfied force to order rules

**Step(s):**
1. Open a table
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the Item Reminder page with the "Ignore And Send" button and the "Advice To Select" section
4. Click the cart icon of the target item
5. Verify that the target item is added to the ordering basket
6. Click the "Ignore And Send" button

**Test Result(s):**
1. The system shows the Item Reminder page with the "Ignore And Send" button and the "Advice To Select" section
2. Clicking the cart icon of the target item adds the item to the ordering basket
3. Clicking the "Ignore And Send" button proceeds with the existing behavior to continue the function

#### 3.4.Item Reminder In Vertical Mobile View Night Mode
**Prerequisite(s):**
1. At least one Item Remind Rule is configured (force to order and suggest to order)
2. The POS station is in vertical mobile view with night mode enabled

**Step(s):**
1. Switch the POS to night mode in vertical mobile view
2. Open a table
3. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
4. Verify that the Item Reminder page is displayed in night mode UI for both item list and ordered item layouts
5. Repeat the workflows for force to order and suggest to order rules

**Test Result(s):**
1. The Item Reminder page is displayed with the night mode UI in vertical mobile view for both item list and ordered item layouts
2. The force to order and suggest to order workflows operate correctly in night mode
3. The night mode UI matches the design specification for vertical mobile view

### 4.No Unsatisfied Rules Behavior

#### 4.1.Continue Function When No Unsatisfied Rules
**Prerequisite(s):**
1. Item Remind Rules are configured but all rules are satisfied for the open table
2. The POS station is in horizontal or vertical mobile view

**Step(s):**
1. Open a table with all Item Remind Rules satisfied
2. Click function "Send Check", "Print Check" or "Print And Paid" depending on Show Reminder Settings
3. Observe the system behavior

**Test Result(s):**
1. The system follows the existing behavior to continue the function without showing the Item Reminder page
2. No Item Reminder page or button is displayed when all rules are satisfied

### 5.Regression Scope

The following related modules and scenarios must be retested after the change:

- Item Remind Rules configuration on the Infrasys POS platform
- Item Reminder workflow in horizontal view day mode and night mode
- Item Reminder workflow in vertical mobile view day mode and night mode
- Force to Order and Suggest to Order rule types in both views
- Existing Item Reminder behavior on horizontal desktop view remains unchanged
- Send Check, Print Check and Print And Paid function keys with Show Reminder Settings
