# CUS017 Daily Top GC Earners Report — GC member filter not working, shows non-member checks Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Daily Top GC Earners Report |
| Report Code | CUS017 |
| JIRA Issue | HERO-60071 |
| Component | REPORTS |
| Issue Type | Bug |
| Environment Note | Customer shangrilagroup, TiDB compatible environment; fixed in r70368 |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Shop or outlet that has the checks to be listed (multi-select). Settings: Go to Platform: System Management > System Configuration > Report > Format Configuration: Show Shop Short Name = Yes/No, Default: No | All Outlets, Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business day of the closed checks to be listed (single-select). Settings: Go to Platform: System Management > System Configuration > Report > Query Configuration: Maximum Query Days, Default: Empty, No Limit | Today, Yesterday, Specified as Below | Yesterday |
| No. Of Member | Number of top GC earners to show in the ranking (single-select) | Default, User Defined | Default |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| GC Number | GC membership number taken from the check extra info; checks without a GC member (guest_no = 0) must not be listed | IFNULL(ckei_member_number.ckei_value, 0) AS guest_no; rows with guest_no = '0' are excluded by the fixed filter guest_no <> '0' |
| Member Name | Name of the GC member who earned the points on the check | - |
| Tier | Tier level of the GC member | - |
| Points Earned | Points earned by the GC member on the closed check for the selected business day | Sum of points earned per member for the selected outlet and business day |

## Test Verification

### 1.GC Member Filter Verification

#### 1.1.All Default Parameters

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. For one outlet and one business day, at least one closed check with GC membership exists (membership_interface member number filled, points earned).
  3. For the same outlet and business day, at least one closed check without GC membership exists (no membership_interface member number, guest_no effectively 0).
**Step(s):**
  1. Log in to Hero Report and open CUS017 Daily Top GC Earners Report.
  2. Select the outlet prepared in the prerequisites.
  3. Select the business day of those checks.
  4. Leave No. Of Member as default.
  5. Click "Run" button.
  6. Verify the report lists only GC member-related checks.
**Reproduction Result(s):**
  1. Non-GC-member checks also appear in the report after the TiDB compatibility changes.
  2. Non-member rows show GC Number as 0 with blank member name and blank tier.
  3. Ranking and totals are skewed by the non-member checks.
**Fix Result(s):**
  1. Only GC member-related checks are listed in the report.
  2. Checks without a GC member number (guest_no = 0) do not appear.
  3. GC Number, Member Name and Tier show valid member values.
  4. No errors displayed.

#### 1.2.[A Single Outlet, Specified Business Day, Default No. Of Member] Non-Member Checks Excluded

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. One outlet has both GC member checks and non-member checks closed on the same business day.
**Step(s):**
  1. Open CUS017 Daily Top GC Earners Report.
  2. Select only that single outlet.
  3. Set Business Dates to Specified as Below and choose the business day of the prepared checks.
  4. Leave No. Of Member as default and click "Run" button.
  5. Compare every listed row against the POS check extra infos for the outlet and business day.
**Reproduction Result(s):**
  1. Rows without a GC member number appear in the report with guest_no = '0'.
  2. The old filter guest_no <> '' does not exclude the '0' rows on TiDB.
**Fix Result(s):**
  1. The report filter guest_no <> '0' excludes all non-member checks.
  2. Only checks with a real GC member number are listed.
  3. Each listed row matches a closed check that has GC membership.
  4. No errors displayed.

#### 1.3.Ranking and Totals Not Skewed by Non-Member Checks

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. For one business day, the outlet has non-member checks with amounts large enough to change the top ranking if they were included.
**Step(s):**
  1. Open CUS017 Daily Top GC Earners Report.
  2. Select the outlet and the business day with the mixed checks.
  3. Run the report and note the ranking order and the totals.
  4. Recalculate the expected ranking and totals using only the GC member checks from the POS check data.
**Reproduction Result(s):**
  1. Ranking and totals include non-member checks and do not match the GC-member-only calculation.
**Fix Result(s):**
  1. The ranking order matches the GC-member-only calculation.
  2. Totals are not inflated by non-member checks.
  3. No non-member rows appear between ranked member rows.
  4. No errors displayed.

#### 1.4.No GC Member Checks in Selected Business Day

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. One outlet has only non-member closed checks (no GC membership) on a specific business day.
**Step(s):**
  1. Open CUS017 Daily Top GC Earners Report.
  2. Select the outlet that has only non-member checks.
  3. Select that business day and click "Run" button.
  4. Verify the report behaviour when no GC member check exists.
**Reproduction Result(s):**
  1. Non-member checks leak into the report because the filter does not exclude guest_no = '0'.
**Fix Result(s):**
  1. The report shows no data rows for the selected business day.
  2. No non-member check appears as a GC earner.
  3. The report completes without errors.

### 2.Member Data Correctness Verification

#### 2.1.GC Number, Member Name and Tier Match POS Check Data

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. GC member checks with known member number, name and tier exist for the selected outlet and business day.
**Step(s):**
  1. Open CUS017 Daily Top GC Earners Report.
  2. Select the outlet and business day of the prepared GC member checks.
  3. Run the report.
  4. Spot-check GC Number, Member Name and Tier of the listed rows against the POS check extra infos.
**Reproduction Result(s):**
  1. Some listed rows show GC Number as 0 or blank member name and blank tier because non-member rows pass the filter.
**Fix Result(s):**
  1. Every listed row shows a valid GC Number.
  2. Member Name and Tier match the member data referenced by the POS check extra infos.
  3. No blank member rows are displayed.
  4. No errors displayed.

#### 2.2.Points Earned Match POS Check Extra Infos

**Prerequisite(s):**
  1. A build with the fix r70368 (or later) is deployed.
  2. GC member checks with known points earned exist for the selected outlet and business day.
**Step(s):**
  1. Open CUS017 Daily Top GC Earners Report.
  2. Select the outlet and business day of the prepared checks.
  3. Run the report.
  4. Compare the Points Earned values of the listed rows with the points recorded on the POS checks.
**Reproduction Result(s):**
  1. Totals and per-row points are distorted by non-member checks passing through the filter.
**Fix Result(s):**
  1. Points Earned per member matches the sum of the points from the related GC member checks.
  2. Points from non-member checks are not counted anywhere in the report.
  3. No errors displayed.

### 3.Regression Scope

- CUS017 Daily Top GC Earners Report member filter with All Outlets, a single outlet and multiple outlets selections.
- CUS017 ranking, totals and No. Of Member behaviour after the filter change to guest_no <> '0'.
- Related reports with the same root cause: HERO-60152 (CUS016) and HERO-60153 (CUS019).
- TiDB / compatible environment behaviour of the member number filter, and MySQL environment to confirm no side effect.
- Export and print of CUS017 to confirm excluded non-member rows do not reappear in any output format.

## Test Environment

- **Version**: Sprint build containing the fix r70368 or later
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (TiDB compatible database), ER Test Environment
