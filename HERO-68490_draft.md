# POS305 PH BIR Sales Summary Report V2 Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | PH BIR Sales Summary Report V2 |
| Code | POS305 |
| Issue Key | HERO-68490 |
| Issue Type | New Feature |
| Status | READY FOR QA |
| Summary | POS305 PH BIR Sales Summary Report V2 - Layout, labeling and Z-Counter enhancements |
| Components | POS |
| Region | Philippines (changes apply only to PH version; other regions keep current layout) |

## Requirement Baseline

Use the existing POS305 PH BIR Sales Summary Report V2 as the baseline and implement the following enhancements:

1. **Grand Accum. Ending Balance**: copy the amount from the Gross Amount column (same value).
2. **Deductions and Discounts**: remove all negative signs; display as positive amounts (zero stays 0 with no sign).
3. **Service Charge column header**: rename from "Service Charge (Without VAT)" to "Service Charge".
4. **Column Reordering**: move Service Charge and Local Tax columns to appear immediately after Net Amount (target order: Net Amount -> Service Charge -> Local Tax).
5. **Z-Counter**: number of daily close count in End Of Day (Business Day). When the report date range covers multiple dates, Z-Counter = number of daily closes within the selected date range. Increments only after a fully successful EOD; resets to zero at the start of each business day.
6. **Region scope**: changes apply only to the Philippines version of the POS305 report; other regions keep the current layout and labeling.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | Select the business day scope (single-select). Begin Date and End Date are enabled when 'Specified As Below' is selected. Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Current Business Day, Last Business Day, Last Month, Specified As Below | Current Business Day |
| Begin Date | Specify the begin business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| End Date | Specify the end business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| Shops | Filter data by shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Outlets | Filter data by outlet(s) under the selected shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Stations | Filter data by station(s) (multi-select) | All, Single, Multiple | All |
| Payment Classes | Filter data by payment class(es) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Mapping Configuration: Payment Mapping, Default: Empty, No Limit | All, Single, Multiple | All |

## Data Fields to Validate

### Report Header

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Title | Show corresponding report title by selection [Shop Name / Shop Address / Tax ID in Shop / Report Name] | - |
| Date Range | Show specified date range. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD | - |
| Software Version | Show software name and version | - |
| Date And Time | Show print date and time. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD; Time Format, Default Time Format: 24-hour clock | - |
| User ID | Show print user first name and last name | - |

### Sales Summary

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Date | Show business day. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD | - |
| Beginning | Show the start payment running number | - |
| Ending | Show the end payment running number | - |
| Grand Accum. Beg. Balance | Accumulated grand total from previous business day | Grand Accum. Beg. Balance = Ending Balance - Net Sales |
| Grand Accum. Ending Balance | **CHANGED**: Now follows the existing 'Gross Amount' column value (copies the Gross Amount of the day). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Grand Accum. Ending Balance = Gross Amount of the day |
| Gross Amount | Show item sales price; for each check: if VAT = 0 and Deductions != 0, Gross Amount = VATable + VAT + VAT-Exempt + Zero-Rated; if Tax1 != 0, Gross Amount = Check Item Total + VAT; else Gross Amount = Check Item Total - Tax2 to Tax25 - Service Charge | - |
| VATable | Show check item total add all discount | - |
| VAT | Show tax less any discount excluding SC & PWD discount (only Tax Index 1) | - |
| VAT-Exempt | Show check item total of whole day for waived tax1 items | - |
| Zero-Rated | Show check item total of whole day for waived tax1 items when VAT-Exempt has no amount | - |

### Deductions (CHANGED - all values displayed as positive; no negative signs)

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Discount | Show discount total. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | - |
| SC | Show SC discount [Discount Group Short Name is 'SC']; **CHANGED**: displayed as positive (no negative sign) | - |
| PWD | Show PWD discount [Discount Group Short Name is 'PWD']; **CHANGED**: displayed as positive (no negative sign) | - |
| NAAC | Show NAAC discount [Discount Group Short Name is 'NAAC']; **CHANGED**: displayed as positive (no negative sign) | - |
| Solo Parent | Show SP discount [Discount Group Short Name is 'SP']; **CHANGED**: displayed as positive (no negative sign) | - |
| Others | Show all discounts excluding SC, PWD, NAAC & SP; **CHANGED**: displayed as positive (no negative sign) | - |
| Returns | Show blank | - |
| Voids | Show deleted item in check; **CHANGED**: displayed as positive (no negative sign) | - |
| Total Deductions | Show the total deductions value; **CHANGED**: displayed as positive (no negative sign) | Total Deductions = SC + PWD + NAAC + Solo Parent + Others + Returns + Voids |

### Adjustment on VAT (CHANGED - all values displayed as positive; no negative signs)

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Discount (Adjustment On VAT) | Show discount adjustment on VAT; **CHANGED**: displayed as positive | - |
| SC (Adjustment On VAT) | Show total of waived Tax1 for SC discount; **CHANGED**: displayed as positive | - |
| PWD (Adjustment On VAT) | Show total of waived Tax1 for PWD discount; **CHANGED**: displayed as positive | - |
| Others (Adjustment On VAT) | Show total of waived Tax1 for non-SC/non-PWD discounts; **CHANGED**: displayed as positive | - |
| VAT On Returns | Show blank; **CHANGED**: displayed as positive | - |
| Others (No Discount) | Show total of waived Tax1 with no discount; **CHANGED**: displayed as positive | - |
| Total VAT Adjustment | Show the total VAT adjustment value; **CHANGED**: displayed as positive | Total VAT Adjustment = SC + PWD + Others + VAT On Returns + Others (No Discount) |

### Tax and Total (CHANGED - Service Charge renamed and reordered; Z-Counter logic updated)

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Net Amount | Show check total | - |
| Service Charge | **CHANGED**: Renamed from "Service Charge (Without VAT)" to "Service Charge". Show service charge total (SC1 - SC5). **CHANGED**: moved to appear immediately after Net Amount. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | - |
| Local Tax | Show Tax index 2-10 total (add to Tax). **CHANGED**: moved to appear immediately after Service Charge. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | - |
| VAT Payable | Show total Tax 1 amount (include add from function key) | - |
| Sales Overrun/Overflow | Show default "0" | - |
| Total Income | Show total income | Total Income = Total Net Amount - Sales Overrun/Overflow |
| Reset Counter | Payment running number reset flag: Reset = 1, Not Reset = 0 | - |
| Z-Counter | **CHANGED**: Now the number of daily close count in End Of Day (Business Day) for the shop. For a date range, count of daily closes within the selected date range. Increments only after a fully successful EOD; resets to zero at the start of each business day. | Count of successful EOD daily closes in selected date range |
| Remarks | Show payment reference info | - |
| Grand Total | Show grand total for corresponding fields; for "Beginning"/"Grand Accum. Beg. Balance" show first value in date range; for "Ending"/"Grand Accum. Ending Balance" show last value in date range | - |

### Report Footer

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Printed At Time | Show print date and time of the local server. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD; Time Format, Default Time Format: 24-hour clock | - |
| Printed By User | Show operator last name and first name | - |
| Report Code | Show report code POS305 (controlled by Show Footer Report Code: Yes) | - |
| P. | Show page number | - |
| Remark | Run the shell "BirPreprocessShell" before opening this report; if never run, prompts "Please run the BirPreprocessShell first" | - |

## Report Data Accuracy Verification

### 1. All Default Parameters

**Prerequisite(s):**
  1. Run the BirPreprocessShell for the current business day before opening the report.
  2. The Philippines (PH) version of POS305 is configured for the test shop/outlet.
  3. Prepare a single business day with at least one closed check that has discounts (SC/PWD/NAAC/Solo Parent/Others) and a successful EOD completed for the shop.
**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select "default" from all parameters
    2.1. Select "Current Business Day" from Date Type
    2.2. Select "All" from Shops
    2.3. Select "All" from Outlets
    2.4. Select "All" from Stations
    2.5. Select "All" from Payment Classes
  3. Click "Run" button
  4. Verify the report displays data for all default parameter combinations
**Test Result(s):**
  1. Report loads successfully
  2. Data displayed is correct for expected parameter combinations
  3. No errors displayed

### 2. [A Single Outlet, Last Business Day, Single Station, All Payment Classes] - Layout and Labeling Verification

**Prerequisite(s):**
  1. Run the BirPreprocessShell for the last business day before opening the report.
  2. The Philippines (PH) version of POS305 is configured for the test shop/outlet.
  3. The selected outlet has discounts (SC, PWD, NAAC, Solo Parent, Others) and a successful EOD completed for the last business day.
**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters
    2.1. Select "Last Business Day" from Date Type
    2.2. Select "Single" from Shops and pick the test shop
    2.3. Select "Single" from Outlets and pick the test outlet
    2.4. Select "Single" from Stations and pick the test station
    2.5. Select "All" from Payment Classes
  3. Click "Run" button
  4. Verify the column header for Service Charge reads "Service Charge" with no "Without VAT" wording
  5. Verify the column order is Net Amount -> Service Charge -> Local Tax with no columns inserted between them
  6. Verify all Deduction and Discount columns (Discount, SC, PWD, NAAC, Solo Parent, Others, Returns, Voids, Total Deductions) display values without any negative sign and as positive amounts
  7. Verify all Adjustment on VAT columns display values without any negative sign and as positive amounts
  8. Verify the Grand Accum. Ending Balance column displays the same value as the Gross Amount column for that business day
  9. Verify the Z-Counter value equals 1 (one successful EOD for the single business day)
**Test Result(s):**
  1. Service Charge column header reads "Service Charge" with no "Without VAT" wording
  2. Service Charge and Local Tax columns appear immediately after Net Amount with no columns inserted between
  3. All Deduction/Discount and Adjustment on VAT columns display as positive amounts with no negative sign; zero values display as 0 with no sign
  4. Grand Accum. Ending Balance value matches the Gross Amount value for the business day
  5. Z-Counter equals 1 for the single business day with one successful EOD
  6. No errors displayed

### 3. [Multiple Outlets, Specified As Below Date Range, All Stations, All Payment Classes] - Z-Counter Multi-Day Verification

**Prerequisite(s):**
  1. Run the BirPreprocessShell for the specified date range before opening the report.
  2. The Philippines (PH) version of POS305 is configured for the test shop/outlets.
  3. The selected outlets have successful EOD completed for each business day in the specified date range (e.g., 3 consecutive business days).
**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters
    2.1. Select "Specified As Below" from Date Type
    2.2. Set Begin Date to the first business day of the range
    2.3. Set End Date to the last business day of the range (covers 3 business days)
    2.4. Select "Single" from Shops and pick the test shop
    2.5. Select "Multiple" from Outlets and pick multiple test outlets
    2.6. Select "All" from Stations
    2.7. Select "All" from Payment Classes
  3. Click "Run" button
  4. Verify each business day row shows a Z-Counter value reflecting the count of successful daily closes (1 per day per shop)
  5. Verify the Grand Total row's Z-Counter reflects the total count of daily closes in the selected date range across all outlets
  6. Verify the Grand Accum. Ending Balance per day matches Gross Amount per day; the Grand Total row shows the last Gross Amount value in the range
**Test Result(s):**
  1. Z-Counter per business day = number of successful EOD closes for that day (1 for a single shop/day)
  2. Grand Total Z-Counter = total count of daily closes within the selected date range
  3. Grand Accum. Ending Balance per row matches Gross Amount of that day; Grand Total row shows the last value in the range
  4. No errors displayed

### 4. [All Outlets, Last Month, All Stations, All Payment Classes] - Negative Sign Removal Across a Range

**Prerequisite(s):**
  1. Run the BirPreprocessShell for the last month before opening the report.
  2. The Philippines (PH) version of POS305 is configured for the test shop/outlets.
  3. The selected outlets have discounts (including zero-value scenarios) and successful EODs across the last month.
**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters
    2.1. Select "Last Month" from Date Type
    2.2. Select "All" from Shops
    2.3. Select "All" from Outlets
    2.4. Select "All" from Stations
    2.5. Select "All" from Payment Classes
  3. Click "Run" button
  4. Verify that for every business day row, all Deduction/Discount columns and Adjustment on VAT columns display as positive amounts (no negative signs)
  5. Verify that any column value that is zero displays as 0 with no sign
  6. Verify the Service Charge column header is "Service Charge" across all rows
  7. Verify the column order Net Amount -> Service Charge -> Local Tax is preserved across the entire report
**Test Result(s):**
  1. No negative signs appear in any Deduction/Discount or Adjustment on VAT column across all rows in the last month range
  2. Zero values display as 0 with no sign
  3. Service Charge column header reads "Service Charge" with no "Without VAT" wording
  4. Column order Net Amount -> Service Charge -> Local Tax is consistent across the entire report
  5. No errors displayed

### 5. Region Scope Verification - Non-Philippines Region Keeps Current Layout

**Prerequisite(s):**
  1. A non-Philippines region shop/outlet is configured for POS305 (or the equivalent BIR Sales Summary Report layout used by that region).
  2. Run the BirPreprocessShell for the selected business day if applicable for the region.
**Step(s):**
  1. Open POS305 (or the regional equivalent BIR Sales Summary Report) for a non-Philippines region
  2. Select "Current Business Day" from Date Type
  3. Select the non-PH shop from Shops
  4. Select the non-PH outlet from Outlets
  5. Click "Run" button
  6. Verify the report keeps the current layout and labeling (Service Charge (Without VAT) header, original column order, original negative-sign behavior for deductions, original Grand Accum. Ending Balance calculation, original Z-Counter logic)
**Test Result(s):**
  1. The non-PH region report keeps the original "Service Charge (Without VAT)" column header
  2. The non-PH region report keeps the original column order (Service Charge/Local Tax NOT moved after Net Amount)
  3. The non-PH region report keeps the original negative-sign behavior for Deductions/Discounts
  4. The non-PH region report keeps the original Grand Accum. Ending Balance calculation
  5. The non-PH region report keeps the original Z-Counter logic
  6. No errors displayed

## Report Functional Verification

### 1. Export Files to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select "Last Business Day" from Date Type and a single PH outlet
  3. Click "Run" button to render the report
  4. Select "Export" from the menu
  5. Select the desired format from the dropdown menu
  6. Click "OK" button
  7. Verify the exported file is saved to the specified location
**Test Result(s):**
  1. Exported file is successful
  2. Exported file is saved to the specified location
  3. Exported file format matches the selected format
  4. Exported file contains the expected data with the new layout (renamed Service Charge, reordered columns, positive deductions, new Z-Counter, Grand Accum. Ending Balance = Gross Amount)

### 2. Print Report to HTML, PDF

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select "Last Business Day" from Date Type and a single PH outlet
  3. Click "Run" button to render the report
  4. Select "Print" from the menu
  5. Verify the printed report is saved to the specified location
**Test Result(s):**
  1. Printed report is successful
  2. Printed report is saved to the specified location
  3. Printed report format matches the selected format
  4. Printed report contains the expected data with the new layout

### 3. Edit Original Preset Parameters

**Prerequisite(s):**
  1. An existing preset parameter exists for POS305 PH BIR Sales Summary Report V2 (created before the enhancement).
**Step(s):**
  1. Go to Preset Parameters
  2. Select the existing preset parameters to edit
  3. Reselect the parameters (Date Type, Shops, Outlets, Stations, Payment Classes)
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

### 4. Auto Run Preset Parameters to Email, FTP, or SFTP with Specified Auto Report Configuration

**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Auto Report Configuration
  2. Set the desired auto report configuration for the report (Auto Report Employee, Restrict access to preset parameter of auto reports outside the current hierarchy, Attachment Expiration Days)
**Step(s):**
  1. Add new or edit auto report schedule
  2. Select the POS305 PH preset parameters to run
  3. Select the desired destination for the report (Email, FTP, or SFTP)
  4. Verify the auto report schedule is set successfully
**Test Result(s):**
  1. Auto report schedule is set successfully
  2. Report is sent successfully to the specified destination, time, format and other settings
  3. Data is correct for the preset parameters
  4. No errors displayed

### 5. Multi-language Support for Report Parameter and Data

**Step(s):**
  1. Select "Language"
  2. Open POS305 PH BIR Sales Summary Report V2
  3. Verify the language is set successfully
  4. Verify the report displays data in the selected language
**Test Result(s):**
  1. Language is set successfully
  2. Report displays data in the selected language
  3. No errors displayed

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select "default" from all parameters
    2.1. Select "Current Business Day" from Date Type
    2.2. Select "All" from Shops
    2.3. Select "All" from Outlets
    2.4. Select "All" from Stations
    2.5. Select "All" from Payment Classes
  3. Verify the report loads successfully on each browser
**Test Result(s):**
  1. Report loads successfully
  2. No errors displayed

### 2. Support Config Zone Compatibility

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters from Report Data Accuracy Verification (single outlet, multiple outlets, all outlets)
  3. Verify the report loads successfully at the current level, lower level, upper level, and all levels including lower level
**Test Result(s):**
  1. Report loads successfully
  2. No errors displayed

### 3. Support Data Service Compatibility - MySQL

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters from Report Data Accuracy Verification, and set date range to less than 7 days
  3. Verify the report loads successfully
**Test Result(s):**
  1. Report loads successfully
  2. No errors displayed
  3. Data displayed is correct for expected parameter combinations

### 4. Support Data Service Compatibility - TiDB

**Step(s):**
  1. Open POS305 PH BIR Sales Summary Report V2
  2. Select parameters from Report Data Accuracy Verification, and set date range to more than 7 days (e.g., Last Month or a Specified As Below range longer than 7 days)
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

- **Tester**: Mike Lin (mike.lin@shijigroup.com)
- **Reviewer**: Eric Fang (eric.fang@shijigroup.com)
