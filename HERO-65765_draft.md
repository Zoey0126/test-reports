# POS056 Detail Check Payment - optimize loading performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Detail Check Payment |
| Code | POS056 |
| Issue Key | HERO-65765 |
| Issue Type | Bug |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Shops/Outlets | Outlets included in the detail check payment listing | All Outlets, A Single Outlet, Multiple Outlets | All Outlets |
| Business Dates | Business date range of the checks | Today, Yesterday, This Week, Custom | Today |
| Period | Meal period filter | All, A Single Period, Multiple Periods | All |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Check payment rows | Payment detail rows of each check | Must match pre-fix POS056 result for the same parameters |
| Report totals | Payment totals in the report | Must equal source check payment totals |

## Report Data Accuracy Verification

### 1.Loading Performance After Optimization

#### 1.1.All Default Parameters
**Prerequisite(s):**
1. A build with the HERO-65765 fix is deployed
2. POS056 payment data exists for the default date range
3. A baseline load time from the slow build is recorded

**Step(s):**
1. Open POS056 Detail Check Payment
2. Select default values for all parameters
3. Click "Run"
4. Record the time until the report finishes loading
5. Verify the payment rows against known checks

**Reproduction Result(s):**
1. POS056 takes an unacceptably long time to load even for a normal business-date range, blocking daily payment review.

**Fix Result(s):**
1. Report loads successfully within an acceptable time compared with the slow baseline
2. Payment rows and totals are complete and correct
3. No errors displayed

#### 1.2.Single Outlet, Long Date Range
**Prerequisite(s):**
1. A build with the HERO-65765 fix is deployed
2. High-volume payment data exists for more than 7 days on a single outlet

**Step(s):**
1. Open POS056
2. Select a single outlet
3. Set a date range of more than 7 days
4. Click "Run"
5. Record load time and verify a sample of checks against POS inquiry

**Reproduction Result(s):**
1. Long date range on POS056 is extremely slow or appears to hang, so users cannot finish payment detail review.

**Fix Result(s):**
1. Report finishes loading without hanging
2. Sampled check payment rows match POS inquiry
3. Totals are correct
4. No errors displayed

#### 1.3.Multiple Outlets, High Volume Day
**Prerequisite(s):**
1. A build with the HERO-65765 fix is deployed
2. Multiple outlets have a high volume of checks and payments on the selected business date

**Step(s):**
1. Open POS056
2. Select multiple outlets
3. Select the high-volume business date
4. Click "Run"
5. Compare row count and payment totals with a known control query or previous export

**Reproduction Result(s):**
1. High-volume days cause POS056 loading to degrade sharply and may time out.

**Fix Result(s):**
1. Report loads successfully for the high-volume day
2. Row count and payment totals match the control source
3. No timeout error is displayed

### 2.Result Accuracy Is Unchanged

#### 2.1.Payment Types And Partial Payments
**Prerequisite(s):**
1. Checks exist with multiple payment types and partial payments
2. A build with the HERO-65765 fix is deployed

**Step(s):**
1. Run POS056 for the outlet and date that contain those checks
2. Locate the checks in the report
3. Compare each payment row with check payment inquiry

**Reproduction Result(s):**
1. Performance issue made it difficult to complete this verification; data correctness must still be confirmed after the optimization.

**Fix Result(s):**
1. Each payment type is listed correctly
2. Partial payments are not merged or dropped
3. Amounts match check payment inquiry

#### 2.2.Export After Optimized Load
**Prerequisite(s):**
1. POS056 has been run successfully after the fix

**Step(s):**
1. Export the report to Excel and PDF
2. Compare exported payment rows with the on-screen result

**Reproduction Result(s):**
1. Users could not reliably export because the on-screen report failed to finish loading.

**Fix Result(s):**
1. Export completes after the report loads
2. Exported rows match the on-screen result
3. No errors displayed

## Report Functional Verification

### 1.Existing Preset And Auto Report Still Run
**Prerequisite(s):**
1. A previously saved POS056 preset or auto report exists

**Step(s):**
1. Run the existing preset
2. If auto report is configured, wait for or trigger the scheduled run
3. Open the result

**Reproduction Result(s):**
1. Slow loading also affected preset / auto report generation time.

**Fix Result(s):**
1. Preset and auto report complete successfully
2. Payment data remains correct
3. Generation time is improved versus the slow baseline

## Report Compatibility Verification

### 1.Support Browser Compatibility - Google Chrome, Mozilla Firefox, Microsoft Edge
**Step(s):**
1. Open POS056 in Chrome and run with a representative date range
2. Repeat in Firefox
3. Repeat in Edge

**Fix Result(s):**
1. Report loads successfully in all three browsers
2. Performance improvement is observed in each browser
3. No errors displayed

### 2.Support Data Service Compatibility - MySQL and TiDB
**Step(s):**
1. Run POS056 with a date range of less than 7 days (MySQL)
2. Run POS056 with a date range of more than 7 days (TiDB)

**Reproduction Result(s):**
1. Loading is slow on both short and long ranges, with long range / TiDB especially impacted.

**Fix Result(s):**
1. Report retrieves data correctly from MySQL and TiDB
2. Loading completes in an acceptable time on both ranges
3. Payment details remain accurate

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment

## Appendix

### Acceptance Criteria(from JIRA)
1. POS056 Detail Check Payment loading performance is optimized
2. Payment detail accuracy is unchanged after the optimization
3. Report, export and auto report / preset continue to work
4. Related regression: long date range, multiple outlets, high-volume days, MySQL and TiDB
