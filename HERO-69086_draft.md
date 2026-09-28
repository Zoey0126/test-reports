# BYOD Module - Add Takeaway Box Setup - Dev Test Report

## Functional Testing

### 1.Takeaway Box Setting Setup in POS Backend

#### 1.1.Add new Takeaway Box under BYOD Management

**Prerequisite(s):**
1. User has access to POS Backend with permission to manage BYOD Module.
2. At least one outlet exists in the system.
3. The selected outlet has an API Menu configured at POS Module > API Settings > Outlet Menu Settings.
4. At least one item is available in the selected outlet's API Menu.

**Step(s):**
1. Log in to POS Backend.
2. Navigate to BYOD Management.
3. Open the "Takeaway Box Settings" tab.
4. Click "Add New".
5. Select the target Outlet.
6. Select the Item that will be the takeaway box item (within the selected outlet's API Menu).
7. Click "Save".
8. Verify the new takeaway box appears in the listing grouped by outlet.

**Test Result(s):**
1. The new takeaway box is successfully saved and appears in the listing.
2. The listing groups takeaway boxes by outlet.
3. The selected item is set as the takeaway box item.

#### 1.2.Edit the contents (items bound to a takeaway box)

**Prerequisite(s):**
1. At least one takeaway box has been added under BYOD Management > Takeaway Box Settings.
2. The selected outlet has multiple items in its API Menu.

**Step(s):**
1. Navigate to BYOD Management > Takeaway Box Settings.
2. Locate the takeaway box entry and click "Edit Content".
3. Move available food items from the Available list to the Selected list.
4. Click "Save".
5. Re-open the takeaway box and verify the bound items are listed.
6. Click the expand (+) button beside the box name to verify the bound foods are shown.
7. Search the listing by item name and verify the bound items can be searched.

**Test Result(s):**
1. The bound items are saved correctly and listed when expanding the takeaway box.
2. Only items within the selected outlet's API Menu are available for binding.
3. The search function correctly locates takeaway boxes by item name.
4. The edit content operation updates the bound items without affecting other takeaway boxes.

### 2.Uniqueness Rule

#### 2.1.One food item cannot be mapped to more than one takeaway box within the same outlet

**Prerequisite(s):**
1. At least two takeaway boxes exist for the same outlet.
2. A shared API Menu is configured for the outlet.

**Step(s):**
1. Navigate to BYOD Management > Takeaway Box Settings.
2. Open Takeaway Box A and bind Food Item X.
3. Save and verify the binding succeeds.
4. Open Takeaway Box B (same outlet) and try to bind the same Food Item X.
5. Observe the system response when attempting to bind.
6. Try binding through the API menu sync flow as well.

**Test Result(s):**
1. The system prevents Food Item X from being bound to two different takeaway boxes within the same outlet.
2. A clear validation message is shown when the user attempts to map the same food item to a second takeaway box.
3. Existing bindings to Takeaway Box A are preserved.

### 3.Delete Takeaway Box

#### 3.1.Delete an existing Takeaway Box entry

**Prerequisite(s):**
1. At least one takeaway box exists under BYOD Management > Takeaway Box Settings.

**Step(s):**
1. Navigate to BYOD Management > Takeaway Box Settings.
2. Locate a takeaway box entry and click "Delete".
3. Confirm the deletion in the confirmation dialog.
4. Verify the takeaway box is removed from the listing.
5. Verify any bound items are released and can be bound to other takeaway boxes.

**Test Result(s):**
1. The takeaway box is successfully deleted and no longer appears in the listing.
2. Items previously bound to the deleted takeaway box are released.
3. No error is thrown during deletion.

### 4.Menu Sync to OGS

#### 4.1.Takeaway Box settings are synced to OGS via the menu sync API

**Prerequisite(s):**
1. Takeaway Box settings have been configured for at least one outlet under BYOD Management > Takeaway Box Settings.
2. Menu sync to OGS is enabled for the relevant outlets.

**Step(s):**
1. Navigate to the menu sync page or trigger menu sync to OGS.
2. Initiate a menu sync for an outlet with configured takeaway box settings.
3. Verify the sync request includes the takeaway box item and bound food items.
4. On the OGS side, verify the takeaway box and bound items are received.
5. Trigger an order in OGS for an item bound to a takeaway box and confirm the system auto-adds the takeaway box to the order.

**Test Result(s):**
1. The menu sync successfully delivers the takeaway box item and bound items to OGS.
2. The takeaway box and its bound items are visible in OGS after sync.
3. Ordering a food item bound to a takeaway box in OGS automatically adds the takeaway box to the order.

### 5.Sync Impact Verification

#### 5.1.Takeaway Box settings remain consistent after multiple sync cycles

**Prerequisite(s):**
1. Takeaway Box settings have been configured and successfully synced to OGS once.

**Step(s):**
1. Trigger menu sync to OGS again for the same outlet.
2. Verify the takeaway box settings on OGS remain unchanged.
3. Edit the bound items of a takeaway box in POS Backend.
4. Trigger menu sync to OGS again.
5. Verify OGS reflects the updated bound items.

**Test Result(s):**
1. Multiple menu sync cycles do not duplicate or lose takeaway box settings.
2. Edits to the bound items are correctly reflected on OGS after the next sync.

## Test Environment Information

- **Test Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
- **Related Version**: Infrasys POS Backend v1.2.115.0
- **Test Browsers**: Chrome, Firefox, Microsoft Edge

## Appendix

### Acceptance Criteria (from JIRA)

New Functions: [Jira-69086] BYOD Module - Takeaway Box Setting

Description:
Adds Takeaway Box Settings under BYOD Management.
Set one takeaway box item per outlet, then bind foods from that outlet's API menu. One food can belong to only one box.
The list is grouped by outlet, with edit, delete, search, and expand to show bound foods.

Setup:
Deploy HERO version 1.2.115.0.
Go to [BYOD Management > Takeaway Box Settings].
On the listing page, click + beside the box name to show bound foods.
On the add page, select the outlet and the takeaway box item, then Save.
On the edit page, move foods between Available and Selected, then Save. Delete removes the box.

A new API is required to sync the takeaway box settings to OGS.