# HERO-69281 Bug Fix Test Report - Duplicate Items Occurred When Get Check Information Request Is Sent During Split Table Save

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Concurrent Get Check Information During Split Table Save

#### F1.1.Verify No Duplicate Items Are Saved When API Request Is Received During Split Table Save
**Prerequisite(s):**
1. API Portal is configured and able to send the API request "Get Check Information" by check number
2. An old check (e.g. Check #0220) with multiple items exists in the workstation
3. The check is not yet paid and can be opened in the ordering panel

**Step(s):**
1. Open the old check (e.g. Check #0220) in the workstation
2. Click the function "Split Table"
3. Follow the existing workflow to split the items into the target table
4. Click the button "Send Check" to save those checks
5. At the same time, send the API request "Get Check Information" by check number #0220 through API Portal
6. Reopen the split checks and review the item list of each check

**Reproduction Result(s):**
1. The system saves duplicate items into the split checks
2. The total item quantity of the check is greater than the original ordered quantity
3. The item list shows the same item more than once with different sequence numbers

**Fix Result(s):**
1. No duplicate item is saved into the split checks
2. The total item quantity of each split check matches the original ordered quantity
3. The item list of each split check contains each item only once

#### F1.2.Verify Item Sequence Number Is Correct in Calling Queue Response
**Prerequisite(s):**
1. API Portal is able to send the API request "Get Check Information" and capture the response payload
2. An old check with at least three items exists in the workstation

**Step(s):**
1. Open the old check in the workstation
2. Click "Split Table" and split the items into the target table
3. Click "Send Check" to save the checks
4. Send the API request "Get Check Information" by check number at the same time as the save
5. Capture the calling queue response for the old items and review the sequence number of each item

**Reproduction Result(s):**
1. The calling queue response contains an incorrect sequence number for the old items
2. The same sequence number is used more than once, or the sequence is not continuous
3. The response content does not match the item sequence stored in the check

**Fix Result(s):**
1. Every item in the calling queue response has a correct and unique sequence number
2. The sequence number is continuous and consistent with the item order stored in the check
3. The response content matches the saved check data

#### F1.3.Verify Multiple Concurrent API Requests During Split Table Save
**Prerequisite(s):**
1. API Portal is able to send multiple "Get Check Information" requests in a short interval
2. A check with multiple items exists and is ready to be split

**Step(s):**
1. Open the check and click "Split Table"
2. Split the items into the target table
3. Click "Send Check"
4. Send three "Get Check Information" requests for the same check number within the saving period
5. Compare the responses and reopen the saved checks

**Reproduction Result(s):**
1. Duplicate items are saved and the item sequence numbers are incorrect in more than one response
2. The saved check data becomes inconsistent with the ordered quantity

**Fix Result(s):**
1. No duplicate item is saved regardless of the number of concurrent requests
2. All responses return a consistent item list with correct sequence numbers
3. The saved check data remains consistent with the ordered quantity

#### F1.4.Verify Get Check Information Returns Current Check Information During Split Table Save
**Prerequisite(s):**
1. API Portal is able to send "Get Check Information" by check number
2. A check is being split and saved at the same moment

**Step(s):**
1. Open the check and click "Split Table"
2. Split the items into the target table and click "Send Check"
3. Send "Get Check Information" for the same check number during the save
4. Compare the response with the check information displayed in the workstation

**Reproduction Result(s):**
1. The response mixes the pre-save item list with the post-save item list
2. The response item quantity is not equal to either the pre-save or the post-save quantity

**Fix Result(s):**
1. The response returns the current check information instead of the partially saved data of the Split Table function
2. The response item quantity is consistent with the check information displayed in the workstation
3. The response does not contain duplicated items or incorrect sequence numbers

#### F1.5.Verify Normal Split Table Workflow Without API Request
**Prerequisite(s):**
1. An old check with multiple items exists in the workstation
2. No API request is sent during the test

**Step(s):**
1. Open the check and click "Split Table"
2. Split the items into the target table
3. Click "Send Check" to save the checks
4. Click "Print All" and reopen each split check

**Reproduction Result(s):**
1. The original split workflow is completed without duplicate items when no API request is received
2. The item quantity of each split check is correct

**Fix Result(s):**
1. The split workflow is completed normally without duplicate items
2. The item quantity and sequence number of each split check are correct
3. The printing and the saved check data are consistent with the split result

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Split Table function with quantity options and equal quantity option
- Send Check save workflow after Split Table
- Print All after Split Table
- API Portal "Get Check Information" by check number for a check that is not being saved
- API Portal "Get Check Information" for paid checks, void checks and checks with void items
- Calling queue response construction for old items
- Item sequence number generation for other item level operations such as Transfer Item and Void Item
- Check item quantity and total amount after Split Table
