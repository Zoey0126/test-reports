# POS050 Cashier Report (By Outlet) - Tips Display Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Cashier Report (By Outlet) |
| Code | POS050 |
| Issue Key | HERO-68372 |
| Issue Type | Bug |
| Status | READY FOR QA |
| Summary | [Report] POS050 - Cashier Report (By Outlet) "Tips" fails to display in the report when "Residue" is null or empty |
| Components | REPORTS |
| Support Ticket | #1938720 - Total tip amount not displayed in POS050 after the last upgrade |
| Department | Infrasys Support |

## Bug Summary

When running POS050 - Cashier Report (By Outlet) for a business day / outlet / meal period where:
- Tips > 0 exist on one or more payments (paym_tips not Residue), AND
- No Residue payments exist (paym_tips = 'r'), so the Residue total is null/empty (not 0.00)

The Total tip amount fails to display in the Total row for that outlet / meal period. The Tips column should display the sum of all non-Residue tips even when the Residue total is null/empty (treated as 0).

### Conditions to Reproduce

- Run POS050 - Cashier Report (By Outlet).
- Select a business day / outlet / meal period that has Tips > 0 on one or more payments (paym_tips not Residue) AND no Residue payments (paym_tips = 'r'), so the Residue total is null/empty (not 0.00).
- Check the Total row for that outlet / meal period.

### Root Cause Direction (Implemented)

The Tips aggregation in the Total row treated null/empty Residue total as a missing row rather than 0, which caused the Tips column to be suppressed entirely when no Residue payments existed for the selected scope. The fix ensures null/empty Residue is treated as 0 so that non-Residue tips are still summed and displayed in the Total row.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | Select the business day scope of the report (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Current Business Day, Last Business Day, Specified As Below | Current Business Day |
| Begin Date | Specify the begin business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| End Date | Specify the end business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| Shops | Filter data by shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Outlets | Filter data by outlet(s) under the selected shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Cashiers | Select cashier(s) by Last Name + First Name (multi-select); displays all cashiers with paid records; if the current login user has no paid record at the current level, that user is also shown | All, Single, Multiple | All |
| Meal Period | Filter data by meal period(s) (multi-select) | All, Single, Multiple | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Report Title | Show corresponding report title by selection | - |
| Shop Name | Show shop name; shown only when a single shop is selected. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | - |
| Outlet Name | Show outlet name; shown only when a single outlet is selected | - |
| Date Range | Show specified business date / range. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD | - |
| Cashier | Show cashier name (Last Name + First Name) | - |
| Meal Period | Show meal period name; if undefined period it will show "Undefined" | - |
| Charge Receipts | Show payment total (payment amount + tips, payment type != cash). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | SUM(paym_pay_total + paym_tips WHERE payment_type != 'cash' AND paym_status != 'd') |
| Other Receipts | Show payment total (payment amount + tips, payment type = cash). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | SUM(paym_pay_total + paym_tips WHERE payment_type = 'cash' AND paym_status != 'd') |
| Total Receipts | Show Charged Receipts + Other Receipts. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Charged Receipts + Other Receipts |
| Charged Tips | Show tips amount for payment type != cash. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | SUM(paym_tips WHERE payment_type != 'cash' AND paym_status != 'd') |
| Cash Tips | Show tips amount for payment type = cash. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | SUM(paym_tips WHERE payment_type = 'cash' AND paym_status != 'd') |
| Tips (Total) | **CHANGED**: Show total tips for the cashier / outlet / meal period. Must display correctly even when Residue total is null/empty (treated as 0). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Charged Tips + Cash Tips (null/empty Residue treated as 0) |
| Residue | Show residue tips total (paym_tips = 'r'); can be null/empty when no Residue payments exist. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | SUM(paym_tips WHERE paym_tips = 'r' AND paym_status != 'd') |
| Total (Per Cashier) | Show per-cashier totals for the meal period | Sum of per-row values per cashier |
| Outlet Total | Show sub total for one outlet for all rows for all periods each business date. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Sub total per outlet per business date |
| Grand Total | Show grand total for all outlet totals. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Sum of all Outlet Totals |

## Test Verification

### 1. Tips Display Verification

#### 1.1. All Default Parameters - Non-Residue Tips With No Residue Payments

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. For one outlet and one business day, at least one closed check has Tips > 0 on a non-Residue payment (paym_tips != 'r').
  3. For the same outlet and business day, there are NO Residue payments (paym_tips = 'r') so the Residue total is null/empty (not 0.00).
  4. Capture the expected total tips amount from source payment data (sum of non-Residue tips).
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Current Business Day" from Date Type.
  3. Select "All" from Shops.
  4. Select "All" from Outlets.
  5. Select "All" from Cashiers.
  6. Select "All" from Meal Period.
  7. Click "Run" button.
  8. Verify the Total row for the outlet / meal period displays the Tips column.
  9. Compare the displayed Tips value against the expected sum of non-Residue tips.
**Reproduction Result(s):**
  1. The Tips column in the Total row for the outlet / meal period is blank or missing when Residue total is null/empty.
  2. The non-Residue tips are not displayed in the Total row even though Tips > 0 exists on payments.
  3. The Tips column may show 0.00 or be entirely suppressed depending on the rendering path.
**Fix Result(s):**
  1. The Tips column in the Total row for the outlet / meal period displays the sum of non-Residue tips.
  2. Null/empty Residue is treated as 0, so non-Residue tips are still summed and displayed.
  3. The displayed Tips value matches the expected sum of non-Residue tips.
  4. No errors displayed.

#### 1.2. [A Single Outlet, Last Business Day, All Cashiers, A Single Meal Period] - Tips Display With No Residue

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. One outlet has Tips > 0 on non-Residue payments for the last business day for a specific meal period.
  3. The same outlet / business day / meal period has NO Residue payments (Residue total is null/empty).
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Last Business Day" from Date Type.
  3. Select "Single" from Shops and pick the test shop.
  4. Select "Single" from Outlets and pick the test outlet.
  5. Select "All" from Cashiers.
  6. Select "Single" from Meal Period and pick the test meal period.
  7. Click "Run" button.
  8. Verify the Total row for that outlet / meal period displays the Tips column with the correct sum.
  9. Verify the Residue column shows blank/empty (since no Residue payments exist) but the Tips column is still populated.
**Reproduction Result(s):**
  1. The Tips column is missing or blank in the Total row because Residue total is null/empty.
  2. The Residue column is also blank, but the Tips column should not depend on Residue being populated.
**Fix Result(s):**
  1. The Tips column displays the correct sum of non-Residue tips in the Total row.
  2. The Residue column may be blank/empty but the Tips column is populated correctly.
  3. No errors displayed.

#### 1.3. [Multiple Outlets, Specified As Below Date Range, All Cashiers, All Meal Periods] - Tips Display Across a Range

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. Multiple outlets have Tips > 0 on non-Residue payments across a multi-day range, with no Residue payments for some or all of those outlets/days.
  3. Capture the expected per-outlet tips totals from source payment data.
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Specified As Below" from Date Type.
  3. Set Begin Date and End Date to cover a multi-day range.
  4. Select "Single" from Shops and pick the test shop.
  5. Select "Multiple" from Outlets and pick multiple test outlets.
  6. Select "All" from Cashiers.
  7. Select "All" from Meal Period.
  8. Click "Run" button.
  9. Verify each outlet's Total row displays the Tips column with the correct per-outlet sum.
  10. Verify the Grand Total row displays the Tips column with the correct aggregate sum across all outlets.
**Reproduction Result(s):**
  1. Outlets with null/empty Residue have missing or blank Tips in their Total rows.
  2. The Grand Total Tips may also be missing or undercounted because the per-outlet Tips values were suppressed.
**Fix Result(s):**
  1. Each outlet's Total row displays the correct per-outlet Tips sum even when Residue is null/empty.
  2. The Grand Total Tips equals the aggregate sum of non-Residue tips across all outlets.
  3. No errors displayed.

### 2. Tips Display With Residue Present (Regression - Positive Path)

#### 2.1. Tips and Residue Both Populated

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. One outlet / business day / meal period has both Tips > 0 on non-Residue payments AND at least one Residue payment (paym_tips = 'r') so Residue total is populated.
  3. Capture the expected Tips and Residue values from source payment data.
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Current Business Day" from Date Type.
  3. Select "Single" from Shops and pick the test shop.
  4. Select "Single" from Outlets and pick the test outlet.
  5. Select "All" from Cashiers.
  6. Select "All" from Meal Period.
  7. Click "Run" button.
  8. Verify the Total row for the outlet / meal period displays both Tips and Residue columns with correct values.
**Reproduction Result(s):**
  1. Tips and Residue both display correctly in the Total row when Residue is populated (positive path was working before the fix).
**Fix Result(s):**
  1. Tips and Residue both continue to display correctly in the Total row (no regression).
  2. The displayed Tips value matches the expected sum of non-Residue tips.
  3. The displayed Residue value matches the expected sum of Residue tips.
  4. No errors displayed.

#### 2.2. Zero Tips and Zero Residue (Negative Path)

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. One outlet / business day / meal period has no tips at all (Tips = 0, Residue = 0/null).
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Current Business Day" from Date Type.
  3. Select the outlet with no tips.
  4. Click "Run" button.
  5. Verify the Total row Tips column displays 0.00 or blank consistently with the previous behavior.
**Reproduction Result(s):**
  1. Tips column shows 0.00 or blank in the Total row when no tips exist (consistent across both paths).
**Fix Result(s):**
  1. Tips column shows 0.00 or blank in the Total row when no tips exist (no regression).
  2. No errors displayed.

### 3. Data Accuracy Verification

#### 3.1. Tips Match Source Payment Data

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. Capture the expected tips values from source payment data (pos_payments / payment tables) for the selected outlet / business day / meal period.
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select "Current Business Day" from Date Type.
  3. Select the test outlet / cashier / meal period.
  4. Click "Run" button.
  5. Compare the displayed Tips value against the captured expected tips from source payment data.
  6. Verify no payments are missing or duplicated in the Tips aggregation.
**Reproduction Result(s):**
  1. Tips value was missing or undercounted in the Total row when Residue was null/empty.
**Fix Result(s):**
  1. Tips value matches the sum of non-Residue tips from source payment data exactly.
  2. No payments are missing or duplicated in the Tips aggregation.
  3. No errors displayed.

#### 3.2. Charged Tips vs Cash Tips Breakdown Correctness

**Prerequisite(s):**
  1. A build with the HERO-68372 fix is deployed.
  2. The selected outlet / business day / meal period has both Charged Tips (payment_type != cash) and Cash Tips (payment_type = cash) on non-Residue payments, with no Residue payments.
**Step(s):**
  1. Open POS050 Cashier Report (By Outlet).
  2. Select the test outlet / cashier / meal period.
  3. Click "Run" button.
  4. Verify Charged Tips and Cash Tips breakdown values are correct in the Total row.
  5. Verify the Total Tips value equals Charged Tips + Cash Tips.
**Reproduction Result(s):**
  1. Total Tips value was missing/blank when Residue was null/empty, even though Charged Tips and Cash Tips individually may have been populated.
**Fix Result(s):**
  1. Charged Tips and Cash Tips breakdown values are correct in the Total row.
  2. Total Tips equals Charged Tips + Cash Tips.
  3. No errors displayed.

### 4. Regression Scope

- POS050 Cashier Report (By Outlet) Tips display with All Outlets, a single outlet and multiple outlets selections.
- POS050 Tips display across all meal period selections (All, Single, Multiple) when Residue is null/empty.
- POS050 Tips display across Date Type selections (Current Business Day, Last Business Day, Specified As Below) when Residue is null/empty.
- POS050 Tips display with Residue present (positive path) to confirm no regression.
- POS050 Tips display with zero tips (negative path) to confirm no regression.
- Source payment data accuracy for Tips, Charged Tips, Cash Tips and Residue columns.
- Export and print of POS050 to confirm the Tips fix applies to all output formats (CSV, Excel, PDF, etc.).
- Related reports with the same Tips aggregation pattern (e.g. CUS0327 / CUS0328 Employee Tips By Meal Period Report) to confirm whether the same root cause affects them.

## Test Environment

- **Version**: Sprint build containing the HERO-68372 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## QC Information

- **Tester**: Yumia Tang (yumia.tang@shijigroup.com)
- **Reviewer**: Kathy Kuang (kathy.kuang@shijigroup.com)
