# HERO-68925 Cannot show item reminder page after adding set-menu item Test Report

## Functional Testing

### 1.Reproduction - Item Reminder Page Blocked After Adding Set-Menu Item

#### 1.1.Item reminder page fails to show after adding a set-menu item and selecting child items

**Prerequisite(s):**
1. The POS client is running and accessible.
2. An item reminder rule is configured with a set-menu item:
   - Location: POS System > Ordering Setup > Item Reminder Rules
   - Setup name: Available Options
   - Select a set-menu item as the target
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Send Check", "Print Check", or "Print And Paid" (depending on Show Reminder Settings).
3. Observe the system displays the items reminder page (because an unsatisfied rule exists).
4. Click the cart icon of a set-menu item on the reminder page.
5. Observe the system displays the child-item selection page.
6. Add/select child items.
7. Click the button "OK".

**Reproduction Result(s):**
1. After clicking "OK" on the child-item selection page, the system fails to show the items reminder page.
2. An overlay cover blocks all further operations on the Item Reminder page.
3. The user cannot continue interacting with the Item Reminder page or proceed with the check flow.

### 2.Fix Verification - Item Reminder Page Restoration

#### 2.1.Item reminder page returns to foreground after adding a set-menu item via Send Check

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. An item reminder rule is configured with a set-menu item at POS System > Ordering Setup > Item Reminder Rules.
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Send Check".
3. Observe the system displays the items reminder page.
4. Click the cart icon of the set-menu item on the reminder page.
5. On the child-item selection page, select the target child items.
6. Click the button "OK".

**Fix Result(s):**
1. After clicking "OK", the system brings the Item Reminder page back to the foreground.
2. The Item Reminder page is shown above the overlay cover, allowing the user to continue operation.
3. No overlay remains blocking further operations.
4. The user can interact with the Item Reminder page normally.

#### 2.2.Item reminder page returns to foreground after adding a set-menu item via Print Check

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. An item reminder rule is configured with a set-menu item.
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Print Check".
3. Observe the system displays the items reminder page.
4. Click the cart icon of the set-menu item on the reminder page.
5. On the child-item selection page, select the target child items.
6. Click the button "OK".

**Fix Result(s):**
1. After clicking "OK", the Item Reminder page returns to the foreground.
2. The page is shown above the overlay cover without blocking.
3. The user can continue operating the Item Reminder page.

#### 2.3.Item reminder page returns to foreground after adding a set-menu item via Print And Paid

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. An item reminder rule is configured with a set-menu item.
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Print And Paid".
3. Observe the system displays the items reminder page.
4. Click the cart icon of the set-menu item on the reminder page.
5. On the child-item selection page, select the target child items.
6. Click the button "OK".

**Fix Result(s):**
1. After clicking "OK", the Item Reminder page returns to the foreground.
2. The page is shown above the overlay cover without blocking.
3. The user can continue operating the Item Reminder page and complete the Print And Paid flow.

### 3.Item Reminder Rule Configuration

#### 3.1.Item reminder rule with set-menu item is configured correctly

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. Access to Infrasys POS platform: POS System > Ordering Setup > Item Remind Rules.

**Step(s):**
1. Go to POS System > Ordering Setup > Item Remind Rules.
2. Click "Add New" to add a new record.
3. Select Available in Locations = Target Shop or Outlet.
4. Select Item / Menu = Item type the rule applies to.
5. Select Available Options = Target set-menu item.
6. Select Suggestion = Suggest to Order or Force to Order.
7. Fill in the remaining details.
8. Click "Save".
9. Open a table and trigger the reminder (Send Check, Print Check, or Print And Paid).

**Fix Result(s):**
1. The item reminder rule is saved successfully with the set-menu item configured.
2. When the reminder is triggered, the Item Reminder page displays the set-menu item.
3. The rule configuration does not cause the overlay blocking issue after the fix.

### 4.Multiple Set-Menu Items on Reminder Page

#### 4.1.Item reminder page handles multiple set-menu items sequentially

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. Two or more item reminder rules are configured with different set-menu items.
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Send Check" or "Print Check".
3. Observe the system displays the items reminder page with multiple set-menu items.
4. Click the cart icon of the first set-menu item.
5. Select child items and click "OK".
6. Verify the Item Reminder page returns to the foreground.
7. Click the cart icon of the second set-menu item.
8. Select child items and click "OK".
9. Verify the Item Reminder page returns to the foreground again.

**Fix Result(s):**
1. After adding the first set-menu item's child items, the Item Reminder page returns to the foreground.
2. After adding the second set-menu item's child items, the Item Reminder page returns to the foreground again.
3. The user can process multiple set-menu items sequentially without any overlay blocking.

### 5.Regression - Non-Set-Menu Item Reminder

#### 5.1.Item reminder page works correctly for non-set-menu items

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. An item reminder rule is configured with a non-set-menu (regular) item.
3. A table is available for ordering.

**Step(s):**
1. Open a table.
2. Click the function "Send Check" or "Print Check".
3. Observe the system displays the items reminder page with the regular item.
4. Add the regular item from the reminder page.
5. Verify the Item Reminder page continues to function without issues.

**Fix Result(s):**
1. The Item Reminder page works correctly for non-set-menu items.
2. Adding a regular item from the reminder page does not cause any overlay blocking.
3. No regression is introduced for non-set-menu item reminder flows.

#### 5.2.Item reminder page close and continue flow works after adding set-menu item

**Prerequisite(s):**
1. The build containing the HERO-68925 fix is deployed to the POS environment.
2. An item reminder rule is configured with a set-menu item.
3. A table is available for ordering and a set-menu item has been added via the reminder page.

**Step(s):**
1. Open a table and trigger the Item Reminder page (Send Check or Print Check).
2. Add the set-menu item and select child items, then click "OK".
3. Verify the Item Reminder page returns to the foreground.
4. Close the Item Reminder page (e.g., click "Close" or proceed).
5. Verify the check flow continues normally (e.g., the check is sent or printed).

**Fix Result(s):**
1. After adding the set-menu item, the Item Reminder page returns to the foreground and can be closed normally.
2. The check flow (Send Check, Print Check, or Print And Paid) continues without errors after the reminder is resolved.
3. No overlay or blocking remains after the Item Reminder page is closed.
