# HERO-68814 Test Report

## Functional Testing

### F1.Unmark Delivery API Request with Single Item

#### F1.1.Verify Successful Unmark Delivery for a Single Delivered Item via API

**Prerequisite(s):**
1. POS backend with the new unmark delivery API deployed is accessible through API Portal in the local test environment
2. The target outlet has the Shiji KDS interface attached OR Setup 1 (Enable API portal generate item unique key = Yes) is configured under POS System > Ordering Setup > Config by Location > Item References Settings
3. A check with at least one item already marked as delivered exists on a real (occupied, not locked) table
4. The API caller (Shiji KDS interface or third-party) holds a valid access token/credential for the API Portal

**Step(s):**
1. Construct an API request with parameters: outlet, check id, item id, quantity (matching a delivered item) and POST it to the new unmark delivery API endpoint
2. Capture the API response code and body
3. Open the target check in POS and verify the delivered status of the target item
4. Verify whether the action slip is printed according to Setup 2 (Print When Rollback) configuration

**Test Result(s):**
1. The API returns a successful response indicating the delivered status of the target item has been cleared
2. The corresponding item's delivered status is cleared in POS
3. If Setup 2 does not exist OR has the value "Yes", the mark delivery action slip is printed for the unmarked item
4. If Setup 2 has the value "No", no action slip is printed

#### F1.2.Verify Item Retrieval by Internal Check Item ID When KDS Interface Is Attached

**Prerequisite(s):**
1. The target outlet has the Shiji KDS interface attached under Config by Location
2. POS backend with the new unmark delivery API is accessible through API Portal
3. A check with multiple items (some delivered, some not delivered) exists on an occupied, unlocked table

**Step(s):**
1. Construct an API request using the internal check item ID of a delivered item as the item identifier
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response and the corresponding item delivered status in POS

**Test Result(s):**
1. The system uses the provided internal check item ID to retrieve the target item because the KDS interface is attached
2. The target item's delivered status is cleared successfully
3. Other items in the check remain unchanged
4. The API returns a successful response

#### F1.3.Verify Item Retrieval by Item Unique Key When KDS Interface Is NOT Attached

**Prerequisite(s):**
1. The target outlet does NOT have the Shiji KDS interface attached under Config by Location
2. Setup 1 (Enable API portal generate item unique key = Yes) is configured under Item References Settings
3. POS backend with the new unmark delivery API is accessible through API Portal
4. A check with a delivered item exists on an occupied, unlocked table

**Step(s):**
1. Construct an API request using the item unique key (generated via Setup 1) as the item identifier
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response and the corresponding item delivered status in POS

**Test Result(s):**
1. The system uses the item unique key (related to Setup 1) to retrieve the target item because the KDS interface is NOT attached
2. The target item's delivered status is cleared successfully
3. The API returns a successful response

### F2.Unmark Delivery API Request with Multiple Items

#### F2.1.Verify Successful Unmark Delivery for Multiple Delivered Items in One Request

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. Setup 1 is configured (or the KDS interface is attached) so items can be retrieved correctly
3. A check exists on an occupied, unlocked table with multiple items already marked as delivered

**Step(s):**
1. Construct an API request containing an item array with multiple delivered items, each with the corresponding item id and quantity
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response code and body
4. Open the target check in POS and verify the delivered status of all items listed in the request

**Test Result(s):**
1. The system loops through the item array one by one and clears the delivered status for each delivered item
2. The API returns a successful response because at least one item's delivered status was cleared successfully
3. All delivered items in the request have their delivered status cleared in POS
4. Items not listed in the request remain unchanged

#### F2.2.Verify Partial Success When Some Items Are Not Found or Not Marked as Delivered

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check exists on an occupied, unlocked table containing at least one delivered item and one not-delivered item
3. The KDS interface is attached OR Setup 1 is enabled

**Step(s):**
1. Construct an API request with an item array containing one delivered item and one item that has NOT been marked as delivered
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response
4. Construct another API request with an item array containing one delivered item and one item id that does not exist under the check
5. POST the request and verify the API response
6. Open the target check in POS and verify the delivered status of items

**Test Result(s):**
1. For the request containing a not-delivered item, the system treats the not-delivered item as failed and proceeds to the next item
2. For the request containing a non-existing item, the system treats the missing item as failed and proceeds to the next item
3. The delivered item included in both requests has its delivered status cleared successfully
4. The system returns a successful response because at least one item's delivered status was cleared successfully

### F3.Unmark Delivery API Request Without Item Array (Whole Check)

#### F3.1.Verify Unmark Delivery for All Items of a Check When No Items Provided

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check exists on an occupied, unlocked table with multiple items, some already marked as delivered and some not delivered

**Step(s):**
1. Construct an API request with parameters outlet and check id only, WITHOUT providing an items array
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response code and body
4. Open the target check in POS and verify the delivered status of all items in the check

**Test Result(s):**
1. The system loops through all items in the check and clears the delivered status for each delivered item
2. Items not previously marked as delivered are treated as failed but processing continues for remaining items
3. The API returns a successful response because at least one item's delivered status was cleared
4. All previously delivered items in the check have their delivered status cleared

### F4.Unmark Delivery API Error and Edge Scenarios

#### F4.1.Verify API Behavior When Target Check Cannot Be Opened

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check id that does not exist or cannot be opened in the system is available for testing

**Step(s):**
1. Construct an API request with a non-existing or invalid check id (and a valid outlet)
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response code and body

**Test Result(s):**
1. The system cannot open the target check
2. The system returns the existing error message and aborts the action request
3. No delivered status is changed for any item
4. The API returns an error response with the appropriate error message

#### F4.2.Verify API Behavior When Table Is Locked or Vacant

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check exists on a table that is currently locked OR a check id corresponding to a vacant table is available for testing

**Step(s):**
1. Construct an API request targeting a check on a locked table
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response and the system behavior
4. Repeat the test with a check id corresponding to a vacant table

**Test Result(s):**
1. When the table is locked, the system waits until the table is unlocked / the table status changes to not vacant
2. After the table is unlocked, the unmark delivery action proceeds and clears delivered status for the target item(s)
3. When the table is vacant, the system returns an appropriate error response indicating the table is not occupied
4. The retry mechanism (KDS retry) is supported as documented

#### F4.3.Verify API Returns Error Response When All Items Fail to Be Cleared

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check exists on an occupied, unlocked table where ALL items in the request are not previously marked as delivered

**Step(s):**
1. Construct an API request where all provided items have NOT been marked as delivered previously
2. POST the request to the new unmark delivery API endpoint
3. Verify the API response code and body

**Test Result(s):**
1. The system treats every item in the request as failed because none were previously marked as delivered
2. No delivered status is cleared for any item
3. The system returns an error response because the delivered status of all items failed to be cleared
4. The action slip is NOT printed because no item's delivered status was cleared

### F5.Action Print Queue Integration for Unmark Delivery

#### F5.1.Verify Action Slip Printing When Setup 2 Is Yes or Does Not Exist

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. Action Print Queue for unmark delivery action type is configured under POS System > Printing Setup > Action Print Queues with Print When Rollback = Yes (Setup 2)
3. A printer/format is bound to the action print queue
4. A check with at least one delivered item exists on an occupied, unlocked table

**Step(s):**
1. Construct an API request targeting the delivered item
2. POST the request to the new unmark delivery API endpoint
3. Observe the printer output for the action slip
4. Repeat the test with Setup 2 not configured (record absent) and observe behavior

**Test Result(s):**
1. When Setup 2 = Yes, the mark delivery action slip is printed after the delivered status is cleared
2. When Setup 2 record does not exist, the action slip is printed (default behavior matches "Yes")
3. The printed action slip reflects the unmark delivery action correctly

#### F5.2.Verify No Action Slip Printed When Setup 2 Is No

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. Action Print Queue for unmark delivery action type is configured with Print When Rollback = No (Setup 2)
3. A check with at least one delivered item exists on an occupied, unlocked table

**Step(s):**
1. Construct an API request targeting the delivered item
2. POST the request to the new unmark delivery API endpoint
3. Observe the printer output for the action slip

**Test Result(s):**
1. When Setup 2 = No, the system does NOT print the action slip for mark delivery after clearing the delivered status
2. The delivered status is still cleared successfully in POS
3. The API returns a successful response

### F6.Retry Mechanism for KDS

#### F6.1.Verify KDS Retry Mechanism Recovers From a Transient Failure

**Prerequisite(s):**
1. POS backend with the new unmark delivery API is accessible through API Portal
2. A check exists on a table that is initially locked and can be unlocked during the test
3. The KDS interface / API caller is configured to retry failed requests

**Step(s):**
1. Lock the target table and trigger an unmark delivery API request
2. Observe the API response (the system waits until the table is unlocked)
3. Unlock the table while the request is in progress or trigger a retry from KDS
4. Verify the API response after the table is unlocked
5. Verify the delivered status of the target item in POS

**Test Result(s):**
1. The system waits for the table to be unlocked / table status to change to not vacant
2. After the table is unlocked, the unmark delivery action proceeds successfully
3. The KDS retry mechanism recovers from the transient failure
4. The target item's delivered status is cleared and a successful response is returned

## Compatibility Testing

### C1.API Portal Compatibility

#### C1.1.Verify Unmark Delivery API Works Across POS Environments

**Prerequisite(s):**
1. The new unmark delivery API is deployed in Cloud, Local, Mobile, OnPremise, Wireless and Wired network configurations
2. POS backend is accessible through API Portal across these environments

**Step(s):**
1. Send the unmark delivery API request from each network/environment configuration
2. Verify the API response in each environment
3. Verify the delivered status of the target item is cleared in each environment

**Test Result(s):**
1. The unmark delivery API returns a successful response in Cloud, Local, Mobile, OnPremise, Wireless and Wired environments
2. The delivered status of the target item is cleared in each environment
3. No environment-specific failures are observed

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Component**: POS
- **Issue Type**: New Feature
- **Interface**: API Portal (Shiji KDS interface / third-party integration)
- **Test Network**: Cloud, Local, Mobile, OnPremise, Wireless, Wired
- **QC Tester**: N/A
- **QC Reviewer**: N/A

## Appendix

### Acceptance Criteria (from JIRA HERO-68814)

Enhancement: Design new API for KDS to trigger Unmark Delivery function key

Parameter: outlet, check id, item id, quantity

POS supports a new API request in API portal for unmark delivery for Shiji KDS interface usage. Use existing API Cloud logic to let Shiji KDS interface/other third party to trigger the unmark delivery action.

- Only available for unmarked delivery and old items
- Support multiple unmarked delivery items in single request
- System supports unmark delivery for all items of a check when no items are given in the request
- System supports a new action print queue for unmark delivery (tbc)

Unsuccessful Scenario: no table/check, no item record, table is not occupied, table is locked

Error Handling:

- Table Locked or Vacant: Wait until the table unlock / the table status change to not vacant
- Cannot find the item under Check: follow the check number, unmark delivery for the remaining item
- Item has been unmarked as delivered: unmark delivery for the remaining item

Retry mechanism: let KDS to retry

Workflow for API portal:

1. System receives the request for unmark delivery
2. If the target check cannot be opened, the system returns the existing error message and abort action request
3. Open the target table
4. If Shiji KDS interface is attached to the outlet, system uses the provided internal check item ID to retrieve the target item
5. If Shiji KDS interface NOT attached, system uses the item unique key (related to setup 1) to retrieve the target item
6. If the no items array provided in the request, loop all the items to clear the delivered status, go to step 10
7. If there is item array provided in the request, loop the item array one by one, go to next step
8. If the target item is not found in the check, the system treats this item as fail, and goes to step 12
9. If the target items are found, go to next step
10. If the corresponding item is NOT marked as delivered previously, the system treats this item as failed, and goes to step 12
11. If the corresponding item is marked as delivered previously, the system clears the delivered status
12. Go back to step 6 to proceed with the next item OR go to next step if it is last item
13. If setup 2 does not exist OR has the value "Yes", system prints the slip mark delivery action slip
14. If setup 2 has the value "No", the system does not print the action slip for mark delivery
15. If the delivered status of any items is cleared successfully, the system returns a successful response
16. If the delivered status of all items failed to be cleared, the system returns an error response

Setup:

- Setup 1: Enable API portal generate item unique key = Yes (Item References Settings under Config by Location)
- Setup 2: Print When Rollback (Action Print Queues under Printing Setup)
