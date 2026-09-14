# POS017 Detail Check Listing Report | Disct Amt column in Grand Total is incorrect Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Detail Check Listing Report |
| Report Code | POS017 |
| JIRA Issue | HERO-67801 |
| Component | REPORTS |
| Issue Type | Bug |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Shop or outlet whose closed checks are listed (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business date range of the checks to be listed (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Today, Yesterday, Specified as Below | Yesterday |
| Begin Date / End Date | Business date range boundaries, enabled when Business Dates = 'Specified as Below' | User Defined | - |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Disct Amt | Discount amount shown on each detail row of the check listing; detail rows already show the correct discount amount before this fix | Discount amount of the check on the detail row |
| Disct Amt (Grand Total) | Discount amount shown on the Grand Total row; before the fix it is about twice the correct total after the previous Grand Total correction | After the fix: sum of the Disct Amt values on the detail rows |

## Test Verification

### 1.Grand Total Accuracy Verification

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. A build with the HERO-67801 fix is deployed.
  2. Closed checks with discount amounts exist for the selected outlets and business dates.
**Step(s):**
  1. Go to Reports and open POS017 Detail Check Listing Report.
  2. Select "default" from all parameters.
    2.1. Select All Outlets from Shops/Outlets.
    2.2. Select Yesterday from Business Dates.
  3. Click "Run" button.
  4. Compare the Disct Amt value on the Grand Total row with the sum of the Disct Amt values on the detail rows.
**Reproduction Result(s):**
  1. The Grand Total Disct Amt is about twice the correct total after the previous Grand Total correction.
  2. Detail rows still show the correct discount amount, so the Grand Total does not match the detail sum.
**Fix Result(s):**
  1. The Grand Total Disct Amt equals the sum of the Disct Amt values on the detail rows.
  2. Detail rows keep showing the correct discount amounts.
  3. No errors displayed.

#### 1.2.[A Single Outlet, Specified Business Date Range With Discounted Checks]

**Prerequisite(s):**
  1. A build with the HERO-67801 fix is deployed.
  2. One outlet has closed checks with known discount amounts within a specific business date range.
**Step(s):**
  1. Open POS017 Detail Check Listing Report.
  2. Select only that single outlet.
  3. Set Business Dates to Specified as Below and enter the prepared date range.
  4. Run the report.
  5. Recalculate the expected Grand Total Disct Amt from the detail rows and compare.
**Reproduction Result(s):**
  1. The Grand Total row shows a doubled Disct Amt that does not match the detail rows.
**Fix Result(s):**
  1. Grand Total Disct Amt matches the sum of the detail row Disct Amt values exactly.
  2. Other Grand Total columns remain correct.
  3. No errors displayed.

#### 1.3.[Multiple Outlets] Grand Total Across Outlets

**Prerequisite(s):**
  1. A build with the HERO-67801 fix is deployed.
  2. Two or more outlets have closed checks with discount amounts in the same business date range.
**Step(s):**
  1. Open POS017 Detail Check Listing Report.
  2. Select multiple outlets in Shops/Outlets.
  3. Select the business date range of the prepared checks.
  4. Run the report.
  5. Verify the Grand Total Disct Amt across all selected outlets.
**Reproduction Result(s):**
  1. The cross-outlet Grand Total Disct Amt is inflated (doubled) and does not match the detail rows.
**Fix Result(s):**
  1. The Grand Total Disct Amt equals the sum of all detail row Disct Amt values across the selected outlets.
  2. Discount amounts are not counted twice for any outlet.
  3. No errors displayed.

#### 1.4.Detail Rows Keep Correct Disct Amt After Fix

**Prerequisite(s):**
  1. A build with the HERO-67801 fix is deployed.
  2. Closed checks with known discount amounts exist for the selected parameters.
**Step(s):**
  1. Open POS017 Detail Check Listing Report.
  2. Select the outlet and business date range of the prepared checks.
  3. Run the report.
  4. Compare each detail row Disct Amt with the discount amount recorded on the POS check.
**Reproduction Result(s):**
  1. Detail rows already show the correct discount amount before the fix; only the Grand Total is wrong.
**Fix Result(s):**
  1. Every detail row Disct Amt still matches the discount amount recorded on the POS check.
  2. The fix does not change any detail row value.
  3. No errors displayed.

#### 1.5.Checks Without Discount in the Same Range

**Prerequisite(s):**
  1. A build with the HERO-67801 fix is deployed.
  2. The selected business date range contains both checks with discount amounts and checks without any discount.
**Step(s):**
  1. Open POS017 Detail Check Listing Report.
  2. Select the outlet and business date range containing both kinds of checks.
  3. Run the report.
  4. Verify the Disct Amt of the zero-discount detail rows and the Grand Total.
**Reproduction Result(s):**
  1. The Grand Total Disct Amt is doubled even when part of the checks has no discount.
**Fix Result(s):**
  1. Zero-discount detail rows show 0 in Disct Amt and add nothing to the total.
  2. Grand Total Disct Amt equals the sum of the discounted detail rows only.
  3. No errors displayed.

### 2.Regression Scope

- POS017 Grand Total row after the previous Grand Total correction: confirm the Disct Amt fix does not change other Grand Total columns (for example amount and net amount columns).
- POS017 detail rows and any subtotal grouping for single outlet, multiple outlets and All Outlets selections.
- Export and print of POS017 (for example Excel and PDF) to confirm the Grand Total Disct Amt is correct in all output formats.
- Business date range selections around day boundaries to confirm totals still match the detail rows.

## Test Environment

- **Version**: Sprint build containing the HERO-67801 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
