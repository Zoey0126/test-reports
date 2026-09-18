# CUS381 Menu Item Sale Report (with Employee Details) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Menu Item Sale Report (with Employee Details) |
| Code | CUS381 |
| Issue Key | HERO-67779 |
| Base Report | POS153 – Menu Item Sale Report |
| Issue Type | Improvement |
| Status | READY FOR QA |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Filter report by outlet scope | All Outlets, A Single Outlet, Multiple Outlets | All Outlets |
| Business Date | Select the business date range | Today, Yesterday, This Week, This Month, Custom | Today |
| Period | Filter report by period | All, A Single Period, Multiple Periods | All |
| Revenue/Non-Revenue | Filter transactions by revenue classification (derived from Non Revenue Payment configuration on payment method) | All, Revenue, Non-Revenue, Revenue+Non-Revenue, Advance Order | All |
| Show Cost | Display the Cost column in the report | Ticked, Unticked | Ticked |
| Show Table Number | Display the Table Number column in the report | Ticked, Unticked | Ticked |
| Group By Outlet | Group report output by outlet | Grouping, No Grouping | Grouping |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Employee Name | Employee/User who closed the bill, populated for every transaction regardless of payment type. Blank if no employee was selected at bill closure. | Sourced from bill closure user (refer to POS114 – Payment Employee Name) |
| Reference | Pay Reference manually entered by the operator at bill closure, populated for all payment types. Blank if no reference was manually entered. | Sourced from bill closure pay reference (refer to POS114 – Pay Reference) |
| Cost | Cost of menu items sold | Inherited from POS153 |
| Table Number | Table number associated with the transaction | Inherited from POS153 |
| Revenue Classification | Transaction revenue classification based on Non Revenue Payment configuration on the payment method (Revenue = NO, Non-Revenue = YES) | Derived from Non Revenue Payment configuration |

### Report Data Accuracy Verification

#### 1. All Default Parameters
**Prerequisite(s):**
  1. Ensure bills with Duty Meal payment type have an employee/user selected at bill closure.
  2. Ensure bills with any payment type have a pay reference manually entered at bill closure.
  3. Ensure payment methods are configured with Non Revenue Payment = NO (Revenue) and Non Revenue Payment = YES (Non-Revenue).
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "default" from all parameters
    2.1. Select "All Outlets" from Shops/Outlets
    2.2. Select "Today" from Business Date
    2.3. Select "All" from Period
    2.4. Select "All" from Revenue/Non-Revenue
    2.5. Verify "Show Cost" is ticked by default
    2.6. Verify "Show Table Number" is ticked by default
    2.7. Select "Grouping" from Group By Outlet
  3. Click "Run" button
  4. Verify the report displays data for all default parameter combinations
  5. Verify the Employee Name column displays the employee/user who closed the bill for every transaction
  6. Verify the Reference column displays the pay reference manually entered by the operator at bill closure
  7. Verify transactions where Non Revenue Payment = NO and Non Revenue Payment = YES are both included (All option)
  8. Verify transactions whose payment method has no Non Revenue Payment configuration are excluded from Revenue and Non-Revenue filtered results but included under All
**Test Result(s):**
  1. Report loads successfully
  2. Data displayed is correct for expected parameter combinations
  3. Employee Name and Reference columns are populated correctly for matching transactions
  4. Transactions without employee or reference display blank values, not errors
  5. No errors displayed

#### 2. [Multiple Outlets, Yesterday Business Day, Multiple Periods, Revenue Type, No Show Cost, No Show Table Number, No Grouping By Outlet]
**Prerequisite(s):**
  1. Ensure bills with Duty Meal payment type have an employee/user selected at bill closure.
  2. Ensure bills with any payment type have a pay reference manually entered at bill closure.
  3. Ensure payment methods are configured with Non Revenue Payment = NO (Revenue).
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters
    2.1. Select "Multiple Outlets" from Shops/Outlets
    2.2. Select "Yesterday" from Business Date
    2.3. Select "Multiple Periods" from Period
    2.4. Select "Revenue" from Revenue/Non-Revenue
    2.5. Untick "Show Cost"
    2.6. Untick "Show Table Number"
    2.7. Select "No Grouping" from Group By Outlet
  3. Click "Run" button
  4. Verify the report displays data for selected parameter combinations
  5. Verify only Revenue transactions (Non Revenue Payment = NO) are included
  6. Verify Non-Revenue transactions (Non Revenue Payment = YES) are excluded
  7. Verify transactions whose payment method has no Non Revenue Payment configuration are excluded
  8. Verify the Employee Name and Reference columns are populated correctly
  9. Verify the Cost and Table Number columns are not displayed
**Test Result(s):**
  1. Report filters by selected parameters
  2. Data displayed is correct for expected parameter combinations
  3. Only matching Revenue transactions are included
  4. Cost and Table Number columns are hidden
  5. No errors displayed

#### 3. [A Single Outlet, This Week Business Day, A Single Period, Non Revenue Types, Show Cost, Show Table Number, Group By Outlet]
**Prerequisite(s):**
  1. Ensure bills with Duty Meal payment type have an employee/user selected at bill closure.
  2. Ensure bills with any payment type have a pay reference manually entered at bill closure.
  3. Ensure payment methods are configured with Non Revenue Payment = YES (Non-Revenue).
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters
    2.1. Select "A Single Outlet" from Shops/Outlets
    2.2. Select "This Week" from Business Date
    2.3. Select "A Single Period" from Period
    2.4. Select "Non-Revenue" from Revenue/Non-Revenue
    2.5. Verify "Show Cost" is ticked
    2.6. Verify "Show Table Number" is ticked
    2.7. Select "Grouping" from Group By Outlet
  3. Click "Run" button
  4. Verify the report displays data for selected parameter combinations
  5. Verify only Non-Revenue transactions (Non Revenue Payment = YES) are included
  6. Verify Revenue transactions (Non Revenue Payment = NO) are excluded
  7. Verify the Employee Name and Reference columns are populated correctly
**Test Result(s):**
  1. Report filters by selected parameters
  2. Data displayed is correct for expected parameter combinations
  3. Only matching Non-Revenue transactions are included
  4. Employee Name and Reference columns are populated correctly
  5. No errors displayed

#### 4. [All Outlets, This Month Business Day, All Periods, Revenue + Non-Revenue Types, Show Cost, Show Table Number, Group By Outlet]
**Prerequisite(s):**
  1. Ensure bills with Duty Meal payment type have an employee/user selected at bill closure.
  2. Ensure bills with any payment type have a pay reference manually entered at bill closure.
  3. Ensure payment methods are configured with both Non Revenue Payment = NO and Non Revenue Payment = YES.
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters
    2.1. Select "All Outlets" from Shops/Outlets
    2.2. Select "This Month" from Business Date
    2.3. Select "All" from Period
    2.4. Select "Revenue+Non-Revenue" from Revenue/Non-Revenue
    2.5. Verify "Show Cost" is ticked
    2.6. Verify "Show Table Number" is ticked
    2.7. Select "Grouping" from Group By Outlet
  3. Click "Run" button
  4. Verify the report displays data for selected parameter combinations
  5. Verify both Revenue (Non Revenue Payment = NO) and Non-Revenue (Non Revenue Payment = YES) transactions are included
  6. Verify transactions whose payment method has no Non Revenue Payment configuration are excluded
  7. Verify the Employee Name and Reference columns are populated correctly
**Test Result(s):**
  1. Report filters by selected parameters
  2. Data displayed is correct for expected parameter combinations
  3. Both Revenue and Non-Revenue transactions are included
  4. Transactions without Non Revenue Payment configuration are excluded
  5. No errors displayed

#### 5. [Multiple Outlets, Custom Business Day, Multiple Periods, Advance Order Types, Show Cost, Show Table Number, Group By Outlet]
**Prerequisite(s):**
  1. Ensure bills with Duty Meal payment type have an employee/user selected at bill closure.
  2. Ensure bills with any payment type have a pay reference manually entered at bill closure.
  3. Ensure advance order transactions exist for the selected business date range.
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters
    2.1. Select "Multiple Outlets" from Shops/Outlets
    2.2. Select "Custom" from Business Date and specify a valid custom date range
    2.3. Select "Multiple Periods" from Period
    2.4. Select "Advance Order" from Revenue/Non-Revenue
    2.5. Verify "Show Cost" is ticked
    2.6. Verify "Show Table Number" is ticked
    2.7. Select "Grouping" from Group By Outlet
  3. Click "Run" button
  4. Verify the report displays data for selected parameter combinations
  5. Verify only Advance Order transactions are included
  6. Verify the Employee Name and Reference columns are populated correctly
**Test Result(s):**
  1. Report filters by selected parameters
  2. Data displayed is correct for expected parameter combinations
  3. Only Advance Order transactions are included
  4. Employee Name and Reference columns are populated correctly
  5. No errors displayed

#### 6. Verify Existing POS153 Report Remains Unchanged
**Prerequisite(s):**
  1. Confirm the existing POS153 Menu Item Sale Report is accessible in the report list.
**Step(s):**
  1. Open the existing POS153 Menu Item Sale Report
  2. Select "default" from all parameters
  3. Click "Run" button
  4. Verify the report output retains the existing POS153 structure, columns, and reporting logic
  5. Verify the Employee Name and Reference columns are NOT added to POS153
  6. Verify the Revenue/Non-Revenue parameter is NOT available in POS153
  7. Verify "Show Cost" and "Show Table Number" remain unticked by default in POS153
**Test Result(s):**
  1. POS153 report loads successfully
  2. POS153 structure, columns, and reporting logic remain unchanged
  3. New enhancements are NOT applied to POS153
  4. No errors displayed

### Report Functional Verification

#### 1. Verify Report Information with Format Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration
  2. Set the desired date format for the report
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "Language" from the menu
  3. Select the desired language from the dropdown menu
  4. Verify the language is set successfully
  5. Verify the report displays data in the selected language
**Test Result(s):**
  1. Language is set successfully
  2. Report displays data in the selected language
  3. No errors displayed

#### 2. Export Files to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "Export" from the menu
  3. Select the desired format from the dropdown menu
  4. Click "OK" button
  5. Verify the exported file is saved to the specified location
  6. Verify the Employee Name and Reference columns appear in the exported file
**Test Result(s):**
  1. Exported file is successful
  2. Exported file is saved to the specified location
  3. Exported file format matches the selected format
  4. Exported file contains the expected data including the new Employee Name and Reference columns

#### 3. Print Report to HTML, PDF
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "Print" from the menu
  3. Verify the printed report is saved to the specified location
  4. Verify the Employee Name and Reference columns appear in the printed report
**Test Result(s):**
  1. Printed report is successful
  2. Printed report is saved to the specified location
  3. Printed report format matches the selected format
  4. Printed report contains the expected data including the new Employee Name and Reference columns

#### 4. Create Preset Parameters from Current Parameters
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "Preset Parameters" from the menu
  3. Click "OK" button
  4. Input the relevant information for the preset parameters
  5. Click "Save" button
  6. Verify the preset parameters are set successfully
  7. Verify the preset parameters are displayed in the report
  8. Open the preset parameters's Report from the list
  9. Verify the report displays data for the preset parameters
  10. Verify the Employee Name and Reference columns are retained in the preset parameter output
**Test Result(s):**
  1. Preset parameters are set successfully
  2. Report filters by the preset parameters
  3. Data displayed is correct for the preset parameters
  4. Employee Name and Reference columns are retained
  5. No errors displayed

#### 5. Edit Original Preset Parameters
**Step(s):**
  1. Go to Preset Parameters
  2. Select the preset parameters to edit
  3. Reselect the parameters
  4. Click "OK" button
  5. Select "Preset Parameters" from the menu
  6. Input the relevant information for the preset parameters
  7. Click "Save" button
  8. Verify the preset parameters are set successfully
  9. Verify the preset parameters are displayed in the report
  10. Open the preset parameters's Report from the list
  11. Verify the report displays data for the preset parameters
**Test Result(s):**
  1. Preset parameters are set successfully
  2. Report filters by the preset parameters
  3. Data displayed is correct for the preset parameters
  4. No errors displayed

#### 6. Run Preset Parameters from Original or Current Parameters
**Step(s):**
  1. Open the preset parameters's Report from the list
  2. Verify the report displays data for the preset parameters
**Test Result(s):**
  1. Data displayed is correct for the preset parameters
  2. No errors displayed

#### 7. Auto Run Preset Parameters to Email, FTP, or SFTP with Specified Auto Report Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Auto Report Configuration
  2. Set the desired auto report configuration for the report
**Step(s):**
  1. Add new or edit auto report schedule
  2. Select the preset parameters to run
  3. Select the desired destination for the report
  4. Verify the auto report schedule is set successfully
**Test Result(s):**
  1. Auto report schedule is set successfully
  2. Report is sent successfully to the specified destination, time, format and other settings
  3. Data is correct for the preset parameters
  4. No errors displayed

#### 8. Multi-language Support for Report Parameter and Data
**Step(s):**
  1. Select "Language"
  2. Open Menu Item Sale Report (with Employee Details)
  3. Verify the language is set successfully
  4. Verify the report displays data in the selected language
  5. Verify the new Employee Name and Reference column headers are translated correctly
**Test Result(s):**
  1. Language is set successfully
  2. Report displays data in the selected language
  3. New column headers are translated correctly
  4. No errors displayed

### Report Compatibility Verification

#### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select "default" from all parameters
    2.1. Select "All Outlets" from Shops/Outlets
    2.2. Select "Today" from Business Date
    2.3. Select "All" from Period
    2.4. Select "All" from Revenue/Non-Revenue
    2.5. Verify "Show Cost" is ticked
    2.6. Verify "Show Table Number" is ticked
    2.7. Select "Grouping" from Group By Outlet
  3. Verify the report loads successfully on Google Chrome, Mozilla Firefox, and Microsoft Edge
**Test Result(s):**
  1. Report loads successfully on all supported browsers
  2. No errors displayed

#### 2. Support Config Zone Compatibility
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters from Report Data Accuracy Verification
  3. Verify the report loads successfully across different Config Zone data hierarchies (current level, lower level, upper level, all levels including lower level)
**Test Result(s):**
  1. Report loads successfully
  2. Data displayed is correct across different Config Zone hierarchies
  3. No errors displayed

#### 3. Support Data Service Compatibility - MySQL
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters from Report Data Accuracy Verification, and set date range to less than 7 days
  3. Verify the report loads successfully
**Test Result(s):**
  1. Report loads successfully
  2. No errors displayed
  3. Data displayed is correct for expected parameter combinations

#### 4. Support Data Service Compatibility - TiDB
**Step(s):**
  1. Open Menu Item Sale Report (with Employee Details)
  2. Select parameters from Report Data Accuracy Verification, and set date range to more than 7 days
  3. Verify the report loads successfully
**Test Result(s):**
  1. Report loads successfully
  2. No errors displayed
  3. Data displayed is correct for expected parameter combinations

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## QC Information
- **Tester**: Yumia Tang (yumia.tang@shijigroup.com)
- **Reviewer**: Kathy Kuang (kathy.kuang@shijigroup.com)
