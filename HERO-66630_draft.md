# MENU003 Item Maintenance Report - Department/Category Changes Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Item Maintenance Report |
| Code | MENU003 |
| Issue Key | HERO-66630 |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Business Date Range | Filter item maintenance by date | Today, Yesterday, This Week, Custom | Today |
| Outlet | Scope of item data | All Outlets, Single Outlet | All Outlets |
| Department | Filter by department | All, Specific Department | All |
| Category | Filter by category | All, Specific Category | All |
| **Include changes for Department/Category** | **NEW PARAMETER - Show Department/Category change details** | **Yes, No** | **No** |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Item Code | Unique item identifier | Sourced from menu master |
| Item Name | Display name of item | Sourced from menu master |
| Department (Current) | Current department assignment | Sourced from menu item configuration |
| Category (Current) | Current category assignment | Sourced from menu item configuration |
| **Department/Category (From)** | **NEW - Department/Category value before change** | **Sourced from Audit Log department/category change records** |
| **Department/Category (To)** | **NEW - Department/Category value after change** | **Sourced from Audit Log department/category change records** |
| **Change Timestamp** | **NEW - When the change occurred** | **Sourced from Audit Log timestamp** |
| **Change Source** | **NEW - How the change was made (bulk import, API, HQ, POS)** | **Derived from audit log source indicator** |

## Report Data Accuracy Verification

#### 1. New Parameter Visible with Correct Default Value

**Prerequisite(s):**
1. Access to MENU003 report parameters screen
2. User has permission to run MENU003

**Step(s):**
1. Open MENU003 Item Maintenance Report
2. Navigate to the parameters screen
3. Locate the new "Include changes for Department/Category" parameter
4. Check the available options
5. Verify the default value

**Test Result(s):**
1. New parameter "Include changes for Department/Category" is present
2. Available options are "Yes" and "No"
3. Default value is "No"
4. Parameter is accessible to all users who can run MENU003

#### 2. Parameter = No - Existing Behavior Preserved

**Prerequisite(s):**
1. MENU003 parameter "Include changes for Department/Category" set to No
2. Baseline MENU003 report output from before this enhancement

**Step(s):**
1. Open MENU003 and ensure "Include changes for Department/Category" = No
2. Select all other parameters at default values
3. Run the report
4. Compare output with baseline (pre-enhancement) MENU003

**Test Result(s):**
1. Report layout, columns, and output are IDENTICAL to baseline
2. No "Department/Category (From)" or "(To)" columns appear
3. All items in date range are displayed regardless of whether they had changes
4. No regression introduced by the enhancement

#### 3. Parameter = Yes - From/To Columns Displayed

**Prerequisite(s):**
1. MENU003 parameter "Include changes for Department/Category" set to Yes
2. Test environment with items that have had Department/Category changes in the report date range
3. Audit Log access to verify change records

**Step(s):**
1. Set "Include changes for Department/Category" = Yes
2. Select a date range containing known Department/Category changes
3. Run MENU003
4. Verify new columns appear: "Department/Category (From)" and "Department/Category (To)"
5. Cross-check From/To values with Audit Log records

**Test Result(s):**
1. Report includes "Department/Category (From)" column
2. Report includes "Department/Category (To)" column
3. From values match the pre-change values in Audit Log
4. To values match the post-change values in Audit Log
5. Change timestamps match Audit Log timestamps

#### 4. One Row Per Change Event

**Prerequisite(s):**
1. An item known to have undergone 3 separate Department/Category changes in the report date range

**Step(s):**
1. Run MENU003 with "Include changes for Department/Category" = Yes
2. Locate the item that changed 3 times
3. Count the rows for this item
4. Verify From/To values form a chain of correct transitions

**Test Result(s):**
1. The item appears as 3 separate rows, one per change event
2. Each row shows correct From (before) and To (after) values
3. Change timestamps match Audit Log for each event
4. Changes outside the report date range are excluded from results

#### 5. Changes from All Sources Captured

**Prerequisite(s):**
1. Test data with Department/Category changes made via all four sources: bulk import, API, HQ back-office, POS Item Maintenance
2. "Include changes for Department/Category" = Yes

**Step(s):**
1. Run MENU003 for the date range containing all changes
2. Check the "Change Source" column for each displayed change
3. Verify changes from each source type are present in the report

**Test Result(s):**
1. Changes made via bulk import are included
2. Changes made via API calls are included
3. Changes made via HQ back-office are included
4. Changes made via POS Item Maintenance are included
5. Change source is correctly identifiable in the report output

#### 6. Only Items with Changes Are Shown (When Enabled)

**Prerequisite(s):**
1. "Include changes for Department/Category" = Yes
2. Items that did NOT change Department/Category in the date range
3. Items that DID change

**Step(s):**
1. Run MENU003 with parameter enabled
2. Count total rows in the report
3. Compare with the known count of changed items in the date range

**Test Result(s):**
1. Only items that had at least one Department/Category change in the date range are shown
2. Items with no changes in the date range do not appear in the result
3. Row count matches the number of change events in the date range

## Report Functional Verification

#### 7. New Columns in All Output Formats

**Prerequisite(s):**
1. "Include changes for Department/Category" = Yes
2. Print, Excel, CSV, and PDF export functions available

**Step(s):**
1. Run MENU003 and view on-screen preview - verify new columns present
2. Print the report - verify new columns in printed output
3. Export to Excel - open file and verify columns and values
4. Export to CSV - open file and verify columns and values
5. Export to PDF - verify new columns present

**Test Result(s):**
1. On-screen preview: both new columns visible with correct values
2. Printed output: both new columns printed correctly
3. Excel export: columns included with proper headers, values match screen
4. CSV export: columns included, values correct
5. PDF export: columns present in correct positions
6. All output formats show consistent From/To values

#### 8. Empty Result Set Shows Column Headers

**Prerequisite(s):**
1. "Include changes for Department/Category" = Yes
2. A date range known to have NO Department/Category changes

**Step(s):**
1. Run MENU003 with the no-change date range
2. Check the report output
3. If report is empty, verify column headers are still visible
4. Try export for the empty result set

**Test Result(s):**
1. Report displays empty result set when no changes exist
2. Column headers including the two new columns are still shown
3. Exporting empty result still includes all column headers including From/To
4. No error messages for the empty case

#### 9. Interaction with Other MENU003 Parameters

**Prerequisite(s):**
1. All MENU003 parameters available
2. Items with Department/Category changes across different departments/categories

**Step(s):**
1. Run MENU003 with parameter enabled + Single Department filter
2. Run MENU003 with parameter enabled + Category filter
3. Run MENU003 with parameter enabled + Single Outlet filter
4. Verify that filtering works correctly with the new columns

**Test Result(s):**
1. Department filter works correctly - only relevant items shown
2. Category filter works correctly - only relevant items shown
3. Outlet filter works correctly - only relevant items shown
4. New columns work harmoniously with all existing MENU003 parameters
