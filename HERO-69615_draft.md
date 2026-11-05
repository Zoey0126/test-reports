# HERO-69615 KDS 2.0 - Support Search by Table Number Test Report

## Background

**User Story**

As a KDS operator, I want to search tickets by table number so that I can quickly locate a specific table's tickets within the currently active status filter.

**Current Behavior**

KDS ticket views provide no way to search tickets by table number. Operators can only filter tickets by status and must visually scan the ticket list to find a specific table.

**Expected Behavior**

A search box is available on all KDS ticket views across every kitchen station, opened by tapping a search icon. Matching uses a contains match on the table number, so entering "2" returns tables 2, 32, 12, and 20. Search applies only within the statuses currently selected/active on the KDS screen and matches the table number only. When no tickets match, the result area shows an empty state with a message such as "No tickets found". The search is available to all KDS users with no role restriction.

## Functional Testing

### 1.Search Box Access and Availability

#### 1.1.Verify Search Box Is Opened by Tapping the Search Icon
**Prerequisite(s):**
1. KDS 2.0 is deployed and accessible on a kitchen station
2. At least one ticket exists on the KDS ticket view
3. The operator is logged in to the KDS screen

**Step(s):**
1. Open the KDS ticket view on a kitchen station
2. Locate the search icon on the ticket view
3. Tap the search icon
4. Observe the screen behaviour

**Test Result(s):**
1. The search box is opened after the operator taps the search icon
2. The search box provides a keypad for the operator to enter a table number

#### 1.2.Verify Search Is Available on All KDS Ticket Views Across Every Kitchen Station
**Prerequisite(s):**
1. KDS 2.0 is deployed with multiple kitchen stations configured
2. Each kitchen station has tickets displayed on its ticket view

**Step(s):**
1. Open the KDS ticket view on the first kitchen station
2. Tap the search icon and confirm the search box opens
3. Repeat the verification on every other kitchen station and every KDS ticket view

**Test Result(s):**
1. The search icon and search box are available on all KDS ticket views
2. The search feature behaves consistently across every kitchen station

### 2.Table Number Search Matching

#### 2.1.Verify Contains Match on Partial Table Number
**Prerequisite(s):**
1. The KDS ticket view shows tickets for tables 2, 32, 12, 20 and 11 under the currently active statuses

**Step(s):**
1. Tap the search icon to open the search menu
2. Enter "2" on the keypad
3. Observe the filtered tables on the right-hand side of the search menu

**Test Result(s):**
1. The filtered table list on the right-hand side shows tables 2, 32, 12 and 20
2. Table 11 is not shown because its table number does not contain "2"
3. Matching uses a contains match on the table number

#### 2.2.Verify Search Matches Table Number Only
**Prerequisite(s):**
1. Tickets exist on the KDS ticket view whose ticket number, guest name or server name contains a value that does not appear in any table number

**Step(s):**
1. Tap the search icon to open the search menu
2. Enter a value that matches a ticket number, a guest name or a server name but not any table number
3. Observe the filtered result
4. Enter a value that matches both a table number and another field such as the ticket number
5. Observe the filtered result

**Test Result(s):**
1. Tickets are not matched by ticket number, guest name or server name
2. When the entered value matches no table number, the text "No tickets found" is shown
3. When the entered value matches a table number, only the tickets of the matching table number are returned

#### 2.3.Verify Search Applies Only Within the Currently Active Status Filter
**Prerequisite(s):**
1. Tickets with the same or similar table numbers exist in different statuses on the KDS screen
2. The operator has selected only a subset of statuses on the KDS screen

**Step(s):**
1. Select one or more statuses on the KDS screen
2. Tap the search icon and enter a table number that exists in both selected and unselected statuses
3. Observe the filtered result

**Test Result(s):**
1. Search applies only within the statuses currently selected/active on the KDS screen
2. Tickets matching the table number in other unselected statuses are not shown

### 3.Search Result Navigation and Highlight

#### 3.1.Verify Clicking a Filtered Table Scrolls to the Target Ticket With Blinking Highlight
**Prerequisite(s):**
1. Multiple tickets exist on the KDS ticket view under the currently active statuses
2. The search menu is opened

**Step(s):**
1. Enter a table number on the keypad to filter tables on the right-hand side
2. Click on one table button from the filtered list
3. Observe the ticket screen

**Test Result(s):**
1. The ticket screen scrolls to the target ticket automatically within the currently selected statuses
2. The ticket shows up on the screen with a highlight outline around it that blinks for a few seconds

#### 3.2.Verify the Latest Created Ticket Is Searched When Two Tickets Share the Same Table Number or Extension
**Prerequisite(s):**
1. Two tickets with the same table number/extension exist on the KDS ticket view, for example two tickets from table 959A, one created at 12:10 and one created at 12:11

**Step(s):**
1. Tap the search icon and enter the table number 959A on the keypad
2. Click the filtered table button to perform the search
3. Observe which ticket the screen scrolls to

**Test Result(s):**
1. The latest created ticket (the one created at 12:11) is searched and scrolled to
2. The earlier created ticket (the one created at 12:10) is not the search target

#### 3.3.Verify the Filtered Table List Is Scrollable When Many Tables Exist
**Prerequisite(s):**
1. A large number of tables with tickets exist on the KDS ticket view under the currently active statuses

**Step(s):**
1. Tap the search icon to open the search menu
2. Enter a common digit that matches many table numbers
3. Review the filtered table list on the right-hand side
4. Scroll through the list and select tables from different positions

**Test Result(s):**
1. When there are too many tables existing, the filtered table list is a scrollable list
2. All matching tables can be reached by scrolling and each table button scrolls to its target ticket correctly

### 4.Verify "No tickets found" Message Is Shown When No Table Number Matches
**Prerequisite(s):**
1. The KDS ticket view shows tickets under the currently active statuses
2. The search menu is opened

**Step(s):**
1. Enter a table number on the keypad that matches no ticket in the active statuses
2. Observe the filtered result area

**Test Result(s):**
1. No table button is matched by the filter number
2. The result area shows the empty state with the text "No tickets found"

## Test Environment

- **Test Environment**: Local test environment
- **Test Devices**: KDS 2.0 kitchen station displays
- **Component**: KDS
- **Issue Type**: New Feature
- **QC Tester**: Orianna Wen (orianna.wen@shijigroup.com)
- **QC Reviewer**: Kiet Zou (kiet.zou@shijigroup.com)
