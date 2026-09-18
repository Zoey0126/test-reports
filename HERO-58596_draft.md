# POS240 Revenue Report (By Station) Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Revenue Report (By Station) |
| Code | POS240 |
| Issue Key | HERO-58596 |
| Issue Type | Bug |
| Status | READY FOR QA |
| Bug Subject | Unusual rounding issues and optimize loading performance |

## Bug Summary

The POS240 Revenue Report (By Station) exhibits two defects:

1. **Unusual rounding issues** – Revenue figures displayed in the report contain unexpected rounding errors, causing incorrect totals and per-station amounts that do not match source data.
2. **Slow loading performance** – Several queries identified in the report are slow, leading to unacceptable load times especially when the date range or station scope grows.

The fix optimizes the running speed of POS240 Revenue Report (By Station) and resolves the rounding error.

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Filter report by outlet/station scope | All Outlets, A Single Outlet, Multiple Outlets | All Outlets |
| Business Date | Select the business date range | Today, Yesterday, This Week, This Month, Custom | Today |
| Period | Filter report by period | All, A Single Period, Multiple Periods | All |
| Group By Outlet | Group report output by station/outlet | Grouping, No Grouping | Grouping |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Station/Outlet Name | Name of the station/outlet | Sourced from outlet configuration |
| Revenue Amount | Total revenue for the station | Sum of revenue transactions per station |
| Rounding Applied | Rounding adjustment applied to revenue | Should follow system rounding configuration (Decimal Places + Rounding setting in Format Configuration) |
| Grand Total | Aggregated revenue across all stations | Sum of per-station revenue with consistent rounding |

### Report Functional Verification

#### 1. Verify Revenue Rounding Accuracy – Single Station
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Confirm the Rounding and Decimal Places settings for the report.
  3. Prepare transactions for a single station whose revenue totals produce fractional values that trigger rounding (e.g., amounts with 3+ decimal places).
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "A Single Outlet" from Shops/Outlets
  3. Select "Today" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Observe the Revenue Amount displayed for the selected station
  8. Compare the displayed Revenue Amount against the source transaction data and the configured Decimal Places / Rounding rule
**Reproduction Result(s):**
  1. The Revenue Amount displayed for the station shows an unexpected rounding value that does not match the configured Decimal Places / Rounding setting
  2. The per-station total deviates from the sum of the underlying transactions by a fractional cent
  3. Rounding is applied inconsistently across rows
**Fix Result(s):**
  1. The Revenue Amount displayed for the station matches the configured Decimal Places / Rounding setting exactly
  2. The per-station total equals the sum of the underlying transactions after applying the configured rounding rule
  3. Rounding is applied consistently across all rows
  4. No errors displayed

#### 2. Verify Revenue Rounding Accuracy – Multiple Stations with Grand Total
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Confirm the Rounding and Decimal Places settings for the report.
  3. Prepare transactions across multiple stations whose summed totals produce rounding discrepancies at the grand total level.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "Multiple Outlets" from Shops/Outlets
  3. Select "Yesterday" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Observe each per-station Revenue Amount
  8. Observe the Grand Total row
  9. Manually sum the per-station revenue values and compare against the displayed Grand Total
**Reproduction Result(s):**
  1. The Grand Total does not equal the sum of the per-station revenue values due to unusual rounding
  2. Per-station amounts and the grand total round inconsistently, introducing a discrepancy of fractional cents
  3. The grand total appears inflated or deflated relative to the true aggregate
**Fix Result(s):**
  1. The Grand Total equals the sum of the per-station revenue values after consistent rounding
  2. Per-station amounts and the grand total round consistently per the configured rule
  3. The grand total matches the true aggregate
  4. No errors displayed

#### 3. Verify Rounding Consistency Across Date Ranges
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Confirm the Rounding and Decimal Places settings for the report.
  3. Prepare transactions across a multi-day range with fractional revenue values.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "All Outlets" from Shops/Outlets
  3. Select "This Week" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Observe the Revenue Amounts for each station across the multi-day range
  8. Verify the rounding behavior is identical for each day/station combination
**Reproduction Result(s):**
  1. Rounding applied to the same station differs across days within the selected range
  2. Some rows show rounding up while others round down for the same fractional value
  3. Totals become unreliable when aggregated across the date range
**Fix Result(s):**
  1. Rounding is applied uniformly across all days and stations within the range
  2. Identical fractional values round identically in every row
  3. Aggregated totals are reliable across the date range
  4. No errors displayed

#### 4. Verify Loading Performance – Normal Data Volume
**Prerequisite(s):**
  1. Prepare a test environment with a normal volume of transactions for the selected station and date range (single business day, single station).
  2. Note the load time observed before the fix (baseline) for comparison.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "A Single Outlet" from Shops/Outlets
  3. Select "Today" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Measure the time taken from clicking Run until the report fully renders
**Reproduction Result(s):**
  1. The report takes an unacceptably long time to load even for a normal data volume
  2. Slow queries are executed as identified in the ticket, causing noticeable delay before results appear
  3. The UI may appear frozen during loading
**Fix Result(s):**
  1. The report loads within an acceptable timeframe for a normal data volume
  2. The previously identified slow queries have been optimized and no longer dominate load time
  3. The UI remains responsive during loading
  4. No errors displayed

#### 5. Verify Loading Performance – Large Data Volume
**Prerequisite(s):**
  1. Prepare a test environment with a large volume of transactions across multiple stations and an extended date range (more than 7 days).
  2. Note the load time observed before the fix (baseline) for comparison.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "All Outlets" from Shops/Outlets
  3. Select "This Month" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Measure the time taken from clicking Run until the report fully renders
  8. Verify data is sourced correctly from TiDB when the date range exceeds 7 days
**Reproduction Result(s):**
  1. The report takes an excessive amount of time to load for a large data volume
  2. Slow queries cause the report to hang or time out
  3. Performance degrades sharply as the number of stations and the date range grow
  4. The slow queries identified in the ticket dominate execution time
**Fix Result(s):**
  1. The report loads within an acceptable timeframe even for a large data volume
  2. Query optimization eliminates the previously identified slow queries
  3. Performance scales reasonably as the number of stations and the date range grow
  4. Data is sourced correctly from TiDB when the date range exceeds 7 days and from MySQL when it does not
  5. No errors displayed

#### 6. Verify Data Accuracy After Performance Optimization
**Prerequisite(s):**
  1. Prepare transactions across multiple stations with known expected revenue totals.
  2. Capture the expected revenue values from source data prior to running the report.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "All Outlets" from Shops/Outlets
  3. Select "This Week" from Business Date
  4. Select "All" from Period
  5. Select "Grouping" from Group By Outlet
  6. Click "Run" button
  7. Compare the displayed per-station revenue and grand total against the captured expected values
  8. Verify no transactions are missing or duplicated after the query optimization
**Reproduction Result(s):**
  1. The optimization changes introduce data discrepancies (missing or duplicated transactions)
  2. Per-station revenue and grand total do not match the expected values
  3. The report is faster but returns incorrect data
**Fix Result(s):**
  1. Per-station revenue and grand total match the expected values exactly after applying the configured rounding rule
  2. No transactions are missing or duplicated
  3. The report is both fast and accurate
  4. No errors displayed

#### 7. Verify Rounding with No Grouping
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report > Format Configuration.
  2. Confirm the Rounding and Decimal Places settings for the report.
  3. Prepare transactions across multiple stations with fractional revenue values.
**Step(s):**
  1. Open POS240 Revenue Report (By Station)
  2. Select "Multiple Outlets" from Shops/Outlets
  3. Select "Yesterday" from Business Date
  4. Select "A Single Period" from Period
  5. Select "No Grouping" from Group By Outlet
  6. Click "Run" button
  7. Observe the revenue values displayed for each transaction/station row
  8. Verify the rounding applied matches the configured Decimal Places / Rounding rule
**Reproduction Result(s):**
  1. Unusual rounding is present in the ungrouped view as well
  2. Per-row amounts round inconsistently when not grouped by outlet
  3. Totals shown in the ungrouped view deviate from the true aggregate
**Fix Result(s):**
  1. Revenue values in the ungrouped view follow the configured Decimal Places / Rounding rule
  2. Per-row amounts round consistently
  3. Totals match the true aggregate
  4. No errors displayed

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## QC Information
- **Tester**: Kathy Kuang (kathy.kuang@shijigroup.com)
- **Reviewer**: Niko Xie (niko.xie@shijigroup.com)
