# POS - Allow KDS to trigger POS mark delivery for printing - Dev (QA Testing) Test Report

## Functional Testing

### 1.New API Request for Mark Delivery

#### 1.1.Successful Mark Delivery for a Single Old Item

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. A Shiji KDS interface is attached to the target outlet.
4. A check exists with at least one old (sent) item that has not yet been marked as delivered.
5. The internal check item ID of the target item is known.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table with one internal check item ID.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The API returns a successful response indicating that the item was marked as delivered.
2. The item status in the check is updated to "delivered".
3. No errors displayed.

#### 1.2.Successful Mark Delivery for Multiple Old Items in One Request

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. A Shiji KDS interface is attached to the target outlet.
4. A check exists with multiple old (sent) items that have not been marked as delivered.
5. The internal check item IDs of the target items are known.

**Step(s):**
1. Send a single mark delivery request to the API portal for the target table with multiple internal check item IDs.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The API returns a successful response indicating that all requested items were marked as delivered.
2. All requested item statuses in the check are updated to "delivered".
3. No errors displayed.

#### 1.3.Mark Delivery for All Items of a Check When No Items Are Given

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. A Shiji KDS interface is attached to the target outlet.
4. A check exists with multiple old (sent) items that have not been marked as delivered.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table without providing any item IDs.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The API returns a successful response indicating that all items of the check were marked as delivered.
2. All item statuses in the check are updated to "delivered".
3. No errors displayed.

#### 1.4.Only Old Items Are Marked as Delivered

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible with valid API credentials.
3. A Shiji KDS interface is attached to the target outlet.
4. A check exists with both old (sent) items and new (unsent) items.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table without providing any item IDs (mark all items flow).
2. Verify which items are marked as delivered.

**Test Result(s):**
1. Only old (sent) items are marked as delivered.
2. New (unsent) items are NOT marked as delivered.
3. No errors displayed.

### 2.Table State Validation

#### 2.1.Target Table Not Occupied

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. The target table is not occupied (no check exists for the table).

**Step(s):**
1. Send a mark delivery request to the API portal for the unoccupied target table.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system returns an error message indicating the target table is not occupied.
2. The action is aborted and no mark delivery is performed.
3. No errors displayed.

#### 2.2.Target Table Locked by Others

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. The target table is locked by another user/process.

**Step(s):**
1. Send a mark delivery request to the API portal for the locked target table.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system returns the existing error message indicating the table is locked by others.
2. The action is aborted and no mark delivery is performed.
3. No errors displayed.

#### 2.3.Target Table Opened Successfully

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. The target table is occupied and not locked.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table.
2. Verify the table opening step.

**Test Result(s):**
1. The system opens the target table successfully.
2. The mark delivery workflow proceeds to the next step.
3. No errors displayed.

### 3.Item Identification

#### 3.1.Shiji KDS Interface Attached - Use Internal Check Item ID

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. A Shiji KDS interface is attached to the target outlet.
3. A check exists with old items.
4. The internal check item IDs of the target items are known.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table with the internal check item IDs.
2. Verify which identifier is used to locate the items.

**Test Result(s):**
1. The system uses the provided internal check item ID to identify the target items.
2. The corresponding items are correctly located in the check.
3. No errors displayed.

#### 3.2.Shiji KDS Interface Not Attached - Use Item Unique Key

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. No Shiji KDS interface is attached to the target outlet.
3. Setup 1 (Enable API portal generate item unique key = Yes) is enabled under Item References Settings.
4. A check exists with old items and known item unique keys.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table with the item unique keys.
2. Verify which identifier is used to locate the items.

**Test Result(s):**
1. The system uses the item unique key to identify the target items.
2. The corresponding items are correctly located in the check.
3. No errors displayed.

#### 3.3.Item Not Found in Check

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. A check exists for the target table.
3. The provided internal check item ID or item unique key does not correspond to any item in the check.

**Step(s):**
1. Send a mark delivery request with a non-existent item identifier.
2. Verify the workflow behaviour for the missing item.

**Test Result(s):**
1. The system skips the not-found item and continues to the next item (step 13).
2. The mark delivery action for other valid items is not affected.
3. No errors displayed.

#### 3.4.Item Already Marked as Delivered

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. A check exists for the target table.
3. Some items in the check have already been marked as delivered previously.

**Step(s):**
1. Send a mark delivery request that includes items already marked as delivered.
2. Verify the workflow behaviour for already-delivered items.

**Test Result(s):**
1. The system treats already-delivered items as failed items for step 15.
2. The system skips these items and continues to the next item (step 13).
3. The failed items are reported in the response with the corresponding reasons.
4. No errors displayed.

### 4.Action Print Queue (Setup 2)

#### 4.1.New Action Print Queue Setup Available

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The signed-in user can access Infrasys POS platform: POS System → Printing Setup → Action Print Queues.

**Step(s):**
1. Navigate to POS System → Printing Setup → Action Print Queues.
2. Add a new record with Available in Locations = target shop or outlet, Action Type = mark delivery action type for API Portal, Print Format = target print format.
3. Click "Save" to save the record.

**Test Result(s):**
1. The new action print queue setup is available under POS System → Printing Setup → Action Print Queues.
2. The new action type for mark delivery (Api Portal) can be selected.
3. The record is saved successfully.
4. No errors displayed.

#### 4.2.Mark Delivery Slip Printed When Setup 2 Exists

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. Setup 2 (action print queue for mark delivery API Portal) is configured for the target location.
3. The API portal mark delivery endpoint is accessible.
4. A check exists with at least one old item not yet marked as delivered.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table.
2. Verify whether a mark delivery action slip is printed.

**Test Result(s):**
1. The system prints the mark delivery action slip via the action print queue (Setup 2).
2. The printed slip contains the correct mark delivery information.
3. No errors displayed.

#### 4.3.Setup 2 With Value "Mark Delivery" Ignored for API Portal

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. An action print queue record with Action Type = "Mark Delivery" (normal workstation) exists.
3. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table.
2. Verify whether the system uses the "Mark Delivery" action print queue record.

**Test Result(s):**
1. The system ignores the action print queue record with Action Type = "Mark Delivery" because that record is for normal workstations only.
2. No mark delivery slip is printed via that record.
3. No errors displayed.

#### 4.4.No Setup 2 - Mark Delivery Still Succeeds Without Print

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. No Setup 2 (action print queue for mark delivery API Portal) is configured.
3. The API portal mark delivery endpoint is accessible.
4. A check exists with at least one old item not yet marked as delivered.

**Step(s):**
1. Send a mark delivery request to the API portal for the target table.
2. Verify the mark delivery result and the printing behaviour.

**Test Result(s):**
1. The mark delivery action succeeds (item is marked as delivered).
2. No mark delivery slip is printed because no Setup 2 is configured.
3. No errors displayed.

### 5.Item References Settings (Setup 1)

#### 5.1.Enable API Portal Generate Item Unique Key Setup Available

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The signed-in user can access Infrasys POS platform: POS System → Ordering Setup → Config by Location.

**Step(s):**
1. Navigate to POS System → Ordering Setup → Config by Location.
2. Select the "Item References Settings" setup.
3. Add a new record or edit an existing record.
4. Configure Apply To, Shop, Outlet and Station as needed.
5. Set "Enable API portal generate item unique key" = Yes.
6. Click "Save" to save the record.

**Test Result(s):**
1. The "Item References Settings" setup is available under Config by Location.
2. The "Enable API portal generate item unique key" field can be set to Yes (Setup 1).
3. Apply To can be configured as All Locations, Shop, Outlet or Station.
4. The record is saved successfully.
5. No errors displayed.

#### 5.2.API Portal Generates Item Unique Key When Setup 1 Is Yes

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. Setup 1 (Enable API portal generate item unique key = Yes) is enabled for the target location.
3. No Shiji KDS interface is attached to the target outlet.

**Step(s):**
1. Open a check for the target table through the API portal.
2. Verify the item unique key is generated and accessible via the API portal.

**Test Result(s):**
1. The API portal generates an item unique key for each item in the check.
2. The item unique key can be used to identify items in the mark delivery request.
3. No errors displayed.

### 6.Response Handling

#### 6.1.Successful Response With No Failed Items

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. A check exists with at least one old item not yet marked as delivered.

**Step(s):**
1. Send a mark delivery request that successfully marks all requested items as delivered.
2. Verify the response body.

**Test Result(s):**
1. The system returns a successful response.
2. The response does not contain any failed items.
3. No errors displayed.

#### 6.2.Successful Response With Some Failed Items

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. A check exists with a mix of valid items and items that will fail (for example already delivered items).

**Step(s):**
1. Send a mark delivery request that includes both valid items and items that will fail.
2. Verify the response body.

**Test Result(s):**
1. The system returns a successful response because at least one item was successfully marked as delivered.
2. The response contains the failed items and the corresponding reasons.
3. No errors displayed.

#### 6.3.Error Response When All Items Fail

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. A check exists where all requested items will fail (for example all items are already delivered).

**Step(s):**
1. Send a mark delivery request where all requested items fail.
2. Verify the response body.

**Test Result(s):**
1. The system returns an error response because no items were successfully marked as delivered.
2. The response contains all failed items and the corresponding reasons.
3. No errors displayed.

### 7.Negative and Edge Cases

#### 7.1.Mark Delivery Request Without Item IDs and Empty Check

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.
3. A check exists for the target table but has no items.

**Step(s):**
1. Send a mark delivery request without providing any item IDs.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system handles the empty check gracefully.
2. The response indicates that no items were marked as delivered.
3. No unhandled server error is displayed.

#### 7.2.Mark Delivery Request for Non-Existent Table

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. The API portal mark delivery endpoint is accessible.

**Step(s):**
1. Send a mark delivery request for a non-existent table number.
2. Verify the HTTP status code and the response body.

**Test Result(s):**
1. The system returns an error message indicating the target table is not occupied.
2. The action is aborted.
3. No errors displayed.

#### 7.3.API Portal Only - Normal POS Workstations Not Required to Support

**Prerequisite(s):**
1. A build with the HERO-67817 change is deployed.
2. A normal POS workstation is available.

**Step(s):**
1. Attempt to trigger the new mark delivery flow from a normal POS workstation.
2. Verify the workstation behaviour.

**Test Result(s):**
1. The new mark delivery flow is not exposed on normal POS workstations.
2. Normal POS workstation mark delivery behaviour remains unchanged.
3. No errors displayed.

### 8.Regression Scope

- Existing API portal mark delivery flows without Shiji KDS interface usage remain unaffected.
- Existing normal POS workstation mark delivery behaviour remains unaffected.
- Action Print Queue records for other action types remain unaffected.
- Item References Settings for other use cases remain unaffected.
- Shiji KDS interface setup and existing KDS interface features remain unaffected.

## Test Environment

- **Version**: Infrasys POS Platform release containing the HERO-67817 change
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
