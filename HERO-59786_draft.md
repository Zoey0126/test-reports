# HERO-59786 POS Reports - Government Uniform Invoice Report Slip Format Additional Enhancements Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Government Uniform Invoice Report (Slip Format) |
| Code | POS289 |
| Type | Improvement |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Business Date | Select the business date for the report | [Date Picker] | Current Date |
| Stations | Filter by station(s) | All Stations, Single Station, Multiple Stations | All Stations |
| Cashiers | Filter by cashier(s) | All Cashiers, Single Cashier, Multiple Cashiers | All Cashiers |
| Void Checks | Include voided checks in the report | Checkbox (Checked/Unchecked) | Checked |
| Period / Cashier and Shift | Group by period, cashier, and shift | All, Single, Multiple | All |
| Period | Filter by period | All Periods, Single Period, Multiple Periods | All Periods |
| Shift | Filter by shift | All Shifts, Single Shift, Multiple Shifts | All Shifts |
| Shift Number | Filter by shift number | All, Single, Multiple | All |
| Sort column | Sort order of report data | [Column Selection] | Default |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| GUI Number | Government Uniform Invoice number | Direct display |
| Check Number | POS check number associated with the GUI | Direct display |
| Void Status | Indicates whether the GUI/check is voided | Direct display |
| Amount | Transaction amount | Direct display |
| Date/Time | Transaction date and time | Direct display |
| Cashier | Cashier who processed the transaction | Direct display |
| Station | Station where the transaction occurred | Direct display |

## Report Data Accuracy Verification

### 1. All Default Parameters

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Select "default" from all parameters
    2.1. Select current business date from Business Date
    2.2. Select All Stations from Stations
    2.3. Select All Cashiers from Cashiers
    2.4. Keep Void Checks checkbox checked
    2.5. Select All from Period / Cashier and Shift
    2.6. Select All Periods from Period
    2.7. Select All Shifts from Shift
    2.8. Select All from Shift Number
    2.9. Select Default sort from Sort column
  3. Click "Run" button
  4. Verify report displays only voided GUI records

**Test Result(s):**
  1. Report loads successfully
  2. Report displays only voided GUI records (non-voided GUI are not shown)
  3. No cross-out rows displayed in red color
  4. No errors are displayed

### 2. Verify Void Checks Checkbox Filters Voided GUI Only

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Keep Void Checks checkbox checked
  3. Run the report
  4. Observe the displayed records
  5. Uncheck the Void Checks checkbox
  6. Run the report again
  7. Observe the displayed records

**Test Result(s):**
  1. When Void Checks is checked, report displays only voided GUI records
  2. When Void Checks is unchecked, report displays no records (or behaves as designed for non-voided)
  3. No cross-out rows in red color are displayed in either case
  4. No errors are displayed

### 3. Verify "Skip GUI" Column is Removed

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Run the report with default parameters
  3. Examine all column headers in the report output
  4. Check if "Skip GUI" column exists

**Test Result(s):**
  1. The "Skip GUI" column is not present in the report output
  2. All other required columns are displayed correctly
  3. Report layout is clean without the removed column
  4. No errors are displayed

### 4. Verify Report Criteria Matches POS123

**Step(s):**
  1. Open POS123 report and note all available criteria/parameters
  2. Open Government Uniform Invoice Report (POS289)
  3. Compare the criteria/parameters of POS289 with POS123
  4. Verify the following criteria exist in POS289:
    - Stations
    - Cashiers
    - Void Checks checkbox
    - Period / Cashier and Shift
    - Period
    - Shift
    - Shift Number
    - Sort column

**Test Result(s):**
  1. POS289 has all the same criteria as POS123
  2. Stations parameter is available with All/Single/Multiple options
  3. Cashiers parameter is available with All/Single/Multiple options
  4. Void Checks checkbox is available
  5. Period / Cashier and Shift parameter is available
  6. Period parameter is available with All/Single/Multiple options
  7. Shift parameter is available with All/Single/Multiple options
  8. Shift Number parameter is available
  9. Sort column parameter is available
  10. No missing criteria compared to POS123

### 5. Verify Stations Filter Works Correctly

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Select a single station from Stations parameter
  3. Run the report
  4. Verify all displayed records belong to the selected station
  5. Select multiple stations
  6. Run the report
  7. Verify records belong to the selected stations

**Test Result(s):**
  1. Report displays only voided GUI records from the selected station(s)
  2. Single station filter works correctly
  3. Multiple stations filter works correctly
  4. No errors are displayed

### 6. Verify Cashiers Filter Works Correctly

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Select a single cashier from Cashiers parameter
  3. Run the report
  4. Verify all displayed records belong to the selected cashier
  5. Select multiple cashiers
  6. Run the report
  7. Verify records belong to the selected cashiers

**Test Result(s):**
  1. Report displays only voided GUI records from the selected cashier(s)
  2. Single cashier filter works correctly
  3. Multiple cashiers filter works correctly
  4. No errors are displayed

### 7. Verify Period and Shift Filters Work Correctly

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Select a specific Period and Shift combination
  3. Run the report
  4. Verify records match the selected Period and Shift
  5. Select a specific Shift Number
  6. Run the report
  7. Verify records match the selected Shift Number

**Test Result(s):**
  1. Report displays only voided GUI records matching the selected Period
  2. Report displays only voided GUI records matching the selected Shift
  3. Report displays only voided GUI records matching the selected Shift Number
  4. No errors are displayed

### 8. Verify Sort Column Works Correctly

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Select a sort column from the Sort column parameter
  3. Run the report
  4. Verify records are sorted by the selected column
  5. Select a different sort column
  6. Run the report
  7. Verify records are sorted by the newly selected column

**Test Result(s):**
  1. Report data is sorted correctly by the selected column
  2. Sort order changes correctly when a different column is selected
  3. No errors are displayed

## Report Functional Verification

### 1. Export Report to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Run the report with default parameters
  3. Select "Export" from the menu
  4. Select each format: CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)
  5. Click "OK" button for each format
  6. Verify the exported file is saved

**Test Result(s):**
  1. Report exports successfully to all supported formats
  2. Exported files contain only voided GUI records
  3. Exported files do not contain the "Skip GUI" column
  4. No errors are displayed

### 2. Print Report to HTML, PDF

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Run the report with default parameters
  3. Select "Print" from the menu
  4. Print to HTML
  5. Print to PDF

**Test Result(s):**
  1. Report prints successfully to HTML and PDF
  2. Printed output contains only voided GUI records
  3. Printed output does not contain the "Skip GUI" column
  4. No cross-out rows in red color in the printed output
  5. No errors are displayed

### 3. Multi-language Support for Report Parameter and Data

**Step(s):**
  1. Set POS language to a non-English language (e.g., Chinese)
  2. Open Government Uniform Invoice Report (POS289)
  3. Verify all parameter names are translated correctly
  4. Run the report and verify data display

**Test Result(s):**
  1. All parameter names (Stations, Cashiers, Void Checks, Period, Shift, Shift Number, Sort column) are translated correctly
  2. Report data is displayed correctly in the selected language
  3. No garbled text or translation errors
  4. No errors are displayed

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289) in Google Chrome
  2. Select default parameters and run the report
  3. Verify report loads correctly
  4. Repeat in Mozilla Firefox
  5. Repeat in Microsoft Edge

**Test Result(s):**
  1. Report loads successfully in all three browsers
  2. Report displays only voided GUI records in all browsers
  3. "Skip GUI" column is absent in all browsers
  4. All report criteria are available in all browsers
  5. No errors are displayed

### 2. Support Data Service Compatibility - MySQL and TiDB

**Step(s):**
  1. Open Government Uniform Invoice Report (POS289)
  2. Set date range to less than 7 days (MySQL)
  3. Run the report and verify data
  4. Set date range to more than 7 days (TiDB)
  5. Run the report and verify data

**Test Result(s):**
  1. Report retrieves data correctly from MySQL for date ranges less than 7 days
  2. Report retrieves data correctly from TiDB for date ranges more than 7 days
  3. Only voided GUI records are displayed regardless of data source
  4. No errors are displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
