# POS202 Discount Detail Report - optimize loading performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Discount Detail Report |
| Code | POS202 |

## Report Functional Verification

### 1.Report Loading Performance

#### 1.1.Verify POS202 loading performance with default parameters
**Prerequisite(s):**
  1. POS202 Discount Detail Report is accessible in the test environment.
  2. The test environment contains a large volume of discount records for the selected period.
  3. The loading time of POS202 before the fix has been recorded as the baseline.
**Step(s):**
  1. Open POS202 Discount Detail Report.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the time from clicking "Run" until the report result is fully displayed.
  5. Verify the report content is complete and correct.
**Reproduction Result(s):**
  1. Before the fix, POS202 takes an excessively long time to load when generating the report with default parameters, and the user has to wait a long time before the result is displayed.
**Fix Result(s):**
  1. After the fix, POS202 loads and returns the report result noticeably faster than the baseline under the same parameters.
  2. The report result is complete and correct, and no errors are displayed.

#### 1.2.Verify loading performance with a wide business date range
**Prerequisite(s):**
  1. A large volume of discount records exists across a wide business date range (e.g., one month or more).
**Step(s):**
  1. Open POS202 Discount Detail Report.
  2. Select a wide business date range and keep the other parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, generating POS202 with a wide business date range takes an excessively long time to load.
**Fix Result(s):**
  1. After the fix, the report loads noticeably faster for the same wide date range.
  2. The report data is complete and correct for the selected range, and no errors are displayed.

#### 1.3.Verify loading performance with multiple outlets and a high volume of checks
**Prerequisite(s):**
  1. Multiple outlets with a high volume of checks and discount records are available in the test environment.
**Step(s):**
  1. Open POS202 Discount Detail Report.
  2. Select multiple outlets covering the high-volume data.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, generating POS202 for multiple outlets with a high volume of checks takes an excessively long time to load.
**Fix Result(s):**
  1. After the fix, the report loads noticeably faster under the same conditions.
  2. The report data is complete and correct, and no errors are displayed.

### 2.Verify report data is consistent and correct after the performance optimization
**Prerequisite(s):**
  1. A POS202 report output generated before the fix (or the source check/discount data) is available as the comparison baseline.
**Step(s):**
  1. Open POS202 Discount Detail Report.
  2. Generate the report with the same parameters as the baseline output.
  3. Compare the discount detail rows and totals against the baseline or the source data.
**Reproduction Result(s):**
  1. Before the fix, the report could only be verified after a long loading time due to the performance issue.
**Fix Result(s):**
  1. After the fix, the report loads faster and the displayed discount detail data is correct and consistent with the baseline or source data.
  2. No data is lost, duplicated, or wrongly aggregated.

### 3.Regression Scope
- POS202 Discount Detail Report generation with default and common parameter combinations
- POS202 export and print functions (PDF, Excel/CSV, HTML)
- Preset parameters and auto report schedules related to POS202, if configured
- Related discount reports (e.g., POS015 Discount Report By Check, POS225 OC/ENT Discount Report) to confirm the shared discount data logic is not affected

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
