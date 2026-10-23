# HERO-68941 [EY Pentest 2026] Improper Content Security Policy Implementation - Outlet Module Test Report

## Test Environment Information
- Local Test Environment
- HQ Test Environment
- Report-only CSP header enabled for validation
- Enforcing CSP header unchanged (permissive: unsafe-inline / unsafe-eval allowed)
- Admin account with access to outlet module pages (Floor Plan, Shop & Outlet Setup, Calendar & Periods, Configuration)
- Browser DevTools / CSP violation report endpoint available for verification

## Functional Testing

### 1.CSP Infrastructure Verification

#### 1.1.Report-only CSP header is applied to outlet module pages
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed to the test environment.
2. The report-only CSP policy is configured at application level.
3. Browser DevTools is available to inspect response headers.

**Step(s):**
1. Log in to the Infrasys admin backend.
2. Navigate to Floor Plans (listing page) under outlet management.
3. Open DevTools > Network tab and inspect the response headers for the page request.
4. Verify the presence of the report-only CSP header (Content-Security-Policy-Report-Only).
5. Repeat the header inspection on Shops, Outlets, Calendar, and System Configuration pages.

**Test Result(s):**
1. The report-only CSP header is present on all inspected outlet module page responses.
2. The report-only policy includes explicit style-src, excludes strict-dynamic, and keeps unsafe-eval temporarily as designed for the migration phase.
3. The enforcing CSP header (Content-Security-Policy) remains unchanged and permissive (unsafe-inline / unsafe-eval still allowed).

#### 1.2.Nonce is injected on inline script and style tags and matches the response header
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed to the test environment.
2. The report-only CSP header is enabled.
3. Browser DevTools is available to inspect the DOM and response headers.

**Step(s):**
1. Open Floor Plans listing page and inspect the page source with DevTools > Elements tab.
2. Verify every inline `<script>` and `<style>` block in the system layouts (admin, login, translator, full-screen) carries a nonce attribute.
3. Verify the nonce value of each inline block matches the nonce declared in the report-only CSP response header.
4. Repeat the inspection on the Outlets edit page and the Floor Plan Map edit page.
5. Inspect shared admin shell elements (navigation, buttons, dialogs, loading overlays) for nonce-carrying script/style tags.

**Test Result(s):**
1. All inline script and style tags in system module scope include a nonce attribute.
2. Each nonce value matches the nonce in the report-only CSP response header.
3. No inline script or style block in the admin shell is missing the nonce.

#### 1.3.Nonce is generated per request and not cached in session
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed to the test environment.
2. The report-only CSP header is enabled.

**Step(s):**
1. Open the Floor Plans listing page and record the nonce value from the response header and inline tags.
2. Reload the same page (F5) and record the nonce value again.
3. Log out, log back in, and open the same page; record the nonce value again.
4. Open the same page in a second browser session (different user or incognito window) and record the nonce value.
5. Compare all recorded nonce values.

**Test Result(s):**
1. The nonce value changes on every page request (no reuse across reloads).
2. The nonce is not persisted in or restored from the session cache.
3. Different sessions receive different nonce values.
4. No CSP violation related to stale nonce is reported after reload or re-login.

#### 1.4.CSP violation report endpoint collects and logs violations
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed to the test environment.
2. The CSP violation report endpoint is configured with logging enabled.

**Step(s):**
1. Browse the outlet module pages (Floor Plans, Outlets, Calendar, System Configuration) under the report-only policy.
2. Trigger normal page interactions (navigation, tab switching, dialog opening).
3. Check the CSP violation report endpoint and its logs for reported violations.
4. Verify violation entries are reviewable (contain document URI, violated directive, source file).

**Test Result(s):**
1. CSP violation reports are collected by the configured endpoint.
2. Violation entries are reviewable with meaningful details (document URI, violated directive).
3. No violations are reported for the standard outlet management admin flows (see acceptance criteria).

#### 1.5.Enforcing CSP header remains permissive during migration
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed to the test environment.

**Step(s):**
1. Inspect the enforcing CSP response header on outlet module pages.
2. Verify script-src still allows unsafe-inline / unsafe-eval.
3. Confirm that legacy plugin admin pages (out of scope for this ticket) still load correctly under the enforcing header.

**Test Result(s):**
1. The enforcing CSP header is unchanged and remains permissive (unsafe-inline / unsafe-eval allowed).
2. No enforcement-related script blocking occurs on outlet module pages or plugin admin pages.
3. Strict enforcement behaviour is not activated by this change.

### 2.Floor Plan Pages - CSP Compliance and Functionality Regression

#### 2.1.Floor Plans listing / view / edit function correctly under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. At least one floor plan record exists.
3. Browser console is open to observe CSP violations.

**Step(s):**
1. Open Floor Plans listing page; verify the list loads, pagination and search work.
2. Open an existing floor plan in view mode; verify all fields and layout render correctly.
3. Switch the floor plan to edit mode; modify the name and other allowed fields and save.
4. Observe the browser console and the CSP violation report endpoint during all operations.

**Test Result(s):**
1. Floor Plans listing, view, and edit all load and operate correctly.
2. No CSP violations are reported in the browser console for these flows.
3. Changes are saved successfully and reflected in the listing.

#### 2.2.Floor Plan Map edit works with Quick Editor and media picker under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. A floor plan with map data exists; media assets are available for the media picker.

**Step(s):**
1. Open Floor Plan Map edit for an existing floor plan.
2. Use the Quick Editor to modify map elements and save.
3. Open the media picker, select an image, and apply it.
4. Use "Add New table row" to add a new table to the map.
5. Open the table attribute dialog; exercise select all / unselect all / clear row actions.
6. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. Quick Editor operations work correctly and changes are saved.
2. The media picker opens, selects, and applies media without CSP violations.
3. Adding a new table row works as before.
4. The table attribute dialog's select all / unselect all / clear row actions behave as expected.
5. No CSP violations are reported in the browser console for these flows.

#### 2.3.Floor Plan Map view renders media caption layout correctly
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. A floor plan map with media captions exists.

**Step(s):**
1. Open Floor Plan Map view mode for a floor plan with media captions.
2. Verify the map, tables, and media captions render with correct layout.
3. Observe the browser console and CSP violation report endpoint while viewing.

**Test Result(s):**
1. The map view renders correctly including the media caption layout.
2. Inline styles converted to external CSS render identically to the previous design.
3. No CSP violations are reported for the map view.

### 3.Shop & Outlet Setup Pages - CSP Compliance and Functionality Regression

#### 3.1.Shops and Outlets listing / view / edit / copy data work under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. At least one shop and one outlet record exist.

**Step(s):**
1. Open Shops listing; verify listing loads, then open view and edit modes and save a change.
2. Use the "copy data" function from an existing shop.
3. Repeat listing / view / edit operations on the Outlets page.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. Shops and Outlets listing / view / edit all function correctly.
2. Shop copy data completes successfully.
3. No CSP violations are reported in the browser console for these flows.

#### 3.2.Outlet Groups, Sections, Brands, and Locations maintain full CRUD functionality
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. Existing Outlet Groups, Sections, Brands, and Locations records are available.

**Step(s):**
1. For each of Outlet Groups, Sections, Brands, and Locations: open the listing page.
2. Create a new record, edit an existing record, and verify the view mode.
3. Delete a test record created in step 2 where applicable.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. Listing, view, edit, and create operations work correctly for all four setup pages.
2. Buttons, tabs, and dialogs on these pages respond as before.
3. No CSP violations are reported in the browser console for these flows.

### 4.Calendar & Periods Pages - CSP Compliance and Functionality Regression

#### 4.1.Calendar and Calendar Groups listing / view / edit work under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. At least one calendar and one calendar group exist.

**Step(s):**
1. Open Calendar listing; verify the list loads correctly.
2. Open view and edit modes of an existing calendar; modify and save.
3. Repeat the same operations on Calendar Groups.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. Calendar and Calendar Groups listing / view / edit function correctly.
2. Inline scripts (now nonce-based) execute correctly on these pages.
3. No CSP violations are reported in the browser console for these flows.

#### 4.2.Periods listing / view / edit / copy and Period Types / Special Periods maintain functionality
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. Existing periods, period types, and special periods records are available.

**Step(s):**
1. Open Periods listing; exercise listing, view, edit, and the copy function.
2. Open Period Types listing; exercise listing, view, and edit.
3. Open Special Periods listing; exercise listing, view, and edit.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. Periods listing / view / edit / copy all work correctly.
2. Period Types and Special Periods listing / view / edit all work correctly.
3. No CSP violations are reported in the browser console for these flows.

### 5.Configuration Pages - CSP Compliance and Functionality Regression

#### 5.1.Attribute Types, Attribute Options, Common Tables, and Currencies maintain CRUD functionality
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. Existing records are available on each configuration page.

**Step(s):**
1. Open Attribute Types and Attribute Options; exercise listing / view / edit.
2. Open Common Tables; exercise listing / view / edit.
3. Open Currencies; exercise listing / view / edit.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. All four configuration pages load and operate correctly.
2. Save and cancel actions work as before on each page.
3. No CSP violations are reported in the browser console for these flows.

#### 5.2.System Configuration, Shop License Control, and Slave Servers pages function correctly
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. Admin privileges are available to access system-level configuration pages.

**Step(s):**
1. Open System Configuration (listing / edit); modify a test-safe configuration value and save, then revert.
2. Open Shop License Control (listing / edit); verify the listing and edit flows.
3. Open Slave Servers (listing / view / edit); exercise the flows.
4. Observe the browser console and CSP violation report endpoint during all operations.

**Test Result(s):**
1. System Configuration, Shop License Control, and Slave Servers pages all function correctly.
2. Editing and saving behave as before with no script errors.
3. No CSP violations are reported in the browser console for these flows.

#### 5.3.Data Process Logs listing and calendar view render correctly
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. Data process log entries exist in the environment.

**Step(s):**
1. Open Data Process Logs listing view; verify logs load correctly.
2. Switch to the calendar view; verify the calendar renders and navigation works.
3. Observe the browser console and CSP violation report endpoint during both views.

**Test Result(s):**
1. Data Process Logs listing and calendar view both render correctly.
2. No CSP violations are reported in the browser console for these views.

### 6.Inline Script and Style Hardening Verification

#### 6.1.No client-side eval() remains for JSON data embedding in system module views
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. Access to outlet module view source/templates is available (or DevTools Sources panel).

**Step(s):**
1. Open the outlet module pages (Floor Plans, Shops, Outlets, Calendar, System Configuration).
2. In DevTools, search the loaded scripts for eval() usage related to JSON embedding.
3. Verify JSON data embedding now uses JSON.parse() or equivalent safe embedding.
4. Verify no eval-related CSP warnings appear under the report-only policy.

**Test Result(s):**
1. No client-side eval() remains in system module views for JSON data embedding.
2. JSON data is embedded via JSON.parse() or equivalent safe mechanism.
3. No eval-related CSP violations are reported.

#### 6.2.No javascript: URLs remain in system module views and admin shell helpers
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. DevTools is available to inspect the DOM.

**Step(s):**
1. Open the outlet module pages and inspect link elements in the DOM.
2. Search for javascript: and javascript:void(0) link targets in system module views.
3. Verify replaced links use # with behaviour handled by delegated event listeners.
4. Click representative replaced links to confirm the original behaviour is preserved.

**Test Result(s):**
1. No javascript: URLs remain in system module views and helpers used by the admin shell.
2. Replaced links still trigger the intended behaviour via delegated event listeners.
3. No navigation errors or console errors occur from the replaced links.

#### 6.3.No inline HTML event attributes remain and equivalent listeners work
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.

**Step(s):**
1. Open the outlet module pages and inspect the DOM for inline event attributes (onclick, etc.) in system module scope.
2. Click representative buttons, tabs, and rows that previously used inline handlers.
3. Verify string-based setTimeout / setInterval calls (if any) have been replaced with function references in system scope.
4. Observe the browser console during the interactions.

**Test Result(s):**
1. No inline HTML event attributes remain in system module scope.
2. All previously inline-wired interactions work via listeners registered in existing JS files.
3. No CSP violations or script errors are reported during the interactions.

#### 6.4.Static inline CSS moved to external stylesheets and inline style attributes reduced
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.

**Step(s):**
1. Open the outlet module pages and inspect for static layout CSS in inline style blocks.
2. Verify static layout CSS is served from external stylesheets where practical.
3. Compare page rendering with the expected design (layout, colours, spacing).
4. Verify dynamic third-party widget styling still works under the interim style-src policy.

**Test Result(s):**
1. Static layout CSS is loaded from external stylesheets in system module scope.
2. Page rendering is visually consistent with the design (no layout regressions).
3. Dynamic widget styling continues to work under the interim style-src policy allowing unsafe-inline.

### 7.Regression on Shared Admin Shell Elements

#### 7.1.Admin shell (navigation, buttons, dialogs, modals) works with nonce-based scripts
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. The admin shell elements are shared across system and plugin admin pages.

**Step(s):**
1. Navigate across system pages using the admin shell navigation.
2. Open representative dialogs and modals from the shared shell (e.g. confirmation dialogs on Floor Plans edit).
3. Verify loading overlays display and dismiss correctly during page transitions.
4. Observe the browser console and CSP violation report endpoint during all interactions.

**Test Result(s):**
1. Navigation, buttons, dialogs, modals, and loading overlays all function correctly.
2. Shared shell scripts execute correctly with nonce attributes.
3. No CSP violations are reported for the shared shell interactions.

#### 7.2.Shared elements embedded on plugin admin pages still function (lookup config widgets)
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed and report-only CSP is enabled.
2. At least one plugin admin page embedding shared shell elements (navigation, buttons, lookup config widgets) is available.

**Step(s):**
1. Open a plugin admin page that embeds shared navigation and buttons.
2. Open a page with lookup config widgets and exercise the widget interactions.
3. Verify the embedded shared elements respond correctly with nonce-based scripts.
4. Observe the browser console and CSP violation report endpoint during the interactions.

**Test Result(s):**
1. Shared navigation, buttons, and lookup config widgets function correctly on plugin admin pages.
2. No CSP violations are reported for the embedded shared elements.
3. No functional regression is observed on the plugin admin page.

## Compatibility Testing

### 1.Browser Compatibility for CSP Remediation on Platform Admin Pages

#### 1.1.Chrome renders and operates outlet module pages correctly under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. Google Chrome (latest version) is installed.

**Step(s):**
1. Open Floor Plans listing/view/edit and Outlets listing/view/edit in Chrome.
2. Verify nonce attributes on inline script/style tags and the report-only CSP header.
3. Exercise representative interactions (edit and save a floor plan, open a dialog).
4. Check the browser console for CSP violations.

**Test Result(s):**
1. All pages render and function correctly in Chrome.
2. Nonce-based scripts and styles load without violations.
3. No CSP violations appear in the Chrome console for the tested flows.

#### 1.2.Firefox renders and operates outlet module pages correctly under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. Mozilla Firefox (latest version) is installed.

**Step(s):**
1. Open Floor Plans listing/view/edit and Shops listing/view/edit in Firefox.
2. Verify nonce attributes on inline script/style tags and the report-only CSP header.
3. Exercise representative interactions (Floor Plan Map edit, table attribute dialog).
4. Check the browser console for CSP violations.

**Test Result(s):**
1. All pages render and function correctly in Firefox.
2. Nonce-based scripts and styles load without violations.
3. No CSP violations appear in the Firefox console for the tested flows.

#### 1.3.Microsoft Edge renders and operates outlet module pages correctly under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. Microsoft Edge (latest version) is installed.

**Step(s):**
1. Open Calendar and Periods pages (listing/view/edit/copy) in Edge.
2. Verify nonce attributes on inline script/style tags and the report-only CSP header.
3. Exercise representative interactions (edit a period, use the copy function).
4. Check the browser console for CSP violations.

**Test Result(s):**
1. All pages render and function correctly in Edge.
2. Nonce-based scripts and styles load without violations.
3. No CSP violations appear in the Edge console for the tested flows.

#### 1.4.Safari renders and operates outlet module pages correctly under report-only CSP
**Prerequisite(s):**
1. The build containing HERO-68941 is deployed.
2. Safari (latest version on macOS) is available.

**Step(s):**
1. Open Configuration pages (System Configuration, Currencies, Data Process Logs) in Safari.
2. Verify nonce attributes on inline script/style tags and the report-only CSP header.
3. Exercise representative interactions (edit a currency, switch Data Process Logs to calendar view).
4. Check the browser console for CSP violations.

**Test Result(s):**
1. All pages render and function correctly in Safari.
2. Nonce-based scripts and styles load without violations.
3. No CSP violations appear in the Safari console for the tested flows.

## Appendix

### JIRA Issue Information
- **Issue Key**: HERO-68941
- **Summary**: [EY Pentest 2026] Improper Content Security Policy Implementation - Outlet Module
- **Issue Type**: Improvement
- **Component**: CORE
- **Status**: READY FOR QA

### Requirement Description

**Background:**
As part of removing unsafe-inline and unsafe-eval from the Content Security Policy (CSP), the platform is moving to a nonce-based approach for inline scripts and styles. Nonce generation and a report-only CSP header are already in place at the application level. This ticket covers CSP remediation work for outlet module code only.

**Objective:**
Make outlet module pages compliant with the target CSP policy so that inline scripts, unsafe URL schemes, and inline event handlers no longer rely on unsafe-inline in script-src. Use the report-only header to validate fixes before platform-wide enforcement.

### Acceptance Criteria
1. Report-only CSP shows no violations on outlet management admin flows
2. All inline script and style tags in system module scope include a valid nonce matching the response header
3. No client-side eval() remains in system module views for JSON data embedding
4. No javascript: URLs remain in system module views and helpers used by the admin shell
5. No inline HTML event attributes remain in system module scope
6. CSP violation reports are collected and reviewable
7. Enforcing CSP header is unchanged (still permissive) - strict enforcement is a separate platform milestone

### Affected Pages (from Release Notes 1.2.116.0)
| Area | Pages |
| --- | --- |
| Floor Plan | Floor Plans (listing / view / edit), Floor Plan Map edit (Quick Editor, media picker, Add New table row, table attribute dialog), Floor Plan Map view |
| Shop & Outlet Setup | Shops, Outlets, Outlet Groups, Sections, Brands, Locations (listing / view / edit, copy data) |
| Calendar & Periods | Calendar, Calendar Groups, Periods (listing / view / edit / copy), Period Types, Special Periods |
| Configuration | Attribute Types, Attribute Options, Common Tables, Currencies, System Configuration, Shop License Control, Slave Servers, Data Process Logs |

### Out of Scope
- Library-related CSP issues (legacy players, tree widgets, color pickers, editors, PDF viewers)
- strict-dynamic enablement (separate platform milestone)
- Enforcing CSP strict mode activation
