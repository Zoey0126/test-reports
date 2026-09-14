# POS006 Detail Check Report - Optimize loading performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Detail Check Report |
| Code | POS006 |

## Report Functional Verification

### 1.Report Loading Performance

#### 1.1.Verify POS006 loading performance with default parameters
**Prerequisite(s):**
  1. POS006 Detail Check Report is accessible in the test environment.
  2. The test environment contains a large volume of check records for the selected period.
  3. The loading time of POS006 before the fix has been recorded as the baseline.
**Step(s):**
  1. Open POS006 Detail Check Report.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the time from clicking "Run" until the report result is fully displayed.
  5. Verify the report content is complete and correct.
**Reproduction Result(s):**
  1. Before the fix, POS006 takes an excessively long time to load when generating the report with default parameters.
**Fix Result(s):**
  1. After the fix, POS006 loads and returns the report result noticeably faster than the baseline under the same parameters.
  2. The report result is complete and correct, and no errors are displayed.

#### 1.2.Verify loading performance with a wide business date range and multiple outlets
**Prerequisite(s):**
  1. A large volume of check records exists across a wide business date range (e.g., one month or more) and multiple outlets.
**Step(s):**
  1. Open POS006 Detail Check Report.
  2. Select a wide business date range and multiple outlets.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, generating POS006 with a wide business date range and multiple outlets takes an excessively long time to load.
**Fix Result(s):**
  1. After the fix, the report loads noticeably faster under the same conditions.
  2. The report data is complete and correct for the selected range and outlets, and no errors are displayed.

#### 1.3.Verify loading performance with a high volume of checks
**Prerequisite(s):**
  1. Outlets with a high volume of checks are available in the test environment.
**Step(s):**
  1. Open POS006 Detail Check Report.
  2. Select the outlets covering the high-volume check data.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, generating POS006 for a high volume of checks takes an excessively long time to load.
**Fix Result(s):**
  1. After the fix, the report loads noticeably faster under the same conditions.
  2. The report data is complete and correct, and no errors are displayed.

### 2.Verify report data is consistent and correct after the performance optimization
**Prerequisite(s):**
  1. A POS006 report output generated before the fix (or the source check data) is available as the comparison baseline.
**Step(s):**
  1. Open POS006 Detail Check Report.
  2. Generate the report with the same parameters as the baseline output.
  3. Compare the check detail rows and totals against the baseline or the source data.
**Reproduction Result(s):**
  1. Before the fix, the report could only be verified after a long loading time due to the performance issue.
**Fix Result(s):**
  1. After the fix, the report loads faster and the displayed check detail data is correct and consistent with the baseline or source data.
  2. No data is lost, duplicated, or wrongly aggregated.

### 3.Regression Scope
- POS006 Detail Check Report generation with default and common parameter combinations
- POS006 export and print functions (PDF, Excel/CSV, HTML)
- Preset parameters and auto report schedules related to POS006, if configured
- Other check detail reports sharing the same data source, if any

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
