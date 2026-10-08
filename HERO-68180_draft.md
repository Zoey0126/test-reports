# POS018 Discount Report - Loading Performance Optimization Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Discount Report |
| Code | POS018 |
| Issue Key | HERO-68180 |
| Issue Type | Bug |
| Status | READY FOR QA |
| Summary | POS018 Discount Report - optimize loading performance |
| Components | REPORTS |
| Customer | ACCOR HOTELS & RESORTS HQ |
| Support Ticket | #1933236 - Report POS018 - not able to generate |
| Design File | Plugin/Pos/webroot/reports/item_discount/item_discount.rptdesign |

## Bug Summary

The POS018 Discount Report dataset `CheckDiscountsItems` is slow or fails to generate under multi-day or last-month runs. The customer (Accor HQ) reported that the report is not able to generate (timeout / long load).

### Hotspots Identified

1. **Correlated scalar subquery `incl_sc`** on `pos_check_tax_sc_refs` per `citm_id` (filter `ctsr_variable LIKE 'incl_sc_ref%'`) - becomes a `DEPENDENT SUBQUERY` once per discount-item row.
2. **LEFT JOIN derived table(s)** on `pos_check_extra_infos` for discount reference (and optionally reason) that re-join `pos_checks` + `pos_business_days` with the same date/outlet filters as the outer query, then `GROUP BY` - materialize + hash join cost grows with range.
3. **Nested `pos_discount_reasons` lookup** (when Show Discount Reason / detail enabled) inside the reason derived table.
4. **`last_month` uses `YEAR(bday_date)` / `MONTH(bday_date)`** (non-SARGable) in `CheckDiscountsItems` / `CheckNumber` / `sBizDays` - bad join order (drives from `pos_discount_types` -> all discounts) vs date-range path.

### Actual Result (Before Fix)

- `EXPLAIN`: `DEPENDENT SUBQUERY` on `pos_check_tax_sc_refs` (index item on `ctsr_citm_id`; filter still includes `LIKE 'incl_sc_ref%'`).
- Reference derived table materializes `pos_check_extra_infos` joined through checks/bdays for the same filter window (duplicate work vs outer join).
- `last_month` `YEAR/MONTH` vs date-range rewrite (same outlet/month, same 26k result rows): `YEAR/MONTH` ~135s vs range ~2.9s (46x slower); `YEAR/MONTH` plan drives from `pos_discount_types` / type index over discounts instead of `outlet_date` range on `pos_business_days`.
- Support ticket: report not able to generate (timeout / long load).

### Fix Direction (Implemented)

- **Primary** - remove correlated `incl_sc`: replace with a `LEFT JOIN` pre-aggregate (or move `incl_sc` into a Script/map dataset keyed by `citm_id`).
- **Discount reference / reason**: prefer direct `LEFT JOIN pos_check_extra_infos ON ckei_cdis_id = cdis_id AND ckei_section/variable ...` (index discount on `ckei_cdis_id`) with `GROUP_CONCAT` in the outer query, or one scoped derived table without re-joining all checks/bdays. Reason id -> name: `LEFT JOIN pos_discount_reasons ON dirs_id = ckei_value` instead of scalar subquery in `CASE`.
- **`last_month`**: replace `YEAR/MONTH` with `bday_date >= '<first_day>' AND bday_date <= '<last_day>'` in `CheckDiscountsItems`, `CheckNumber`, `sBizDays`.
- **Optionally** validate numeric IN-lists before concatenation.

### Expected Result (After Fix)

- Outer path stays date/outlet-driven (`bday` -> `checks` -> `discounts` -> `discount items` -> `items`), without per-row correlated `incl_sc` scans when a join aggregate is possible.
- Discount reference / reason joins `pos_check_extra_infos` once (via `ckei_cdis_id` / scoped map), not re-scan all checks in range inside a derived table.
- `last_month` uses a SARGable range on `bday_date` (same shape as `specified`).

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Date Type | Select the business date scope of the report (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Current Business Day, Last Business Day, Last Month, Specified As Below | Current Business Day |
| Begin Date | Specify the begin business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| End Date | Specify the end business date, format CCYY-MM-DD; enabled when Date Type = 'Specified As Below' | - | - |
| Shops | Filter data by shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Outlets | Filter data by outlet(s) under the selected shop(s) (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All, Single, Multiple | All |
| Discount | Select the discount scope displayed in the report (single-select) | Item Discount + Check Discount, Item Discount, Check Discount | Item Discount + Check Discount |
| Discount Types | Select discount type(s) to display (multi-select) | All, Single, Multiple | All |
| Revenue/Non-Revenue | Filter checks by revenue type (single-select) | All, Revenue, Non-Revenue, Advance Order | All |
| Show Discount Reference | Whether to show discount reference (single-select) | Yes, No | No |
| Show Discount Reason | Whether to show discount reason (single-select) | Yes, No | No |
| Detail/Summary | Display granularity (single-select) | Detail, Summary | Summary |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Shop Name | Show specified shop name in header. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | - |
| Outlet Name | Show specified outlet name in header. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | - |
| Report Name | Show the report title "Discount Report" | - |
| Date Range | Show specified date range in header. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD | - |
| Printed | Show print time and print user. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Date Format, Default Date Format: CCYY-MM-DD; Time Format, Default Time Format: 24-hour clock | - |
| Outlet | Group detail data by outlet and show outlet name | - |
| Check # | Show the check number; checks sorted by check number ascending | - |
| Apply User | Show the user who applied the discount | - |
| Discount Type | Show discount type name (item discount / check discount) | - |
| Discount Reference | Show discount reference from `pos_check_extra_infos` (section='discount', variable='reference'); when multiple references exist for the same discount type on one check, separated by "," | `GROUP_CONCAT` of `ckei_value` joined via `ckei_cdis_id` |
| Discount Reason | Show discount reason name from `pos_discount_reasons` joined via `dirs_id = ckei_value` (section='discount', variable='reason'); if no reason, empty | `LEFT JOIN pos_discount_reasons ON dirs_id = ckei_value` |
| Department | Show department name. Settings: Go to Platform: System Management > System Configuration > Report > Mapping Configuration: Department Mapping, Default: Empty, No Limit | - |
| Item Code | Show item code | - |
| Item Name | Show item name | - |
| Qty | Show item quantity | - |
| Gross Amount | Show item gross sales amount. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | - |
| Discount | Show discount amount. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | - |
| Incl SC | **CHANGED**: Show included service charge reference from `pos_check_tax_sc_refs` (`ctsr_variable LIKE 'incl_sc_ref%'`); now aggregated via `LEFT JOIN` pre-aggregate (no correlated subquery). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | `LEFT JOIN (SELECT ctsr_citm_id, SUM(IFNULL(ctsr_value, 0)) AS incl_sc FROM pos_check_tax_sc_refs WHERE ... GROUP BY ctsr_citm_id) sc ON sc.ctsr_citm_id = citm_id` |
| Net Amount | Show net sales amount (Gross - Discount). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Gross Amount - Discount |
| Total (Per Check) | Show per-check totals | Sum of per-row values per check |
| Outlet Total | Show sub total for one outlet. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Sum of per-check totals within outlet |
| Grand Total | Show grand total. Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Rounding, Default Rounding: Round Off; Decimal Places, Default Decimal Places: 2; Currency Sign, Default Currency Sign: $; Thousand Separator, Default Thousand Separator: Empty; Decimal Separator, Default Decimal Separator: . | Sum of outlet totals |
| Report Code | Show report code POS018 (controlled by Show Footer Report Code: Yes = show, No = hide) | - |

## Test Verification

### 1. Loading Performance Verification

#### 1.1. All Default Parameters - Single Day, Single Outlet, No Reference/Reason

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. A test environment with a representative data volume for one outlet on the current business day.
  3. Note the baseline load time observed before the fix for the same scope.
  4. Capture the EXPLAIN plan of the `CheckDiscountsItems` dataset before the fix (correlated `incl_sc` subquery present).
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select "default" from all parameters
    2.1. Select "Current Business Day" from Date Type
    2.2. Select "All" from Shops
    2.3. Select "All" from Outlets
    2.4. Select "Item Discount + Check Discount" from Discount
    2.5. Select "All" from Discount Types
    2.6. Select "All" from Revenue/Non-Revenue
    2.7. Select "No" from Show Discount Reference
    2.8. Select "No" from Show Discount Reason
    2.9. Select "Summary" from Detail/Summary
  3. Click "Run" button
  4. Measure the time taken from clicking Run until the report fully renders
  5. Capture the EXPLAIN plan of `CheckDiscountsItems` after the fix (no correlated `incl_sc` subquery)
**Reproduction Result(s):**
  1. Even for a single day / single outlet, the correlated `incl_sc` subquery runs once per discount-item row.
  2. EXPLAIN shows `DEPENDENT SUBQUERY` on `pos_check_tax_sc_refs` filtered by `ctsr_variable LIKE 'incl_sc_ref%'`.
**Fix Result(s):**
  1. The report loads within an acceptable timeframe for a single day / single outlet.
  2. EXPLAIN no longer shows `DEPENDENT SUBQUERY` for `incl_sc`; the `LEFT JOIN` pre-aggregate is used instead.
  3. No errors displayed.

#### 1.2. [A Single Outlet, Specified As Below (~7 days), All Discount Types, All Revenue, Show Discount Reference=Yes, Detail] - Multi-Day With Reference

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. A test environment with a representative data volume for one outlet over ~7 days (e.g. Accor / multi-outlet or one busy outlet).
  3. Note the baseline load time observed before the fix for the same scope with Show Discount Reference=Yes and Detail enabled.
  4. Capture the EXPLAIN plan of `CheckDiscountsItems` before the fix (derived table materializes `pos_check_extra_infos` joined through checks/bdays).
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select parameters
    2.1. Select "Specified As Below" from Date Type
    2.2. Set Begin Date and End Date to cover ~7 days
    2.3. Select "Single" from Shops and pick the test shop
    2.4. Select "Single" from Outlets and pick the test outlet
    2.5. Select "Item Discount + Check Discount" from Discount
    2.6. Select "All" from Discount Types
    2.7. Select "All" from Revenue/Non-Revenue
    2.8. Select "Yes" from Show Discount Reference
    2.9. Select "No" from Show Discount Reason
    2.10. Select "Detail" from Detail/Summary
  3. Click "Run" button
  4. Measure the time taken from clicking Run until the report fully renders
  5. Capture the EXPLAIN plan of `CheckDiscountsItems` after the fix (single scoped `LEFT JOIN pos_check_extra_infos` instead of derived table re-joining all checks/bdays)
**Reproduction Result(s):**
  1. Multi-day run with Show Discount Reference=Yes and Detail is very slow or fails to generate.
  2. EXPLAIN shows the reference derived table materializes `pos_check_extra_infos` joined through `pos_checks` + `pos_business_days` for the same filter window (duplicate work vs outer join).
  3. The report may time out before returning results.
**Fix Result(s):**
  1. The report loads within an acceptable timeframe for the ~7-day range with Show Discount Reference=Yes and Detail.
  2. EXPLAIN shows a single scoped `LEFT JOIN pos_check_extra_infos ON ckei_cdis_id = cdis_id AND ckei_section/variable ...` (or one scoped derived table) instead of re-joining all checks/bdays.
  3. No errors displayed.

#### 1.3. [All Outlets, Last Month, All Discount Types, All Revenue, Show Discount Reference=Yes, Show Discount Reason=Yes, Detail] - Last Month Non-SARGable Rewrite

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. A test environment with a representative data volume for one outlet over the last month.
  3. Note the baseline load time observed before the fix for `last_month` (~135s for 26k result rows vs ~2.9s for the equivalent date-range rewrite).
  4. Capture the EXPLAIN plan of `CheckDiscountsItems` / `CheckNumber` / `sBizDays` before the fix (`YEAR(bday_date) = <y> AND MONTH(bday_date) = <m>` non-SARGable; drives from `pos_discount_types` / type index over discounts).
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select parameters
    2.1. Select "Last Month" from Date Type
    2.2. Select "All" from Shops
    2.3. Select "All" from Outlets
    2.4. Select "Item Discount + Check Discount" from Discount
    2.5. Select "All" from Discount Types
    2.6. Select "All" from Revenue/Non-Revenue
    2.7. Select "Yes" from Show Discount Reference
    2.8. Select "Yes" from Show Discount Reason
    2.9. Select "Detail" from Detail/Summary
  3. Click "Run" button
  4. Measure the time taken from clicking Run until the report fully renders
  5. Capture the EXPLAIN plan of `CheckDiscountsItems` / `CheckNumber` / `sBizDays` after the fix (`bday_date >= '<first_day>' AND bday_date <= '<last_day>'` SARGable; outer path drives from `pos_business_days` outlet_date range).
**Reproduction Result(s):**
  1. `last_month` with `YEAR/MONTH` is ~135s for the same outlet/month and 26k result rows vs ~2.9s for the equivalent date-range rewrite (46x slower).
  2. EXPLAIN drives from `pos_discount_types` / type index over discounts instead of `outlet_date` range on `pos_business_days`.
  3. The report may time out before returning results (support ticket #1933236: report not able to generate).
**Fix Result(s):**
  1. `last_month` uses a SARGable range on `bday_date` (same shape as `specified`): `bday_date >= '<first_day>' AND bday_date <= '<last_day>'` in `CheckDiscountsItems`, `CheckNumber`, `sBizDays`.
  2. The outer path stays date/outlet-driven (`bday` -> `checks` -> `discounts` -> `discount items` -> `items`).
  3. The report loads within an acceptable timeframe for the last-month range (close to the equivalent date-range rewrite ~2.9s for the same scope).
  4. No errors displayed.

#### 1.4. [Multiple Outlets, Specified As Below (Large Range > 7 days), All Discount Types, All Revenue, Show Discount Reference=No, Summary] - Large Range Without Reference/Reason

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. A test environment with a large data volume across multiple outlets over an extended range (> 7 days).
  3. Note the baseline load time observed before the fix for the same scope.
  4. Verify data is sourced correctly from TiDB when the date range exceeds 7 days.
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select parameters
    2.1. Select "Specified As Below" from Date Type
    2.2. Set Begin Date and End Date to cover > 7 days
    2.3. Select "Single" from Shops and pick the test shop
    2.4. Select "Multiple" from Outlets and pick multiple test outlets
    2.5. Select "Item Discount + Check Discount" from Discount
    2.6. Select "All" from Discount Types
    2.7. Select "All" from Revenue/Non-Revenue
    2.8. Select "No" from Show Discount Reference
    2.9. Select "No" from Show Discount Reason
    2.10. Select "Summary" from Detail/Summary
  3. Click "Run" button
  4. Measure the time taken from clicking Run until the report fully renders
  5. Verify data is sourced correctly from TiDB when the date range exceeds 7 days and from MySQL when it does not
**Reproduction Result(s):**
  1. Large range runs are dominated by the correlated `incl_sc` subquery and the `pos_discount_types`-driven path.
  2. Performance degrades sharply as the number of outlets and the date range grow.
**Fix Result(s):**
  1. The report loads within an acceptable timeframe even for a large range without reference/reason.
  2. The `LEFT JOIN` pre-aggregate eliminates the correlated `incl_sc` scans.
  3. Data is sourced correctly from TiDB when the date range exceeds 7 days and from MySQL when it does not.
  4. No errors displayed.

### 2. Data Accuracy Verification

#### 2.1. Incl SC Values Match Source After Pre-Aggregate Rewrite

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. Capture the expected Incl SC values per discount-item row from source `pos_check_tax_sc_refs` (filter `ctsr_variable LIKE 'incl_sc_ref%'`) before the fix.
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select "Current Business Day" from Date Type and a single test outlet.
  3. Click "Run" button.
  4. Compare the displayed Incl SC values against the captured expected values from source data.
  5. Verify no `ctsr_value` rows are missing or duplicated after the pre-aggregate rewrite.
**Reproduction Result(s):**
  1. Incl SC values were correct but slow because of the correlated subquery.
**Fix Result(s):**
  1. Incl SC values match the expected sum per `citm_id` from source data exactly.
  2. No `ctsr_value` rows are missing or duplicated after the pre-aggregate rewrite.
  3. No errors displayed.

#### 2.2. Discount Reference Values Match Source After Scoped Join Rewrite

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. Capture the expected discount reference values per check / discount type from source `pos_check_extra_infos` (section='discount', variable='reference') before the fix.
  3. Include checks with multiple references for the same discount type (separated by "," in the output).
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select "Current Business Day" from Date Type and a single test outlet.
  3. Select "Yes" from Show Discount Reference.
  4. Click "Run" button.
  5. Compare the displayed Discount Reference values against the captured expected values.
  6. Verify multiple references for the same discount type on one check are separated by ",".
**Reproduction Result(s):**
  1. Discount Reference values were correct but slow because of the derived table materialization.
**Fix Result(s):**
  1. Discount Reference values match the expected values from source data exactly.
  2. Multiple references for the same discount type on one check are separated by ",".
  3. No references are missing or duplicated after the scoped join rewrite.
  4. No errors displayed.

#### 2.3. Discount Reason Values Match Source After Direct Join Rewrite

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. Capture the expected discount reason names from `pos_discount_reasons` joined via `dirs_id = ckei_value` (section='discount', variable='reason') before the fix.
  3. Include discounts with no reason (empty displayed).
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select "Current Business Day" from Date Type and a single test outlet.
  3. Select "Yes" from Show Discount Reference.
  4. Select "Yes" from Show Discount Reason.
  5. Click "Run" button.
  6. Compare the displayed Discount Reason values against the captured expected values.
  7. Verify discounts with no reason display empty.
**Reproduction Result(s):**
  1. Discount Reason values were correct but slow because of the nested scalar subquery in `CASE`.
**Fix Result(s):**
  1. Discount Reason values match the expected reason names from `pos_discount_reasons` exactly.
  2. Discounts with no reason display empty.
  3. No errors displayed.

#### 2.4. Net Amount = Gross Amount - Discount After Rewrite

**Prerequisite(s):**
  1. A build with the HERO-68180 fix is deployed.
  2. Capture the expected Gross Amount, Discount, and Net Amount per row from source data before the fix.
**Step(s):**
  1. Open POS018 Discount Report.
  2. Select "Current Business Day" from Date Type and a single test outlet.
  3. Click "Run" button.
  4. Compare the displayed Gross Amount, Discount, and Net Amount against the captured expected values.
  5. Verify Net Amount = Gross Amount - Discount for every row.
**Reproduction Result(s):**
  1. Net Amount was correct but slow because of the correlated subquery and derived table.
**Fix Result(s):**
  1. Gross Amount, Discount, and Net Amount match the expected values exactly.
  2. Net Amount = Gross Amount - Discount for every row.
  3. No errors displayed.

### 3. Regression Scope

- POS018 Discount Report load time across all Date Type selections (Current Business Day, Last Business Day, Last Month, Specified As Below) to confirm the optimization is generic across date scopes.
- POS018 Discount Report load time with Show Discount Reference=Yes/No and Show Discount Reason=Yes/No to confirm the scoped join rewrite handles all combinations.
- POS018 Discount Report load time with Detail/Summary granularity to confirm the rewrite handles both views.
- POS018 Discount Report data accuracy for Incl SC, Discount Reference, Discount Reason, Gross Amount, Discount, Net Amount after the rewrite.
- POS018 Discount Report `sShowAllDiscountReference = 'yes'` path (joins discounts + discount_items + checks + extra_infos again inside the derived table - heavier than the default branch) to confirm whether the same optimization applies.
- POS018 Discount Report `GROUP BY ckei_cdis_id, ckei_citm_id` while selecting non-aggregated `ckei_value` - nondeterministic under `ONLY_FULL_GROUP_BY` (prefer `GROUP_CONCAT` / `MAX`) - confirm the fix uses `GROUP_CONCAT` / `MAX`.
- POS018 Discount Report parameter string concatenation (shops/outlets/discount types/dates) - validate numeric IN-lists before concatenation.
- Related reports with the same root cause: POS017 Hybrid architecture (Script DataSets + in-memory maps for heavy extra-info / tax-sc lookups) as an optional follow-up reference.
- TiDB / compatible environment behaviour of the rewritten queries, and MySQL environment to confirm no side effect.
- Export and print of POS018 to confirm the optimization applies to all output formats (CSV, Excel, PDF, etc.).

## Test Environment

- **Version**: Sprint build containing the HERO-68180 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (Accor HQ or equivalent large HQ test environment), ER Test Environment
- **Database**: MySQL and TiDB (compatible environment) - verify data is sourced from TiDB when date range > 7 days and from MySQL otherwise

## QC Information

- **Tester**: Jacqueline Wei (jacqueline.wei@shijigroup.com)
- **Reviewer**: Eric Fang (eric.fang@shijigroup.com)
