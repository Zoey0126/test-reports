# HERO-69087 BYOD Module - Add Takeaway Box Setup - API Test Report

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
2. The listing groups takeaway boxes by outlet (similar to the Mix and Match Rules page).
3. The selected item is set as the takeaway box item with its Name and Code shown.

#### 1.2.Edit the contents (items bound to a takeaway box)

**Prerequisite(s):**
1. At least one takeaway box has been added under BYOD Management > Takeaway Box Settings.
2. The selected outlet has multiple items in its API Menu.

**Step(s):**
1. Navigate to BYOD Management > Takeaway Box Settings.
2. Locate the takeaway box entry and click "Edit Content".
3. Verify only items within the selected outlet's API Menu (POS Module > API Settings > Outlet Menu Settings) are shown.
4. Move available food items from the Available list to the Selected list.
5. Click "Save".
6. Re-open the takeaway box and verify the bound items are listed.
7. Click the expand (+) button beside the box name to verify the bound foods are shown.

**Test Result(s):**
1. The bound items are saved correctly and listed when expanding the takeaway box.
2. Only items within the selected outlet's API Menu are available for binding.
3. The edit content operation updates the bound items without affecting other takeaway boxes.

#### 1.3.Delete an existing Takeaway Box entry

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

### 2.Uniqueness Rule for Food Item Mapping

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

**Test Result(s):**
1. The system prevents Food Item X from being bound to two different takeaway boxes within the same outlet.
2. A clear validation message is shown when the user attempts to map the same food item to a second takeaway box.
3. Existing bindings to Takeaway Box A are preserved.

### 3.Takeaway Box Settings Sync API - GET /outlet_takeaway_boxes

#### 3.1.Sync takeaway box settings to OGS via the menu sync API

**Prerequisite(s):**
1. Takeaway Box settings have been configured for at least one outlet under BYOD Management > Takeaway Box Settings.
2. Menu sync to OGS is enabled for the relevant outlets.
3. The BYODGAPI endpoint is reachable.

**Step(s):**
1. Trigger menu sync to OGS for an outlet with configured takeaway box settings.
2. Call the GET /outlet_takeaway_boxes endpoint with the configured outlet code.
3. Verify the sync request includes the takeaway box item and bound food items.
4. On the OGS side, verify the takeaway box and bound items are received.

**Test Result(s):**
1. The menu sync successfully delivers the takeaway box item and bound items to OGS.
2. The API returns the takeaway box settings for the configured outlet.
3. The takeaway box and its bound items are visible in OGS after sync.

#### 3.2.API returns takeaway box settings for a single outlet

**Prerequisite(s):**
1. Takeaway Box settings have been configured for at least one BYOD-enabled outlet.
2. The outlet has an active takeaway box configuration with active allowed food items.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with a single valid outlet_codes parameter (e.g., 1001).
2. Call the API with a single valid languages parameter (e.g., en).
3. Verify the response status and data structure.

**Test Result(s):**
1. The API returns a "success" status.
2. The response data contains the outlet with its code, shop_code, and localized name.
3. The outlet's boxes array contains the configured takeaway box with code, localized name, last_modified_time, and bound items.

#### 3.3.API returns takeaway box settings for multiple outlets

**Prerequisite(s):**
1. Takeaway Box settings have been configured for at least two BYOD-enabled outlets.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with multiple outlet_codes separated by commas (e.g., 1001,1002,1003).
2. Call the API with a single or multiple languages parameter (e.g., en,zh-Hant).
3. Verify the response status and data structure for each outlet.

**Test Result(s):**
1. The API returns a "success" status.
2. The response data contains all the requested outlets that have active takeaway box configurations.
3. Each outlet in the response shows its boxes and bound items with the requested language localization.

### 4.API Response Data and Localization

#### 4.1.Enterprise config zone item name override is supported per outlet shop

**Prerequisite(s):**
1. Takeaway Box settings have been configured for an outlet.
2. Enterprise config zone item name override is configured for the outlet's shop.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with the outlet code of the shop that has a config zone item name override.
2. Verify the localized name of the takeaway box item and bound food items in the response.
3. Compare the returned item names with the config zone override values.

**Test Result(s):**
1. The API returns the item names overridden by the enterprise config zone for the outlet's shop.
2. The localized names reflect the config zone override rather than the default item names.
3. The override is applied per outlet shop correctly.

#### 4.2.Only BYOD-enabled outlets with active takeaway box configuration are returned

**Prerequisite(s):**
1. At least one BYOD-enabled outlet with active takeaway box configuration exists.
2. At least one non-BYOD outlet or an outlet without configured boxes exists.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with outlet codes that include both BYOD-enabled and non-BYOD outlets.
2. Verify the response data for the outlets returned.

**Test Result(s):**
1. Only BYOD-enabled outlets with active takeaway box configuration are returned in the response.
2. Non-BYOD outlets are omitted from the response.
3. Outlets without configured boxes are omitted from the response.

#### 4.3.Outlets without active allowed food items are omitted

**Prerequisite(s):**
1. A takeaway box exists for an outlet but has no active allowed food items bound to it.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with the outlet code of the outlet whose takeaway box has no active allowed food items.
2. Verify the response data for that outlet.

**Test Result(s):**
1. The outlet whose takeaway boxes have no active allowed food items is omitted from the response.
2. The response only includes outlets with at least one active takeaway box containing active allowed food items.

### 5.API Error Handling

#### 5.1.Invalid or non-BYOD outlet codes are silently skipped

**Prerequisite(s):**
1. At least one valid BYOD-enabled outlet with takeaway box configuration exists.
2. An invalid or non-BYOD outlet code is available for testing.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with a mix of valid and invalid/non-BYOD outlet codes (e.g., 1001,INVALID,9999).
2. Verify the response status and data.

**Test Result(s):**
1. The API returns a "success" status.
2. Invalid or non-BYOD outlet codes are silently skipped and do not cause an error.
3. The response only includes data for the valid BYOD-enabled outlets with active takeaway box configuration.

#### 5.2.Missing outlet_codes parameter returns an error

**Prerequisite(s):**
1. The BYODGAPI endpoint is reachable.

**Step(s):**
1. Call GET /outlet_takeaway_boxes without the outlet_codes parameter.
2. Verify the response status code and error message.

**Test Result(s):**
1. The API returns a status code 400.
2. The response status is "fail" with the message "missing_outlet_code".
3. The error message indicates the "outlet_codes" parameter is missing in the API URL.

#### 5.3.Missing languages parameter returns an error

**Prerequisite(s):**
1. The BYODGAPI endpoint is reachable.

**Step(s):**
1. Call GET /outlet_takeaway_boxes with a valid outlet_codes parameter but without the languages parameter.
2. Verify the response status code and error message.

**Test Result(s):**
1. The API returns a status code 400.
2. The response status is "fail" with the message "missing_languages".
3. The error message indicates the "languages" parameter is missing in the API params.

### 6.OGS Auto-Add Takeaway Box on Order

#### 6.1.Ordering a food item bound to a takeaway box auto-adds the takeaway box

**Prerequisite(s):**
1. Takeaway Box settings have been configured and successfully synced to OGS for an outlet.
2. A food item is bound to a takeaway box for the outlet.

**Step(s):**
1. In OGS, place an order for a food item that is bound to a takeaway box.
2. Verify the order content after adding the food item.

**Test Result(s):**
1. The system auto-adds the takeaway box to the order when the bound food item is ordered.
2. The takeaway box appears in the order with the correct item code and name.
3. The auto-added takeaway box is associated with the correct outlet.

#### 6.2.Ordering a food item not bound to any takeaway box does not add a takeaway box

**Prerequisite(s):**
1. Takeaway Box settings have been synced to OGS for an outlet.
2. A food item exists that is not bound to any takeaway box.

**Step(s):**
1. In OGS, place an order for a food item that is not bound to any takeaway box.
2. Verify the order content after adding the food item.

**Test Result(s):**
1. No takeaway box is auto-added to the order when a non-bound food item is ordered.
2. The order only contains the ordered food item without any unexpected takeaway box.

### 7.Sync Impact and Consistency Verification

#### 7.1.Takeaway Box settings remain consistent after multiple sync cycles

**Prerequisite(s):**
1. Takeaway Box settings have been configured and successfully synced to OGS once.

**Step(s):**
1. Trigger menu sync to OGS again for the same outlet.
2. Verify the takeaway box settings on OGS remain unchanged.
3. Call GET /outlet_takeaway_boxes and compare the response with the previous sync.

**Test Result(s):**
1. The takeaway box settings on OGS remain consistent after multiple sync cycles.
2. The API response is identical to the previous sync for the same outlet.
3. No duplicate or missing entries are introduced by repeated syncs.
