# HERO-69391 Test Report - Printing Error Alert Did Not Show Up in Vertical Mobile View That Blocks Operations

## Background

In vertical mobile view, when the printing error alert setting is enabled and the printer device is offline, clicking function "Send Check" causes the mobile application to keep prompting for processing, and the system cannot perform any operation after sending the kitchen slip.

Expected behaviour: operations will not be blocked and no error dialog box shows in mobile view.

## Functional Testing

### 1. Bug Reproduction and Fix Verification - Printing Error Alert in Vertical Mobile View

#### 1.1. Verify Send Check with Offline Printer Does Not Block Operations in Vertical Mobile View

**Prerequisite(s):**
1. The setup "Prompt Printing Error Alert By Functions" exists under POS System > Ordering Setup > Config by Location with a record for the target location, with "Support" set to "Yes" and "Kitchen Slip" set to "Yes"
2. The printer device status is offline
3. Tester is logged in to the POS client with vertical mobile view

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Send Check" to trigger kitchen slip printing
4. After the kitchen slip is sent with the printer offline, try to perform other operations on the check and the ordering panel

**Reproduction Result(s):**
1. Before the fix, the mobile application keeps prompting that the table is processing after the kitchen slip is sent while the printer is offline
2. The system cannot perform any other operations in mobile view; the user is blocked

**Fix Result(s):**
1. The send check operation completes without the application staying in the processing state
2. Operations are not blocked; the user can continue to perform other operations on the check and the ordering panel in vertical mobile view

#### 1.2. Verify No Printing Error Dialog Box Shows in Vertical Mobile View When Printer Is Offline

**Prerequisite(s):**
1. The setup "Prompt Printing Error Alert By Functions" exists under POS System > Ordering Setup > Config by Location with a record for the target location, with "Support" set to "Yes" and "Kitchen Slip" set to "Yes"
2. The printer device status is offline
3. Tester is logged in to the POS client with vertical mobile view

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Send Check" to trigger kitchen slip printing
4. Observe the screen after the send check action with the printer offline

**Reproduction Result(s):**
1. Before the fix, the printing error alert does not show up in vertical mobile view, and instead the mobile application keeps prompting for processing, which blocks operations

**Fix Result(s):**
1. No error dialog box shows in mobile view when the printer is offline
2. No processing prompt remains on screen, and the interface returns to the normal operable state

### 2. Related Printing Scenarios - Regression After Fix

#### 2.1. Verify Send Check and Kitchen Slip Printing Work Normally in Vertical Mobile View When Printer Is Online

**Prerequisite(s):**
1. The setup "Prompt Printing Error Alert By Functions" exists under POS System > Ordering Setup > Config by Location with a record for the target location, with "Support" set to "Yes" and "Kitchen Slip" set to "Yes"
2. The printer device status is online
3. Tester is logged in to the POS client with vertical mobile view

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Send Check" to trigger kitchen slip printing
4. Check the kitchen slip at the printer and the check status in the POS client

**Reproduction Result(s):**
1. Before the fix, send check with an online printer works normally (the bug is only triggered when the printer is offline)

**Fix Result(s):**
1. The kitchen slip is printed successfully at the printer
2. The send check operation completes normally and the check status is updated as before
3. Operations in vertical mobile view are not affected

#### 2.2. Verify Printing Error Alert Behaviour in Horizontal Desktop View Is Unchanged

**Prerequisite(s):**
1. The setup "Prompt Printing Error Alert By Functions" exists under POS System > Ordering Setup > Config by Location with a record for the target location, with "Support" set to "Yes" and "Kitchen Slip" set to "Yes"
2. The printer device status is offline
3. Tester is logged in to the POS client with horizontal desktop view

**Step(s):**
1. Open a new check
2. Order item(s)
3. Click function "Send Check" to trigger kitchen slip printing
4. Observe the system behaviour in horizontal desktop view with the printer offline

**Reproduction Result(s):**
1. Before the fix, horizontal desktop view follows the existing behaviour of the "Prompt Printing Error Alert By Functions" setting when the printer is offline (the bug only occurs in vertical mobile view)

**Fix Result(s):**
1. The behaviour of horizontal desktop view regarding the printing error alert remains unchanged after the fix
2. The send check operation in horizontal desktop view is not affected by the fix

### 3. Regression Scope

The following related modules and functions must be retested after the fix:

- Send Check function and kitchen slip printing flow in vertical mobile view, with the printer both offline and online
- Processing/loading state handling in vertical mobile view after printing actions
- The "Prompt Printing Error Alert By Functions" setting (Support, Kitchen Slip) under Config by Location and its effect on different views
- Printing error alert behaviour in horizontal desktop view (must remain unchanged)
- General check operations in vertical mobile view after send check (e.g., continue ordering, other functions on the ordering panel)

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-69391 |
| Summary | Printing error alert did not show up in vertical mobile view that blocks operations |
| Sprint | POS - 20261019-20261030 |
| Template | infrasys |
| Issue Type | Bug |
| QC Tester | Zephyr Ji (zephyr.ji@shijigroup.com) |
| QC Reviewer | Kathy Kuang (kathy.kuang@shijigroup.com) |
| Test Target | POS client - vertical mobile view, Send Check / kitchen slip printing |
| Required Setup | "Prompt Printing Error Alert By Functions" (POS System > Ordering Setup > Config by Location): Support = Yes, Kitchen Slip = Yes |
| Device Condition | Printer device offline (bug scenario) and online (regression scenario) |
