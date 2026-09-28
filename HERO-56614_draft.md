# CUS245/CUS371 Report | Grand Total's Incl Sc value is incorrect Test Report

## Functional Testing

### 1.Grand Total Incl Sc Calculation

#### 1.1.CUS245 Grand Total Incl Sc equals the sum of individual check Incl Sc values

**Prerequisite(s):**
1. CUS245 Cross Outlet Revenue Report with Cost and Payment Group is deployed.
2. A report run contains multiple outlets and multiple checks within the selected date range.
3. Each check contains values for Incl Sc (Service Charge Inclusive).

**Step(s):**
1. Open CUS245 Cross Outlet Revenue Report with Cost and Payment Group.
2. Run the report with a date range and outlet scope that include multiple checks.
3. Identify each check's Incl Sc value in the report.
4. Locate the Grand Total row in the report.
5. Compare the Grand Total Incl Sc value with the sum of all individual check Incl Sc values.
6. Repeat the run across different outlet scopes (single outlet, multiple outlets, all outlets) and verify the Grand Total Incl Sc value each time.

**Reproduction Result(s):**
1. The Grand Total Incl Sc value does not match the sum of the individual check Incl Sc values across the report scope (bug reproduced).
2. The discrepancy is observed regardless of whether the run is scoped to a single outlet, multiple outlets, or all outlets.

**Fix Result(s):**
1. The Grand Total Incl Sc value equals the sum of the individual check Incl Sc values for the selected scope.
2. The result is consistent across single outlet, multiple outlet, and all outlet scopes.
3. No negative impact on other Grand Total columns (Net Sales, VAT, Total, etc.) is observed.

#### 1.2.CUS371 Grand Total Incl Sc equals the sum of individual check Incl Sc values

**Prerequisite(s):**
1. CUS371 Cross Outlet Revenue Report is deployed.
2. A report run contains multiple outlets and multiple checks within the selected date range.
3. Each check contains values for Incl Sc.

**Step(s):**
1. Open CUS371 Cross Outlet Revenue Report.
2. Run the report with a date range and outlet scope that include multiple checks.
3. Identify each check's Incl Sc value in the report.
4. Locate the Grand Total row in the report.
5. Compare the Grand Total Incl Sc value with the sum of all individual check Incl Sc values.
6. Repeat the run with different outlet scopes and date ranges to verify the Grand Total Incl Sc value.

**Reproduction Result(s):**
1. The Grand Total Incl Sc value does not match the sum of the individual check Incl Sc values in CUS371 (bug reproduced).
2. The mismatch persists across different outlet scopes and date range selections.

**Fix Result(s):**
1. The Grand Total Incl Sc value in CUS371 equals the sum of the individual check Incl Sc values.
2. Results are consistent across different outlet scopes and date range selections.

### 2.Cross-Report Consistency

#### 2.1.CUS245 and CUS371 Grand Total Incl Sc values match for the same data scope

**Prerequisite(s):**
1. CUS245 and CUS371 are deployed.
2. The same business date and outlet scope are used for both reports.

**Step(s):**
1. Run CUS245 with a chosen outlet scope and date range.
2. Record the Grand Total Incl Sc value from CUS245.
3. Run CUS371 with the same outlet scope and date range.
4. Record the Grand Total Incl Sc value from CUS371.
5. Compare the two Grand Total Incl Sc values.

**Reproduction Result(s):**
1. The Grand Total Incl Sc values from CUS245 and CUS371 do not match for the same data scope (bug reproduced).

**Fix Result(s):**
1. The Grand Total Incl Sc values from CUS245 and CUS371 are identical for the same data scope.
2. Both reports compute Incl Sc using the same underlying logic.

## Test Environment Information

- **Test Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
- **Related Reports**: CUS245 Cross Outlet Revenue Report with Cost and Payment Group, CUS371 Cross Outlet Revenue Report

## Appendix

### Acceptance Criteria (from JIRA)

Issue 1: Grand Total's Incl Sc value is incorrect.

Affected Reports:
- CUS245 Cross Outlet Revenue Report with Cost and Payment Group.
- CUS371 Cross Outlet Revenue Report.

Expected Result: Grand Total's Incl Sc value should equal the sum of individual check Incl Sc values within the selected scope and should be consistent between CUS245 and CUS371 for the same data scope.