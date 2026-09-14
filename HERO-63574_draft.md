# CUS026 Detail Check Listing Report (ANZ) | Optimize loading performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Detail Check Listing Report (ANZ) |
| Code | CUS026 |

## Report Functional Verification

### 1.Report Loading Performance

#### 1.1.Verify CUS026 loading performance with default parameters
**Prerequisite(s):**
  1. CUS026 Detail Check Listing Report (ANZ) is accessible in the test environment.
  2. The test environment contains a large volume of check records for the selected period.
  3. The loading time of CUS026 before the fix has been recorded as the baseline.
**Step(s):**
  1. Open CUS026 Detail Check Listing Report (ANZ).
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the time from clicking "Run" until the report result is fully displayed.
  5. Verify the report content is complete and correct.
**Reproduction Result(s):**
  1. Before the fix, CUS026 takes a long time to load when generating the report with default parameters (ticket #1774993 reported the report extraction taking too long).
**Fix Result(s):**
  1. After the enhancement, CUS026 loads and returns the report result faster than the baseline under the same parameters.
  2. The report result is complete and correct, and no errors are displayed.

#### 1.2.Verify loading performance with a larger date range
**Prerequisite(s):**
  1. Check data exists across a larger date range (e.g., one month or more).
**Step(s):**
  1. Open CUS026 Detail Check Listing Report (ANZ).
  2. Select a larger business date range and keep the other parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, running CUS026 for a larger date range took a long time to load.
**Fix Result(s):**
  1. After the enhancement, the report loads noticeably faster for the same larger date range.
  2. The report data is complete and correct, and no errors are displayed.

#### 1.3.Verify loading performance with multiple outlets
**Prerequisite(s):**
  1. Multiple outlets with check data are available in the test environment.
**Step(s):**
  1. Open CUS026 Detail Check Listing Report (ANZ).
  2. Select multiple outlets and keep the other parameters at their default values.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, running CUS026 for multiple outlets took a long time to load.
**Fix Result(s):**
  1. After the enhancement, the report loads noticeably faster for the same outlets.
  2. The report data is complete and correct, and no errors are displayed.

#### 1.4.Verify loading performance with a high volume of checks
**Prerequisite(s):**
  1. Outlets with a high volume of checks are available in the test environment.
**Step(s):**
  1. Open CUS026 Detail Check Listing Report (ANZ).
  2. Select the outlets covering the high-volume check data.
  3. Click "Run" to generate the report.
  4. Measure the loading time and verify the report content.
**Reproduction Result(s):**
  1. Before the fix, running CUS026 for a high volume of checks took a long time to load.
**Fix Result(s):**
  1. After the enhancement, the report loads noticeably faster under the same conditions.
  2. The report data is complete and correct, and no errors are displayed.

### 2.Verify report parameters, layout, and data meaning remain unchanged
**Prerequisite(s):**
  1. A CUS026 report output generated before the fix is available as the comparison baseline.
**Step(s):**
  1. Open CUS026 Detail Check Listing Report (ANZ).
  2. Verify the same report parameters as before are available and selectable.
  3. Generate the report with the same parameters as the baseline output.
  4. Compare the report layout, columns, and field meaning with the baseline output.
  5. Compare the report data with the baseline output.
**Reproduction Result(s):**
  1. Before the fix, the report content could only be obtained after a long loading time due to the performance issue.
**Fix Result(s):**
  1. After the enhancement, the report loads faster and the parameters, layout, and data meaning are the same as before.
  2. The report data is identical to the baseline output for the same parameters.

### 3.Regression Scope
- CUS026 Detail Check Listing Report (ANZ) generation with default and common parameter combinations
- CUS026 export and print functions (PDF, Excel/CSV, print)
- Preset parameters and auto report schedules related to CUS026, if configured
- Other ANZ check listing reports sharing the same data source, if any

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
