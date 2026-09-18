# MENU003 Item Maintenance Report - Add Optional Department/Category Change Columns Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Item Maintenance Report |
| Code | MENU003 |
| Issue Key | HERO-66630 |
| Issue Type | Improvement |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Include changes for Department/Category | Control whether Department/Category change details are displayed | Yes, No | No |
| Business Dates | Report date range used to select item maintenance / change events | Date range | Current period |
| Shops/Outlets | Filter by shop or outlet | All Outlets, A Single Outlet, Multiple Outlets | All Outlets |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Department/Category (From) | Department/Category value before the change | From existing item maintenance / Audit Log change record |
| Department/Category (To) | Department/Category value after the change | From existing item maintenance / Audit Log change record |
| Change timestamp | Time of the Department/Category change event | From Audit Log |
| Change source | Source of the change when identifiable | Bulk import, API, HQ back-office, POS Item Maintenance |

## Report Data Accuracy Verification

### 1.New Parameter Default And Availability

#### 1.1.Parameter Present With Default No
**Prerequisite(s):**
1. User has permission to run MENU003

**Step(s):**
1. Open MENU003 Item Maintenance Report
2. View the available parameter options
3. Observe "Include changes for Department/Category"

**Test Result(s):**
1. The new parameter labeled "Include changes for Department/Category" is present
2. The default value is No
3. The parameter is accessible to all users who can run MENU003

### 2.Parameter Disabled Preserves Existing Behavior
**Prerequisite(s):**
1. Item maintenance records exist in the report date range, including items with and without Department/Category changes

**Step(s):**
1. Open MENU003
2. Leave "Include changes for Department/Category" = No
3. Run the report
4. Check on-screen, print and export output

**Test Result(s):**
1. Report layout, columns and output are identical to the current MENU003 behavior
2. Columns "Department/Category (From)" and "Department/Category (To)" do not appear in any output format
3. All items within the report date range are displayed regardless of whether they had Department/Category changes
4. No errors displayed

### 3.Parameter Enabled Displays Change Columns

#### 3.1.All Default Parameters Except Include Changes = Yes
**Prerequisite(s):**
1. At least one item had a Department/Category change within the report date range
2. Matching Audit Log records exist

**Step(s):**
1. Open MENU003
2. Select default values for other parameters
3. Set "Include changes for Department/Category" = Yes
4. Click "Run"
5. Verify on-screen columns and values against Audit Log

**Test Result(s):**
1. Report includes "Department/Category (From)" and "Department/Category (To)" columns
2. From/To values match the corresponding Audit Log change records
3. Only items that had at least one Department/Category change within the date range are shown
4. No errors displayed

#### 3.2.One Row Per Change Event
**Prerequisite(s):**
1. An item had its Department/Category changed three times within the report date range
2. At least one change event exists outside the report date range

**Step(s):**
1. Set "Include changes for Department/Category" = Yes
2. Set the date range to include the three in-range changes and exclude the out-of-range event
3. Run MENU003
4. Count rows for that item and compare From/To and timestamps with Audit Log

**Test Result(s):**
1. The item appears as three separate rows, one per in-range change event
2. Each row shows the value before the change in From and the value after the change in To
3. Each row includes the timestamp of the change event
4. The out-of-range change event is excluded

#### 3.3.Empty Result When No Changes In Range
**Prerequisite(s):**
1. No item in the selected date range had a Department/Category change

**Step(s):**
1. Set "Include changes for Department/Category" = Yes
2. Run MENU003 for that date range

**Test Result(s):**
1. The report displays an empty result set
2. The new column headers remain visible
3. No errors displayed

### 4.Changes From All Sources Are Captured

#### 4.1.Bulk Import, API, HQ And POS Item Maintenance
**Prerequisite(s):**
1. Department/Category changes exist from bulk import, API, HQ back-office and POS Item Maintenance within the date range

**Step(s):**
1. Set "Include changes for Department/Category" = Yes
2. Run MENU003
3. Locate one change from each source
4. Compare with Audit Log

**Test Result(s):**
1. Changes made through bulk import are included
2. Changes made through API calls are included
3. Changes made through HQ back-office are included
4. Changes made through POS Item Maintenance are included
5. The source of each change is identifiable in the report output when the audit data provides it

## Report Functional Verification

### 1.Columns Appear In Print And Export
**Prerequisite(s):**
1. "Include changes for Department/Category" = Yes
2. At least one change row is available

**Step(s):**
1. Run MENU003
2. Verify columns on screen
3. Print the report
4. Export to CSV, Excel and PDF
5. Verify From/To columns in each format

**Test Result(s):**
1. Columns appear in on-screen report view
2. Columns appear in printed output
3. Columns appear in CSV, Excel and PDF
4. From/To values are consistent across formats

### 2.Existing Preset Parameters Still Run
**Prerequisite(s):**
1. A previously saved MENU003 preset exists from before this enhancement

**Step(s):**
1. Run the old preset
2. Confirm Include changes for Department/Category behaves as No when not stored

**Test Result(s):**
1. Old preset still runs
2. Existing layout is preserved when the new parameter is No / unset
3. No errors displayed

## Report Compatibility Verification

### 1.Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Step(s):**
1. Open MENU003 in Chrome, set Include changes = Yes and run
2. Repeat in Firefox
3. Repeat in Edge

**Test Result(s):**
1. Report loads successfully in all three browsers
2. New parameter and columns display correctly
3. No errors displayed

### 2.Support Data Service Compatibility - MySQL and TiDB
**Step(s):**
1. Run MENU003 with a date range of less than 7 days
2. Run MENU003 with a date range of more than 7 days
3. Enable Include changes for Department/Category in both runs

**Test Result(s):**
1. Report loads successfully on both short and long date ranges
2. From/To change data is correct
3. No errors displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## Appendix

### Acceptance Criteria(from JIRA)
1. MENU003 provides a new parameter to control whether Department/Category changes are included; default is No
2. When disabled, MENU003 keeps the current layout and output logic
3. When enabled, MENU003 displays Department/Category (From) and (To) columns
4. From/To values match the actual change record shown in Audit Log
5. One row per change event; events outside the date range are excluded
6. Changes from bulk import, API, HQ back-office and POS Item Maintenance are captured
7. Columns appear in on-screen, printed and exported (CSV/Excel/PDF) output
