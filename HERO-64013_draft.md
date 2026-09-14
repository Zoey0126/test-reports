# CUS205 cover_breakdown_melco | Include Outlet in Business Day filter Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Cover Breakdown (Melco) |
| Report Code | CUS205 |
| JIRA Issue | HERO-64013 |
| Component | REPORTS |
| Issue Type | Bug |
| Related Ticket | INC0683679 / Ticket #1779600 (daily report xlsx issue on business day 28 Feb 2026) |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Outlet selection; the business day filter now includes the outlet so each outlet is evaluated with its own business day (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business day filter applied per selected outlet (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Today, Yesterday, Specified as Below | Yesterday |
| Begin Date / End Date | Business day range boundaries, enabled when Business Dates = 'Specified as Below' | User Defined | - |

## Test Verification

### 1.Business Day Filter With Outlet Verification

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. Cover data exists for the outlets and business day under test.
**Step(s):**
  1. Open the CUS205 cover_breakdown_melco report.
  2. Select "default" from all parameters.
    2.1. Select All Outlets from Shops/Outlets.
    2.2. Select Yesterday from Business Dates.
  3. Run the report.
  4. Verify the report returns cover data of the selected outlets for their own business day.
**Reproduction Result(s):**
  1. The business day filter does not include the outlet, so the report returns cover data based on the property business day only.
  2. Rows from the wrong business day appear for outlets whose business day differs from the property business day.
**Fix Result(s):**
  1. The Business Day filter includes the Outlet, and the report returns cover data for each outlet business day.
  2. Data displayed is correct for the default parameter combination.
  3. No errors displayed.

#### 1.2.[A Single Outlet, Specified Business Day] Covers Match the Outlet Business Day

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. One outlet has known cover counts on a specific outlet business day.
**Step(s):**
  1. Open the CUS205 cover_breakdown_melco report.
  2. Select only that single outlet.
  3. Set Business Dates to Specified as Below and choose the outlet business day of the prepared covers.
  4. Run the report.
  5. Compare the cover data shown with the source data recorded for that outlet business day.
**Reproduction Result(s):**
  1. The report mixes covers of adjacent calendar days because the outlet is not part of the business day filter.
**Fix Result(s):**
  1. The report shows only the covers of the selected outlet for its own business day.
  2. Cover counts match the source data of that outlet business day exactly.
  3. No errors displayed.

#### 1.3.[Multiple Outlets, Specified Business Day] Each Outlet Uses Its Own Business Day

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. Two or more outlets with different business day crossover times have cover data on the same calendar day.
**Step(s):**
  1. Open the CUS205 cover_breakdown_melco report.
  2. Select multiple outlets in Shops/Outlets.
  3. Set Business Dates to Specified as Below and choose the business day under test.
  4. Run the report.
  5. Verify the cover data of every selected outlet row by row.
**Reproduction Result(s):**
  1. Outlets with a different business day crossover show cover data of the wrong business day in the same report run.
**Fix Result(s):**
  1. Each selected outlet shows cover data of its own business day.
  2. No outlet inherits the business day of another outlet or of the property.
  3. No errors displayed.

#### 1.4.Outlets With Different Business Day Crossover Times

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. One outlet has a business day crossover different from the property (for example 06:00 vs 03:00) and has covers recorded around both crossover times.
**Step(s):**
  1. Open the CUS205 cover_breakdown_melco report.
  2. Select the outlet with the different crossover time.
  3. Select the business day around the crossover and run the report.
  4. Verify that covers before and after the crossover time are assigned to the correct outlet business day.
**Reproduction Result(s):**
  1. Covers near the crossover time fall into the wrong business day because the filter ignores the outlet.
**Fix Result(s):**
  1. Covers before the crossover time belong to the previous outlet business day.
  2. Covers after the crossover time belong to the selected outlet business day.
  3. Total covers per business day match the source data.
  4. No errors displayed.

#### 1.5.Business Day With No Covers for an Outlet

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. One outlet has no cover data on the selected business day but has cover data on the adjacent business day.
**Step(s):**
  1. Open the CUS205 cover_breakdown_melco report.
  2. Select that outlet and the business day without covers.
  3. Run the report.
  4. Verify the report result for the empty business day.
**Reproduction Result(s):**
  1. Cover data of the adjacent business day leaks into the selected business day because the filter does not include the outlet.
**Fix Result(s):**
  1. The report shows no data rows for the outlet on the business day without covers.
  2. No cover data is borrowed from the adjacent business day.
  3. The report completes without errors.

#### 1.6.Daily Report (xlsx) Generated With Correct Business Day Data

**Prerequisite(s):**
  1. A build with the HERO-64013 fix is deployed.
  2. The scheduled daily report (xlsx) for the affected scenario (COD/SC/ATR/Mocha) is configured.
**Step(s):**
  1. Trigger or wait for the scheduled daily CUS205 xlsx generation for a business day.
  2. Open the generated xlsx file.
  3. Verify the business day shown in the file matches the outlet business day of the data rows.
**Reproduction Result(s):**
  1. The daily xlsx generated for business day 28 Feb 2026 contains data filtered without the outlet, producing incorrect daily figures.
**Fix Result(s):**
  1. The generated daily xlsx contains only cover data of the correct outlet business day.
  2. Figures in the file match the on-screen report for the same business day.
  3. No errors displayed.

### 2.Regression Scope

- CUS205 cover_breakdown_melco with All Outlets, a single outlet and multiple outlets selections.
- Outlets whose business day crossover differs from the property business day.
- Scheduled daily report (xlsx) generation for the COD/SC/ATR/Mocha scenario referenced in INC0683679.
- Export and print of CUS205 to confirm business day filtering stays correct in all output formats.
- Business day range selections spanning two calendar days to confirm no data duplication or loss.

## Test Environment

- **Version**: Sprint build containing the HERO-64013 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
