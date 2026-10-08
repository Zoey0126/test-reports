# Report Console - Report Viewer First-Load Performance Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Report Console - Report Viewer First-Load Performance |
| Code | N/A (Report Console / Report Viewer entry path: report/reports/view) |
| Issue Key | HERO-68473 |
| Issue Type | Bug |
| Status | READY FOR QA |
| Summary | Opening a report in Report Viewer is very slow on the first load for non-admin HQ users with many outlets |
| Components | REPORTS |
| Customer | Hyatt HQ (~2800 outlets) |
| Related Tickets | HERO-63472 (Report Console listing in-memory caching), HERO-62187 (Optimize report list loading), HERO-50240 (Optimize view report / Set Report Authorities loading) |

## Bug Summary

When a non-admin HQ user with a large outlet set opens a report from Report Console for the first time (report authority cache is empty for that user and report), Infrasys Cloud builds per-outlet report authority cache before the Report Viewer page returns. On a large HQ with about 2800 outlets, Waiting (TTFB) for the view page was about 2.7 minutes. Opening the same report again (cache already built) was about 1.57 seconds.

The wait is on the view page itself, before the BIRT report engine runs, and is not caused by the report SQL. The fix optimizes `genReportAclCacheByReport` (used when opening Report Viewer and aligned module copies) so the first load returns in a few seconds instead of minutes while still writing the full per-outlet cache rows required by the BIRT report outlet list.

### Preconditions (Important)

- Reproduce with a non-admin user (User Role is not Administrator / user_role != a).
- The user must see a large outlet set (e.g. HQ / multi-property, hundreds to thousands of outlets). Hyatt HQ evidence ~2800 outlets.
- Report authority cache for that user + report must be cold (no rows yet, or those rows cleared).
- Do NOT use an Administrator account to reproduce the multi-minute delay — admin skips the heavy per-outlet authority evaluation, so the extreme first-load wait will not match customer evidence.

### Fix Direction (Implemented)

- Batch-load existing ACL cache for (user, report); return early when all outlets are already cached.
- Reuse listing-style memoized ACL checks for non-admin; admin fast-path allow-all.
- Chunk bulk inserts (e.g. 500).
- Still write full per-outlet cache before BIRT. Do NOT only cache selected shops (missing rows = no access in OutletList).

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| User Role | User role used to open the report (single-select). Administrator role is not the target reproduce path. | Administrator, Non-Administrator | Non-Administrator |
| Outlet Set Size | Outlet visibility scope of the user (single-select). Large HQ = hundreds to thousands of outlets. | Single Outlet, Small Set, Large HQ | Large HQ |
| Authority Cache State | Report authority cache state for the (user, report) pair (single-select) | Cold (Empty), Warm (Already Built) | Cold (Empty) |
| Target Report | Any report opened from Report Console via report/reports/view | Any | Any |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| View Page TTFB | Waiting (TTFB) of the reports/view document request measured in browser DevTools Network | Wall-clock duration from request send to first byte of the view response |
| Authority Cache Rows | Number of per-outlet report authority cache rows written for the (user, report) pair | Count of cache rows = number of outlets visible to the user for that report |
| Outlet List (BIRT) | Outlet list used by the BIRT report after the cache is built | Derived from per-outlet cache rows; missing rows are treated as no access |
| Cache Hit Ratio | Ratio of cache hits vs cache misses during the view page authority evaluation | Hits / (Hits + Misses) |

## Test Verification

### 1. Report Viewer First-Load Performance Verification

#### 1.1. All Default Parameters - Non-Admin HQ User, Cold Cache, Large Outlet Set

**Prerequisite(s):**
  1. A test HQ environment with a non-admin user that can see a large outlet set (e.g. Hyatt HQ ~2800 outlets, or an equivalent large HQ test environment).
  2. Report authority cache for the (user, target report) pair is cleared (cold cache) before the test.
  3. Browser DevTools Network panel is open to capture the reports/view request.
  4. Note the baseline TTFB observed before the fix (≈2.7 minutes for ~2800 outlets) for comparison.
**Step(s):**
  1. Log in as the non-admin HQ user with a large outlet set.
  2. From Report Console, open any report (URL path report/reports/view/...).
  3. In browser DevTools Network, capture Waiting / TTFB for the reports/view document request.
  4. Verify the view page returns and the report renders.
  5. After the page returns, verify the per-outlet report authority cache rows have been written for the (user, report) pair.
**Reproduction Result(s):**
  1. First open (non-admin, cold cache, ~2800 outlets): Waiting (TTFB) ≈ 2.7 minutes on reports/view.
  2. The page appears frozen during the wait; the BIRT report engine does not run until the cache build completes.
  3. Per-outlet ACL cache is built per outlet on miss, evaluating report ACL with multiple SQL queries per outlet (no request-level memoization).
  4. Bulk inserts of ~N cache rows occur after the per-outlet evaluation.
**Fix Result(s):**
  1. First open (non-admin, cold cache, ~2800 outlets) returns within a few seconds instead of ≈2.7 minutes.
  2. The view page returns quickly; the BIRT report engine can proceed.
  3. Existing ACL cache rows are batch-loaded up front; the path returns early when all outlets are already cached.
  4. Listing-style memoized ACL checks are reused for non-admin; admin fast-path allow-all is preserved.
  5. Bulk inserts are chunked (e.g. 500) and full per-outlet cache rows are written before BIRT runs.
  6. No errors displayed.

#### 1.2. Warm Cache Path - Non-Admin HQ User, Warm Cache, Large Outlet Set

**Prerequisite(s):**
  1. A test HQ environment with a non-admin user that can see a large outlet set (e.g. Hyatt HQ ~2800 outlets).
  2. Open the target report once first to build the (user, report) authority cache (warm path baseline ≈1.57 seconds).
  3. Do not clear the cache between the first and second open.
  4. Browser DevTools Network panel is open to capture the reports/view request.
**Step(s):**
  1. Log in as the same non-admin HQ user.
  2. From Report Console, open the same report again (cache already built).
  3. In browser DevTools Network, capture Waiting / TTFB for the reports/view document request.
  4. Verify the view page returns and the report renders with the same outlet list as the cold path.
**Reproduction Result(s):**
  1. Second open (cache already built) returns in ≈1.57 seconds (already acceptable, but path is shared with the cold path code).
  2. The view page returns quickly because all outlets are already cached.
**Fix Result(s):**
  1. Warm path remains fast and at or below the previous warm-path level (~1.57 seconds).
  2. Batch-load returns early when all outlets are already cached, so warm path does not re-evaluate ACL per outlet.
  3. The outlet list used by the report is unchanged vs the cold path.
  4. No errors displayed.

#### 1.3. Non-Admin User With a Small Outlet Set - Cold Cache

**Prerequisite(s):**
  1. A non-admin user with a small outlet set (single property or a few outlets) is configured.
  2. Report authority cache for the (user, target report) pair is cleared (cold cache) before the test.
  3. Browser DevTools Network panel is open to capture the reports/view request.
**Step(s):**
  1. Log in as the non-admin user with a small outlet set.
  2. From Report Console, open any report (URL path report/reports/view/...).
  3. In browser DevTools Network, capture Waiting / TTFB for the reports/view document request.
  4. Verify the view page returns and the per-outlet cache rows are written for the small outlet set.
**Reproduction Result(s):**
  1. First open for a small outlet set is fast enough to mask the issue; the multi-minute delay is only visible on large HQ outlet sets.
**Fix Result(s):**
  1. First open returns quickly (no regression for small outlet sets).
  2. Per-outlet cache rows are written for the small outlet set.
  3. No errors displayed.

#### 1.4. Administrator User - Cold Cache (Negative Test)

**Prerequisite(s):**
  1. An Administrator user (user_role = a) is configured.
  2. Report authority cache for the (admin, target report) pair is cleared (cold cache) before the test.
  3. Browser DevTools Network panel is open to capture the reports/view request.
  4. Confirm admin does NOT reproduce the multi-minute delay (admin skips heavy per-outlet authority evaluation).
**Step(s):**
  1. Log in as the Administrator user.
  2. From Report Console, open any report (URL path report/reports/view/...).
  3. In browser DevTools Network, capture Waiting / TTFB for the reports/view document request.
  4. Verify the admin path uses the fast-path allow-all and does not run the heavy per-outlet authority evaluation.
**Reproduction Result(s):**
  1. Admin first open returns quickly even before the fix (admin skips the heavy per-outlet authority evaluation), so the extreme wait is not reproducible for admin.
**Fix Result(s):**
  1. Admin path continues to use the fast-path allow-all and returns quickly.
  2. No regression is introduced for admin users.
  3. No errors displayed.

### 2. Cache Correctness Verification

#### 2.1. Full Per-Outlet Cache Written Before BIRT Runs

**Prerequisite(s):**
  1. A non-admin HQ user with a large outlet set (e.g. ~2800 outlets) is configured.
  2. Report authority cache for the (user, target report) pair is cleared (cold cache) before the test.
  3. Capture the expected list of outlets visible to the user for the target report.
**Step(s):**
  1. Log in as the non-admin HQ user.
  2. From Report Console, open the target report.
  3. Wait for the view page to return.
  4. Inspect the report authority cache table for the (user, report) pair.
  5. Verify cache rows exist for every outlet visible to the user for that report.
  6. Verify the BIRT OutletList matches the list of cached outlets (missing rows still treated as no access).
**Reproduction Result(s):**
  1. The cold path eventually writes all per-outlet cache rows after the multi-minute wait, but the wait blocks the BIRT engine.
  2. If only selected shops were cached, BIRT OutletList would incorrectly treat uncached outlets as no access.
**Fix Result(s):**
  1. The full per-outlet cache is written before BIRT runs (no shortcuts that only cache selected shops).
  2. BIRT OutletList matches the list of cached outlets; missing rows are still treated as no access.
  3. No errors displayed.

#### 2.2. Cache Hit vs Miss Behavior - Cold Path Returns Early When All Outlets Already Cached

**Prerequisite(s):**
  1. A non-admin HQ user with a large outlet set (e.g. ~2800 outlets) is configured.
  2. Report authority cache for the (user, target report) pair is partially populated (some outlets cached, some not).
**Step(s):**
  1. Log in as the non-admin HQ user.
  2. From Report Console, open the target report.
  3. In browser DevTools Network, capture Waiting / TTFB for the reports/view document request.
  4. Verify the batch-load path returns early when all outlets are already cached (warm path).
  5. For the partially cached state, verify only the missing outlets are evaluated and the rest are reused.
**Reproduction Result(s):**
  1. Without batch-load, every outlet is re-evaluated on each open even if some are already cached.
  2. Partial cache state still triggers per-outlet SQL evaluation for already-cached outlets.
**Fix Result(s):**
  1. Batch-load up front returns early when all outlets are already cached.
  2. Partial cache state only evaluates missing outlets; cached outlets are reused.
  3. No errors displayed.

#### 2.3. Report Access Rules and Outlet List Unchanged

**Prerequisite(s):**
  1. A non-admin HQ user with a large outlet set is configured.
  2. Capture the expected outlet list and access rules for the target report before applying the fix.
  3. Clear the (user, report) authority cache before each run.
**Step(s):**
  1. Open the target report before the fix and capture the outlet list shown in the BIRT report.
  2. Apply the fix and clear the cache.
  3. Open the target report again and capture the outlet list.
  4. Compare the two outlet lists and verify they match exactly.
  5. Verify report access rules (allow/deny per outlet) are unchanged.
**Reproduction Result(s):**
  1. Before the fix, the outlet list and access rules are correct but slow to compute.
**Fix Result(s):**
  1. The outlet list and access rules are unchanged after the optimization.
  2. Missing cache rows are still treated as no access for the outlet list.
  3. No errors displayed.

### 3. Regression Scope

- All reports opened from Report Console via report/reports/view for non-admin HQ users with large outlet sets (target reproduce path).
- Warm path (cache already built) for the same reports and users to confirm no regression vs the previous warm-path level.
- Administrator users opening reports via report/reports/view to confirm the fast-path allow-all is preserved.
- Related listing optimization tickets: HERO-63472 (Report Console listing in-memory caching), HERO-62187 (Optimize report list loading), HERO-50240 (Optimize view report / Set Report Authorities loading).
- BIRT report outlet list correctness: confirm full per-outlet cache is still written before BIRT runs and missing rows are still treated as no access.
- Multiple target reports (different report codes) to confirm the optimization is generic across reports, not specific to one report.
- Bulk insert chunking (e.g. 500) behavior under large HQ outlet sets to confirm inserts complete without timeouts.

## Test Environment

- **Version**: Sprint build containing the HERO-68473 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (Hyatt HQ ~2800 outlets or equivalent large HQ test environment), ER Test Environment

## QC Information

- **Tester**: TBD
- **Reviewer**: TBD
