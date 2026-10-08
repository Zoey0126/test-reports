# HERO-62139 Test Report

## Functional Testing

### F1.Action Slip Printed After Delay Following Scheduled Items Sent

#### F1.1.Verify Action Slip Is Printed After Delay Following Scheduled Item Send via "Set Item Scheduling"

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: Action Print Queue with Action Type = "Scheduled Item Sent" is configured under POS System > Printing Setup > Action Print Queues, with a print format and printer bound to it
3. Setup 2: Scheduler For Sending Item is configured under POS System > Ordering Setup > Config by Location with Turn on scheduler = Yes and Delay time of printing action slip = a target delay time in minutes (e.g. 1 minute) for the target outlet/station
4. A printer is connected to the workstation and is reachable from the POS Client
5. A new check can be opened and scheduled items can be added

**Step(s):**
1. Open a new check at a table
2. Add menu items to the check
3. Use the function "Set Item Scheduling" to schedule items with a target due time
4. Click the "Send Check" function to send the check to the kitchen
5. Observe the kitchen slip being printed when the scheduled time is reached
6. Observe the action slip being printed after the configured delay time (Delay time of printing action slip)
7. Verify the action slip content

**Test Result(s):**
1. The kitchen slip is printed when the scheduled time is reached
2. After the configured delay time (e.g. 1 minute) following the kitchen slip printing, the system prints the action slip
3. The action slip contains the correct items, table number with table extension, check number, and due time
4. The action slip is printed on the configured printer

#### F1.2.Verify Action Slip Is Printed After Delay Following Scheduled Item Send via "Auto Fired Time for Pending Item"

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: Action Print Queue with Action Type = "Scheduled Item Sent" is configured under Action Print Queues with a print format and printer bound
3. Setup 2: Scheduler For Sending Item is configured with Turn on scheduler = Yes and Delay time of printing action slip = a target delay time in minutes (e.g. 1 minute) for the target outlet/station
4. A printer is connected to the workstation
5. A new check can be opened and pending items can be added with an auto fired time

**Step(s):**
1. Open a new check at a table
2. Add menu items to the check as pending items with an auto fired time configured
3. Click the "Send Check" function to send the check to the kitchen
4. Observe the kitchen slip being printed when the auto fired time is reached
5. Observe the action slip being printed after the configured delay time (Delay time of printing action slip)
6. Verify the action slip content

**Test Result(s):**
1. The kitchen slip is printed when the auto fired time is reached
2. After the configured delay time following the kitchen slip printing, the system prints the action slip
3. The action slip contains the correct items, table number with table extension, check number, and due time
4. The action slip is printed on the configured printer

### F2.Action Slip Content Verification

#### F2.1.Verify Action Slip Contains All Required Information

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: Action Print Queue with Action Type = "Scheduled Item Sent" is configured with a print format containing the required variables
3. Setup 2: Scheduler For Sending Item is configured with Turn on scheduler = Yes and Delay time of printing action slip = a target delay time in minutes
4. A printer is connected to the workstation
5. A new check at a table (with extension if applicable) can be opened

**Step(s):**
1. Open a new check at a table (note the table number and extension)
2. Add multiple menu items to the check
3. Use the function "Set Item Scheduling" to schedule items with a target due time (note the due time)
4. Click the "Send Check" function
5. Wait for the scheduled time to be reached and the kitchen slip to be printed
6. Wait for the configured delay time and observe the action slip
7. Verify each field on the printed action slip

**Test Result(s):**
1. The action slip contains the Items field with all scheduled items listed correctly (item name, quantity)
2. The action slip contains the Table number field with the table number and table extension
3. The action slip contains the Check number field with the correct check number
4. The action slip contains the Due time field with the scheduled due time
5. All fields are correctly formatted and readable on the printed output

### F3.Setup Configuration and Behavior

#### F3.1.Verify No Action Slip Is Printed When Setup 2 Turn on Scheduler = No

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: Action Print Queue with Action Type = "Scheduled Item Sent" is configured under Action Print Queues
3. Setup 2: Scheduler For Sending Item is configured with Turn on scheduler = No (or the setup record does not exist) for the target outlet/station
4. A printer is connected to the workstation

**Step(s):**
1. Open a new check at a table
2. Add menu items to the check and use "Set Item Scheduling" to schedule items with a target due time
3. Click the "Send Check" function
4. Wait for the scheduled time to be reached
5. Observe whether an action slip is printed

**Test Result(s):**
1. When Setup 2 Turn on scheduler = No (or the setup does not exist), no scheduler is triggered for sending items
2. No action slip is printed after the scheduled time
3. No unexpected behavior is observed

#### F3.2.Verify Action Slip Delay Time Honors the Configured Value

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. Setup 1: Action Print Queue with Action Type = "Scheduled Item Sent" is configured under Action Print Queues
3. Setup 2: Scheduler For Sending Item is configured with Turn on scheduler = Yes and Delay time of printing action slip = a specific target value in minutes (e.g. 5 minutes)
4. A printer is connected to the workstation

**Step(s):**
1. Open a new check at a table, add items and schedule them with a target due time
2. Click the "Send Check" function
3. Wait for the scheduled time to be reached and the kitchen slip to be printed
4. Observe the time the action slip is printed relative to the kitchen slip print time
5. Verify the actual delay matches the configured delay time

**Test Result(s):**
1. The action slip is printed after the configured delay time (e.g. 5 minutes) following the kitchen slip printing
2. The actual delay between the kitchen slip print and the action slip print matches the configured Delay time of printing action slip
3. No action slip is printed before the configured delay time elapses

### F4.Regression Scope - Other Print Operations and Send Check Behaviors

#### F4.1.Verify Other Print Operations and Send Check Behaviors Remain Unaffected

**Prerequisite(s):**
1. POS Client with the new feature applied is installed and running on a Workstation (Windows 11 Enterprise)
2. Setup 1 and Setup 2 are configured for the new action slip printing
3. Multiple print formats (kitchen slip, customer check, action slip for other actions, report) are configured in POS

**Step(s):**
1. Open a new check, add items (both scheduled and non-scheduled), send the check and verify the kitchen slip is printed correctly
2. Print the customer check and verify it prints correctly
3. Trigger an action slip for another action type (e.g. mark delivery, void item) and verify it prints correctly
4. Generate a report and verify it prints correctly
5. Verify non-scheduled items do not trigger the new action slip

**Test Result(s):**
1. The kitchen slip is printed correctly for both scheduled and non-scheduled items
2. The customer check prints correctly
3. Action slips for other action types (mark delivery, void item) print correctly without regression
4. Report printing functions correctly
5. Non-scheduled items do not trigger the new "Scheduled Item Sent" action slip

## Compatibility Testing

### C1.Workstation and Network Compatibility

#### C1.1.Verify Action Slip Printing Works Across Networks

**Prerequisite(s):**
1. POS Client is installed on a Workstation (Windows 11 Enterprise)
2. Setup 1 and Setup 2 are configured for the new action slip printing
3. POS backend and printer are reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations

**Step(s):**
1. Connect the POS Client to Cloud network, perform a scheduled item send workflow and verify the action slip prints after the delay
2. Repeat under Local, OnPremise, Wireless and Wired networks

**Test Result(s):**
1. The action slip prints correctly after the configured delay across Cloud, Local, OnPremise, Wireless and Wired networks
2. The action slip content is correct in each network configuration
3. No network-related compatibility issues are introduced by the new feature

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS
- **Issue Type**: Improvement
- **Module**: Action Print Queue, Scheduler For Sending Item
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **QC Tester**: Phoebe Chen (phoebe.chen@shijigroup.com)
- **QC Reviewer**: Briana Li (briana.li@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-62139)

Enhancement Details: Dev case 2/2 for HERO-45421

Release Notes:

- It is supported to print an action slip after a specified delay following scheduled items being sent to the kitchen
- The following information is supported on the action slip: Items, Table number with table extension, Check number, Due time

Workflow:

1. Open a new check
2. Order scheduled items, either by the function "Set Item Scheduling" or by setting an auto fired time for pending item
3. Click function "Send Check"
4. The system prints the kitchen slip when the scheduled time is reached
5. After the delay specified in the setup "Delay time of printing action slip", the system prints an action slip

Setup:

- Setup 1: Action Type = "Scheduled Item Sent" under POS System > Printing Setup > Action Print Queues
- Setup 2: Scheduler For Sending Item under POS System > Ordering Setup > Config by Location with Turn on scheduler = Yes and Delay time of printing action slip = Target delay time in minutes
