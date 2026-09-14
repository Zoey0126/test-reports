# Report - POS015 Discount Report By Check - Apply decimal place and thousands separator per Report Format Configuration Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Discount Report By Check |
| Code | POS015 |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Shops and outlets included in the report (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business date range of the checks (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Yesterday, Today, Specified as Below | Yesterday |
| Decimal Place | Number of decimal places applied to the numeric values in the report, configured under System Management > System Configuration > Report Module (single-select) | 0, 1, 2 | 2 |
| Thousand Separator | Whether thousands separators are applied to the numeric values in the report, configured under System Management > System Configuration > Report Module (single-select) | Enabled, Disabled | Enabled |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Item Department | Shows the item department of the discount records | - |
| Item Department Amount | Numeric value of the Item Department columns, formatted per the active Report Format Configuration. Settings: Go to Platform: System Management > System Configuration > Report Module: Decimal Place, Default: 2; Thousand Separator, Default: Enabled | Formatted with exactly the configured decimal-place count and thousands separators per the Report Format Configuration |

## Report Data Accuracy Verification

### 1.Decimal Place Formatting

#### 1.1.Verify Item Department columns keep exactly the configured decimal places (Decimal Place = 2)
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report Module and set Decimal Place = 2.
  2. Discount data with numeric Item Department values (e.g., 10.5) exists for the selected period.
**Step(s):**
  1. Open POS015 Discount Report By Check.
  2. Keep all parameters at their default values.
  3. Click "Run" to generate the report.
  4. Check the numeric values in the Item Department columns.
**Test Result(s):**
  1. All numeric values in the Item Department columns display with exactly 2 decimal places (e.g., 10.5 displays as 10.50).
  2. Trailing zeros are retained and no decimals are dropped or added.
  3. The report loads successfully with no errors displayed.

#### 1.2.Verify Decimal Place = 1 displays values with exactly one decimal place
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report Module and set Decimal Place = 1.
**Step(s):**
  1. Open POS015 Discount Report By Check.
  2. Generate the report with default parameters.
  3. Check the numeric values in the Item Department columns.
**Test Result(s):**
  1. All numeric values in the Item Department columns display with exactly 1 decimal place per the existing rounding rules.
  2. The report loads successfully with no errors displayed.

#### 1.3.Verify Decimal Place = 0 removes all decimal places per existing rounding rules
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report Module and set Decimal Place = 0.
  2. Discount data with decimal values (e.g., 10.50) exists for the selected period.
**Step(s):**
  1. Open POS015 Discount Report By Check.
  2. Generate the report with default parameters.
  3. Check the numeric values in the Item Department columns.
**Test Result(s):**
  1. All decimal places are removed (e.g., 10.50 becomes 11 when rounded, or 10 when truncated per existing rounding rules).
  2. The report loads successfully with no errors displayed.

### 2.Thousand Separator Formatting

#### 2.1.Verify thousands separators are applied when the Thousand Separator setting is enabled
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report Module and set Thousand Separator = Enabled.
  2. Discount data with large values (e.g., 1234567.89) exists for the selected period.
**Step(s):**
  1. Open POS015 Discount Report By Check.
  2. Generate the report with default parameters.
  3. Check the numeric values in the Item Department columns.
**Test Result(s):**
  1. Thousands separators are applied to all relevant numeric values (e.g., 1234567.89 displays as 1,234,567.89).
  2. The report loads successfully with no errors displayed.

#### 2.2.Verify no thousands separators are applied when the Thousand Separator setting is disabled
**Prerequisite(s):**
  1. Go to Platform: System Management > System Configuration > Report Module and set Thousand Separator = Disabled.
**Step(s):**
  1. Open POS015 Discount Report By Check.
  2. Generate the report with default parameters.
  3. Check the numeric values in the Item Department columns.
**Test Result(s):**
  1. No thousands separators are applied to any numeric values (e.g., the value displays as 1234567.89).
  2. The report loads successfully with no errors displayed.

## Report Functional Verification

### 1.Verify identical formatting across on-screen, PDF, and Excel/CSV outputs
**Prerequisite(s):**
  1. The Report Format Configuration is set (e.g., Decimal Place = 2, Thousand Separator = Enabled).
**Step(s):**
  1. Open POS015 Discount Report By Check and generate the report with default parameters.
  2. Check the Item Department column values on the on-screen display.
  3. Select "Export" from the menu and export the report to PDF, then check the Item Department column values.
  4. Select "Export" from the menu and export the report to Excel/CSV, then check the Item Department column values.
**Test Result(s):**
  1. The decimal place and thousands separator formatting is applied identically across the on-screen, PDF, and Excel/CSV outputs.
  2. If any output format cannot render the configured formatting, the value is still displayed without corrupting or dropping the underlying numeric data.
  3. No errors displayed.

### 2.Verify HQ and Local produce identical formatted output for the same underlying data
**Prerequisite(s):**
  1. The HQ backend and a local store terminal both read from the same Report Format Configuration under System Configuration > Report Module.
  2. The same underlying discount data is available in both environments.
**Step(s):**
  1. Generate POS015 Discount Report By Check from the HQ backend with default parameters.
  2. Generate POS015 Discount Report By Check from the local store terminal with the same parameters.
  3. Compare the Item Department column values of both outputs.
**Test Result(s):**
  1. HQ and Local produce identical formatted output (same decimal places and thousands separators) for the same underlying data.
  2. No errors displayed in either environment.

### 3.Verify configuration changes apply to newly generated reports only
**Prerequisite(s):**
  1. A POS015 report was previously generated and exported/saved with the old Report Format Configuration (e.g., Decimal Place = 2).
**Step(s):**
  1. Change the Report Format Configuration under System Management > System Configuration > Report Module (e.g., change Decimal Place from 2 to 0).
  2. Open the previously saved/exported POS015 report file.
  3. Verify the formatting of the previously saved/exported report.
  4. Generate a new POS015 Discount Report By Check with the same parameters.
  5. Verify the formatting of the newly generated report.
**Test Result(s):**
  1. The previously saved/exported report remains unchanged with the old formatting.
  2. The newly generated report reflects the updated Report Format Configuration.
  3. Regenerating the report again also reflects the new formatting.

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
