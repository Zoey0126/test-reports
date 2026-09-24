# HERO-68245 POS160 Cashier Settlement Report(By Outlet) - Missing Outlet filtering condition in Business Day data retrieval Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | POS160 Cashier Settlement Report(By Outlet) |
| Code | POS160 |
| Issue Key | HERO-68245 |
| Issue Type | Bug |

## Bug Description

POS160 Cashier Settlement Report (By Outlet) is missing the Outlet filtering condition when retrieving Business Day data. This causes the report to include data from outlets that should not be included in the filtered results.

## Regression Test Scope

The following related modules and functions should be covered in regression testing:
- POS160 Cashier Settlement Report (By Outlet) parameter filtering
- Business Day data retrieval logic for all outlet-specific reports
- Outlet parameter applicability in Cashier Settlement reports
- Other Cashier Settlement report variants (if any)

## Test Cases

### 1. Verify Outlet Filter is Applied for Business Day Data Retrieval

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. Multiple outlets exist with Business Day data
  3. User has permission to run POS160 report

**Step(s):**
  1. Open Infrasys Cloud and navigate to the report module
  2. Select report POS160 "Cashier Settlement Report (By Outlet)"
  3. Select a specific outlet from the "Outlet" parameter
  4. Select a Business Day range that contains data for multiple outlets
  5. Run the report
  6. Verify the report data

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: Business Day data retrieval does not apply the selected outlet filter, causing data from all outlets to be included in the result
  3. The report displays settlement records for outlets other than the selected one

**Fix Result(s):**
  1. Report runs successfully
  2. After fix: Business Day data retrieval correctly applies the selected outlet filter
  3. Only settlement records for the selected outlet are displayed
  4. No data from unselected outlets appears in the report

### 2. Verify Report with All Outlets Selected

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. Multiple outlets exist with Business Day data

**Step(s):**
  1. Open Infrasys Cloud and navigate to the report module
  2. Select report POS160 "Cashier Settlement Report (By Outlet)"
  3. Select "All Outlets" from the "Outlet" parameter
  4. Select a Business Day range
  5. Run the report
  6. Verify the report includes data for all outlets

**Reproduction Result(s):**
  1. Report runs without error
  2. Data for all outlets is displayed

**Fix Result(s):**
  1. Report runs successfully
  2. All outlets' settlement data is correctly retrieved and displayed
  3. No errors displayed

### 3. Verify Report with Single Outlet Selected

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. At least one outlet has Business Day data

**Step(s):**
  1. Open Infrasys Cloud and navigate to the report module
  2. Select report POS160 "Cashier Settlement Report (By Outlet)"
  3. Select a single specific outlet from the "Outlet" parameter
  4. Select a Business Day range
  5. Run the report
  6. Verify the report only contains data for the selected outlet

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: report may contain data from other outlets due to missing filter condition

**Fix Result(s):**
  1. Report runs successfully
  2. Only the selected outlet's settlement data is displayed
  3. No data from other outlets is present
  4. No errors displayed

### 4. Verify Report with Multiple Outlets Selected

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. Multiple outlets exist with Business Day data

**Step(s):**
  1. Open Infrasys Cloud and navigate to the report module
  2. Select report POS160 "Cashier Settlement Report (By Outlet)"
  3. Select multiple specific outlets from the "Outlet" parameter
  4. Select a Business Day range
  5. Run the report
  6. Verify the report only contains data for the selected outlets

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: report may contain data from unselected outlets due to missing filter condition

**Fix Result(s):**
  1. Report runs successfully
  2. Only the selected outlets' settlement data is displayed
  3. No data from unselected outlets is present
  4. No errors displayed

### 5. Verify Report Data Accuracy After Fix

**Prerequisite(s):**
  1. Access to Infrasys Cloud report module
  2. Known Business Day data exists for specific outlets

**Step(s):**
  1. Open Infrasys Cloud and navigate to the report module
  2. Select report POS160 "Cashier Settlement Report (By Outlet)"
  3. Select a single outlet with known settlement data
  4. Select the Business Day corresponding to the known data
  5. Run the report
  6. Compare the report output with the expected settlement values

**Reproduction Result(s):**
  1. Report runs without error
  2. Before fix: data may be incorrect due to inclusion of other outlets' data

**Fix Result(s):**
  1. Report runs successfully
  2. Report data matches the expected settlement values for the selected outlet
  3. No discrepancies in totals or line items
  4. No errors displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
