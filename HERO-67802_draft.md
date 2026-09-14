# API Cloud Modification - Allow KDS to trigger POS mark delivery for printing Test Report

## Functional Testing

### 1.Mark Delivery API Success Scenarios

#### 1.1.Mark delivery for existing unmarked items returns success without error

**Prerequisite(s):**
1. Infrasys POS version 1.2.115.0 or above is deployed.
2. Outlet 1001 of shop 0001 has check 0008 containing delivery items that have not been marked yet.
3. The Shiji KDS interface is configured and the API Cloud Portal account has access to POSgAPI 10.21.0.

**Step(s):**
1. Send POST /shops/0001/outlets/1001/checks/0008/mark_deliveries with a valid interface_code, applied_user_no, and items containing valid item id or unique_key.
2. Check the response and the item status on the check.
3. Trigger printing for the marked delivery.

**Test Result(s):**
1. The response is returned without error.
2. The items are marked as delivered on the check and only existing, not previously marked items are marked.
3. The mark delivery triggers the printing flow as expected.

#### 1.2.Mark delivery using item unique_key when the interface is not attached to the outlet

**Prerequisite(s):**
1. Infrasys POS version 1.2.115.0 or above is deployed.
2. The outlet is not attached with the shiji_kds interface.
3. The check contains delivery items that have not been marked yet.

**Step(s):**
1. Send the mark delivery request identifying the items by unique_key instead of item id.

**Test Result(s):**
1. The response is returned without error and the items identified by unique_key are marked as delivered.

#### 1.3.Mark delivery includes child items

**Prerequisite(s):**
1. The check contains a parent delivery item with child items (e.g., set meal components) that have not been marked yet.

**Step(s):**
1. Send the mark delivery request with the parent item id or unique_key and its child_items list.
2. Check the mark delivery status of the parent item and its child items.

**Test Result(s):**
1. The response is returned without error.
2. The parent item and its child items are all marked as delivered.

#### 1.4.Employee identity follows the application user number for shiji_KDS

**Prerequisite(s):**
1. The shiji_kds interface is attached to the outlet.
2. An application user number is configured with employee id and password.

**Step(s):**
1. Send the mark delivery request with interface_code = shiji_KDS and the corresponding applied_user_no.
2. Check the employee recorded on the mark delivery operation.

**Test Result(s):**
1. The employee id and employee password follow the application user number.
2. The mark delivery succeeds and the applied employee recorded on the check matches the application user number.

### 2.Mark Delivery API Validation and Partial Failure

#### 2.1.Missing item id when shiji_kds interface is attached to the outlet

**Prerequisite(s):**
1. The shiji_kds interface is attached to the outlet.
2. The check contains delivery items that have not been marked yet.

**Step(s):**
1. Send the mark delivery request without the mandatory item id (only unique_key).

**Test Result(s):**
1. The request is rejected with a validation error because item id is mandatory when the shiji_kds interface is attached to the outlet.
2. No item is marked as delivered.

#### 2.2.Mark delivery for item not existing in the check

**Prerequisite(s):**
1. The check contains delivery items and the tester knows an item id or unique_key that does not exist in the check.

**Step(s):**
1. Send the mark delivery request containing only the item that does not exist in the check.

**Test Result(s):**
1. The response is returned with error: Fail to mark delivery.
2. No item is marked as delivered on the check.

#### 2.3.Items already marked before are skipped with a warning

**Prerequisite(s):**
1. Item A in the check has already been marked as delivered and item B has not been marked yet.

**Step(s):**
1. Send the mark delivery request containing both item A (already marked) and item B (not marked).
2. Check the response and the mark status of both items.

**Test Result(s):**
1. The response is returned with warning: Fail to mark delivery for part of items.
2. Item A is not marked again and item B is marked successfully.
3. The warning_details in the response list item A with the corresponding message.

#### 2.4.All items invalid returns error

**Prerequisite(s):**
1. The tester knows several item ids or unique_keys that do not exist in the check or have been marked before.

**Step(s):**
1. Send the mark delivery request containing only invalid items (not existing or already marked).

**Test Result(s):**
1. The response is returned with error: Fail to mark delivery.
2. No valid mark delivery is applied to the check.

#### 2.5.Invalid interface_code is rejected

**Prerequisite(s):**
1. The API Cloud Portal account has access to the mark delivery API.

**Step(s):**
1. Send the mark delivery request with an unknown interface_code.
2. Send another request with an interface_code value longer than 128 characters.

**Test Result(s):**
1. Both requests are rejected with a validation error on the interface_code parameter.
2. No item is marked as delivered.

### 3.Regression Scope

1. POS-side mark delivery for delivery items from the POS application.
2. KDS item marking flow and item status synchronization between KDS and POS.
3. Check printing after mark delivery is triggered.
4. Existing POSgAPI check operations (e.g., order, mark paid, void) are not affected by the new interface_code parameter.
