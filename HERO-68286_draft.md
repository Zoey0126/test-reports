# POS060 BIR Report (Philippines) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | BIR Report (Philippines) |
| Code | POS060 |
| JIRA Ticket | HERO-68286 |
| Issue Type | Improvement |
| Status | READY FOR QA |
| QC Tester | Mike Lin |
| QC Reviewer | Eric Fang |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | (single-select) Selects the business date range for the report. Begin Date and End Date are enabled when 'Specified As Below' is selected. | Current Business Day, Last Business Day, Specified As Below | Current Business Day |
| Begin Date | Specify the begin business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below'. | - | - |
| End Date | Specify the end business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below'. | - | - |
| Shops | (multi-select) Filter data by shop(s). | All, Single, Multiple | All |
| Outlets | (multi-select) Filter data by outlet(s) under the selected shop(s). | All, Single, Multiple | All |
| Stations | (multi-select) Filter data by station(s). | All, Single, Multiple | All |
| Payment Classes | (multi-select) Filter data by payment class(es). | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Title | Show corresponding report title by selection [Shop Name / Shop Address / Tax ID in Shop / Report Name]. | - |
| Date Range | Show specified date range. | - |
| Software Version | Show software name and version. | - |
| Date And Time | Show print date and time. | - |
| User ID | Show print user first name and last name. | - |
| Date | Show business day. | - |
| Beginning | Show the start payment running number. | - |
| Ending | Show the end payment running number. | - |
| Grand Accum. Beg. Balance | Accumulated grand total from previous business day. | Grand Accum. Beg. Balance = Ending Balance - Net Sales |
| Grand Accum. Ending Balance | Accumulated ending balance. | Grand Accum. Ending Balance = Beginning Balance + Net Sales |
| Gross Amount | Show item sales price. | - |
| VATable | Show check item total add all discount. | - |
| VAT | Show tax less any discount excluding SC & PWD discount (only Tax Index 1). | - |
| VAT-Exempt | Show check item total of whole day for waived tax1 items. | - |
| Zero-Rated | Show check item total for waived tax1 items with no VAT-Exempt. | - |
| Discount | Show discount amount. | - |
| SC | Show SC discount (negative number). | - |
| PWD | Show PWD discount (negative number). | - |
| NAAC | Show NAAC discount (negative number). | - |
| Solo Parent | Show SP discount (negative number). | - |
| Others | Show all discounts excluding SC, PWD, NAAC & SP (negative number). | - |
| Voids | Show deleted item in check. | - |
| Total Deductions | Show the total deductions value. | Total Deductions = SC + PWD + NAAC + Solo Parent + Others + Returns + Voids |
| Total VAT Adjustment | Show the total VAT adjustment value. | Total VAT Adjustment = SC + PWD + Others + VAT On Returns + Others (No Discount) |
| Service Charge (Without VAT) | Show service charge total (SC1 - SC5). | - |
| Local Tax | Show Tax index 2-10 total. | - |
| VAT Payable | Show total Tax 1 amount. | - |
| Net Amount | Show check total. | - |
| Total Income | Show total income. | Total Income = Total Net Amount - Sales Overrun/Overflow |
| Grand Total | Show grand total for corresponding fields. | - |
| Printed At Time | Show print date and time of the local server. | - |
| Printed By User | Show operator last name and first name. | - |
| Report Code | Show report code POS060. | - |
| P. | Show page number. | - |

## Report Data Accuracy Verification

### 1. All Default Parameters
**Prerequisite(s):**
  1. Run BirPreprocessShell before opening this report.
  2. Ensure migration status is completed so the formal rptdesign reads pos_bir_* tables.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select "default" from all parameters:
    2.1. Select "Current Business Day" from Date Type.
    2.2. Select "All" from Shops.
    2.3. Select "All" from Outlets.
    2.4. Select "All" from Stations.
    2.5. Select "All" from Payment Classes.
  3. Click "Run" button.
  4. Verify report displays data for all default parameter combinations.
  5. Verify report reads from the new pos_bir_station_summaries (3-table UNION) + pos_bir_daily_infos tables.
**Test Result(s):**
  1. Report loads successfully.
  2. Data displayed is correct for expected parameter combinations.
  3. Station and payment display names resolve from POS master data at runtime.
  4. No errors displayed.

### 2. [A Single Outlet, Last Business Day, A Single Station, A Single Payment Class]
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Last Business Day" from Date Type.
    2.2. Select "Single" from Shops.
    2.3. Select "Single" from Outlets.
    2.4. Select "Single" from Stations.
    2.5. Select "Single" from Payment Classes.
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify payment type is resolved from POS payment/check data by payment class (not stored on summary table).
**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Payment type resolved correctly from POS payment/check data by payment class.
  4. No errors displayed.

### 3. [Multiple Outlets, Specified As Below Business Day, Multiple Stations, Multiple Payment Classes]
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Specified As Below" from Date Type.
    2.2. Set Begin Date and End Date to a range within the last 3 months (retention period).
    2.3. Select "Multiple" from Shops.
    2.4. Select "Multiple" from Outlets.
    2.5. Select "Multiple" from Stations.
    2.6. Select "Multiple" from Payment Classes.
  3. Click "Run" button.
  4. Verify report displays data for selected parameter combinations.
  5. Verify the viewer renders directly (within retention, no on-demand routing).
**Test Result(s):**
  1. Report filters by selected parameters.
  2. Data displayed is correct for expected parameter combinations.
  3. Report renders directly without on-demand flow for date ranges within retention.
  4. No errors displayed.

### 4. [All Outlets, Specified As Below Business Day (Beyond Retention), All Stations, All Payment Classes] - On-Demand Flow
**Prerequisite(s):**
  1. Ensure the transitional _legacy.rptdesign is present and the report is listed for on-demand.
  2. Run BirPreprocessShell for the target date range.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select parameters:
    2.1. Select "Specified As Below" from Date Type.
    2.2. Set Begin Date to 2026-01-01 and End Date to 2026-03-31 (older than last 3 months).
    2.3. Select "All" from Shops.
    2.4. Select "All" from Outlets.
    2.5. Select "All" from Stations.
    2.6. Select "All" from Payment Classes.
  3. Verify the report engine shows "Background generation required" and lists monthly segments for on-demand generation.
  4. Click "Add to generation queue" to open the On-Demand Job form.
  5. Fill in email interface, recipients, and format.
  6. Submit the on-demand job.
  7. Verify the result page shows the created Preset Parameters id and Scheduled Task ids.
  8. Verify Preset Parameters listing shows the on-demand preset (e.g. "On-demand POS060 2026-01-01 2026-03-31").
  9. Verify Report Management > Scheduled Tasks lists the one-time BirPreprocessShell tasks for each month segment.
  10. After preprocess and AutoReport complete, verify the emailed report is produced from the formal design reading pos_bir_* union temporary tables.
**Test Result(s):**
  1. Report engine correctly routes to on-demand flow for dates older than retention.
  2. Monthly segments are listed for on-demand generation.
  3. On-Demand Job form opens with email interface, recipients, and format fields.
  4. Preset Parameters id and Scheduled Task ids are created successfully.
  5. On-demand preset appears in Preset Parameters listing.
  6. BirPreprocessShell tasks are listed in Scheduled Tasks for each month segment.
  7. Emailed report is produced from the formal design reading pos_bir_* union temporary tables.
  8. No errors displayed.

### 5. Verify 3-Table UNION Data Source
**Prerequisite(s):**
  1. Ensure pos_bir_station_summaries, pos_bir_station_monthly_summaries, and pos_bir_station_summaries_temporary tables have test data.
  2. Ensure pos_bir_daily_infos table has running number / cumulative field data.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select "Specified As Below" from Date Type with a range that spans both retention and on-demand periods.
  3. Click "Run" button.
  4. Verify the report reads from the 3-table UNION:
    4.1. pos_bir_station_summaries (daily, within retention).
    4.2. pos_bir_station_monthly_summaries (monthly, for complete months in long ranges).
    4.3. pos_bir_station_summaries_temporary (on-demand, for incomplete month segments).
  5. Verify pos_bir_daily_infos is joined for running numbers / cumulative fields.
  6. Verify station display names resolve from POS master data at runtime.
  7. Verify payment display names resolve from POS master data at runtime.
**Test Result(s):**
  1. Report correctly reads from the 3-table UNION data source.
  2. Running numbers and cumulative fields are correctly joined from pos_bir_daily_infos.
  3. Station and payment display names are resolved from POS master data at runtime.
  4. No errors displayed.

### 6. Verify Transitional _legacy.rptdesign Routing (Before Migration Completes)
**Prerequisite(s):**
  1. Ensure migration status is NOT completed.
  2. Ensure transitional _legacy.rptdesign file is present.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select "Current Business Day" from Date Type.
  3. Click "Run" button.
  4. Verify the Report Module routes to the _legacy.rptdesign file (reads rpt_dyn_pos_bir_* dynamic tables).
  5. Verify on-demand UI is enabled only when the transitional legacy design is present and the report is listed for on-demand.
**Test Result(s):**
  1. Report Module routes to the _legacy.rptdesign file before migration completes.
  2. Report reads from rpt_dyn_pos_bir_* dynamic tables.
  3. On-demand UI is enabled only when transitional legacy design is present.
  4. No errors displayed.

### 7. Verify Formal rptdesign Routing (After Migration Completes)
**Prerequisite(s):**
  1. Ensure migration status is completed.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select "Current Business Day" from Date Type.
  3. Click "Run" button.
  4. Verify the Report Module uses the formal rptdesign file (reads pos_bir_* standard tables).
  5. Verify the report does NOT read from rpt_dyn_pos_bir_* dynamic tables.
**Test Result(s):**
  1. Report Module uses the formal rptdesign file after migration completes.
  2. Report reads from pos_bir_* standard tables.
  3. Report does not read from rpt_dyn_pos_bir_* dynamic tables.
  4. No errors displayed.

### 8. Verify Payment Type Resolution from POS Payment/Check Data
**Prerequisite(s):**
  1. Ensure POS payment/check data has payments mapped to payment classes.
**Step(s):**
  1. Open BIR Report (Philippines) from POS Module > Desktop Format.
  2. Select "Last Business Day" from Date Type.
  3. Select "Single" from Payment Classes.
  4. Click "Run" button.
  5. Verify payment type is not stored on the summary table but resolved from POS payment/check data by payment class.
  6. Verify payment display names are correct.
**Test Result(s):**
  1. Payment type is correctly resolved from POS payment/check data by payment class.
  2. Payment display names are correct.
  3. No errors displayed.

## Report Functional Verification

### 1. Verify Report Information with Format Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Set the desired date format for the report.
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select "Language" from the menu.
  3. Select the desired language from the dropdown menu.
  4. Verify the language is set successfully.
  5. Verify the report displays data in the selected language.
**Test Result(s):**
  1. Language is set successfully.
  2. Report displays data in the selected language.
  3. No errors displayed.

### 2. Export Files to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript, PowerPoint (.pptx)
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select "Export" from the menu.
  3. Select the desired format from the dropdown menu.
  4. Click "OK" button.
  5. Verify the exported file is saved to the specified location.
**Test Result(s):**
  1. Exported file is successful.
  2. Exported file is saved to the specified location.
  3. Exported file format matches the selected format.
  4. Exported file contains the expected data.

### 3. Print Report to HTML, PDF
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select "Print" from the menu.
  3. Verify the printed report is saved to the specified location.
**Test Result(s):**
  1. Printed report is successful.
  2. Printed report is saved to the specified location.
  3. Printed report format matches the selected format.
  4. Printed report contains the expected data.

### 4. Create Preset Parameters from Current Parameters
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select "Preset Parameters" from the menu.
  3. Click "OK" button.
  4. Input the relevant information for the preset parameters.
  5. Click "Save" button.
  6. Verify the preset parameters are set successfully.
  7. Verify the preset parameters are displayed in the report.
  8. Open the preset parameters' Report from the list.
  9. Verify the report displays data for the preset parameters.
**Test Result(s):**
  1. Preset parameters are set successfully.
  2. Report filters by the preset parameters.
  3. Data displayed is correct for the preset parameters.
  4. No errors displayed.

### 5. Edit Original Preset Parameters
**Step(s):**
  1. Go to Preset Parameters.
  2. Select the preset parameters to edit.
  3. Reselect the parameters.
  4. Click "OK" button.
  5. Select "Preset Parameters" from the menu.
  6. Input the relevant information for the preset parameters.
  7. Click "Save" button.
  8. Verify the preset parameters are set successfully.
  9. Verify the preset parameters are displayed in the report.
  10. Open the preset parameters' Report from the list.
  11. Verify the report displays data for the preset parameters.
**Test Result(s):**
  1. Preset parameters are set successfully.
  2. Report filters by the preset parameters.
  3. Data displayed is correct for the preset parameters.
  4. No errors displayed.

### 6. Run Preset Parameters from Original or Current Parameters
**Step(s):**
  1. Open the preset parameters' Report from the list.
  2. Verify the report displays data for the preset parameters.
**Test Result(s):**
  1. Data displayed is correct for the preset parameters.
  2. No errors displayed.

### 7. Auto Run Preset Parameters to Email, FTP, or SFTP with Specified Auto Report Configuration
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Auto Report Configuration.
  2. Set the desired auto report configuration for the report.
**Step(s):**
  1. Add new or edit auto report schedule.
  2. Select the preset parameters to run.
  3. Select the desired destination for the report.
  4. Verify the auto report schedule is set successfully.
**Test Result(s):**
  1. Auto report schedule is set successfully.
  2. Report is sent successfully to the specified destination, time, format and other settings.
  3. Data is correct for the preset parameters.
  4. No errors displayed.

### 8. Multi-Language Support for Report Parameter and Data
**Step(s):**
  1. Select "Language".
  2. Open BIR Report (Philippines).
  3. Verify the language is set successfully.
  4. Verify the report displays data in the selected language.
**Test Result(s):**
  1. Language is set successfully.
  2. Report displays data in the selected language.
  3. No errors displayed.

### 9. Verify Auto-Cleanup On-Demand Jobs Configuration
**Prerequisite(s):**
  1. Go to Admin Console > System Configuration > Report Module.
**Step(s):**
  1. Set "Auto-cleanup On-Demand jobs" to "Yes".
  2. Create and run an on-demand job for BIR Report (Philippines).
  3. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks are soft-deleted.
  4. Set "Auto-cleanup On-Demand jobs" to "No".
  5. Create and run another on-demand job.
  6. After the on-demand report is sent, verify the temporary preset and related one-time scheduled tasks are NOT soft-deleted.
**Test Result(s):**
  1. When "Auto-cleanup On-Demand jobs" = Yes, temporary preset and one-time scheduled tasks are soft-deleted after on-demand report is sent.
  2. When "Auto-cleanup On-Demand jobs" = No, temporary preset and one-time scheduled tasks are NOT soft-deleted.
  3. No errors displayed.

## Report Compatibility Verification

### 1. Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Step(s):**
  1. Open BIR Report (Philippines) in Google Chrome.
  2. Select "default" from all parameters:
    2.1. Select "Current Business Day" from Date Type.
    2.2. Select "All" from Shops.
    2.3. Select "All" from Outlets.
    2.4. Select "All" from Stations.
    2.5. Select "All" from Payment Classes.
  3. Verify the report loads successfully.
  4. Repeat steps 1-3 in Mozilla Firefox.
  5. Repeat steps 1-3 in Microsoft Edge.
**Test Result(s):**
  1. Report loads successfully in Google Chrome.
  2. Report loads successfully in Mozilla Firefox.
  3. Report loads successfully in Microsoft Edge.
  4. No errors displayed.

### 2. Support Config Zone Compatibility
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification at different Config Zone hierarchy levels:
    2.1. Current hierarchy level.
    2.2. Lower hierarchy level.
    2.3. Upper hierarchy level.
    2.4. All hierarchy levels including lower levels.
  3. Verify the report loads successfully at each level.
**Test Result(s):**
  1. Report loads successfully at current hierarchy level.
  2. Report loads successfully at lower hierarchy level.
  3. Report loads successfully at upper hierarchy level.
  4. Report loads successfully at all hierarchy levels including lower levels.
  5. No errors displayed.

### 3. Support Data Service Compatibility - MySQL
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification, and set date range to less than 7 days.
  3. Verify the report loads successfully.
  4. Verify data is retrieved from MySQL.
**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from MySQL.

### 4. Support Data Service Compatibility - TiDB
**Step(s):**
  1. Open BIR Report (Philippines).
  2. Select parameters from Report Data Accuracy Verification, and set date range to more than 7 days.
  3. Verify the report loads successfully.
  4. Verify data is retrieved from TiDB.
**Test Result(s):**
  1. Report loads successfully.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from TiDB.

### 5. Support Data Service Compatibility - SQL Server (MSSQL)
**Step(s):**
  1. Open BIR Report (Philippines) on an MSSQL environment.
  2. Select parameters from Report Data Accuracy Verification.
  3. Verify the report loads successfully.
  4. Verify data is retrieved from MSSQL.
**Test Result(s):**
  1. Report loads successfully on MSSQL environment.
  2. No errors displayed.
  3. Data displayed is correct for expected parameter combinations.
  4. Data is retrieved from MSSQL.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
