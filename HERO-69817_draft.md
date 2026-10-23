# HERO-69817 Bug Fix Test Report - SSDLC Dependency Checking Fix for moment.js and moment.min.js Upgrade to 2.31+

## Functional Testing

### F1.Bug Reproduction and Fix Verification - moment.js Version Upgrade

#### F1.1.Verify moment.min.js Version Is 2.31.0 on Affected Pages
**Prerequisite(s):**
1. Infrasys POS Backend is deployed with the fixed build
2. The login account has the authority to access the affected pages
3. The browser developer tool is available

**Step(s):**
1. Log in to Infrasys POS Backend
2. Open an affected page with the calendar view, for example System Management > Data Synchronization > Data Release Log > Calendar View
3. Right-click on the page and select "Inspect"
4. Open the [Network] tab and refresh the page
5. Find moment.min.js in the network request list and check its version

**Reproduction Result(s):**
1. The loaded moment.min.js is a version lower than 2.31.0
2. The dependency checking report flags the outdated moment.js and moment.min.js

**Fix Result(s):**
1. The loaded moment.min.js is version 2.31.0
2. Both moment.js and moment.min.js are upgraded to version 2.31.0 on the affected pages
3. No outdated moment dependency remains on the affected pages

#### F1.2.Verify Calendar View Navigation Works After the Upgrade
**Prerequisite(s):**
1. The affected page contains reservation or log records across multiple months
2. The browser developer tool console is open

**Step(s):**
1. Open an affected page with the calendar view
2. Click the "<" button to go to the previous month
3. Click the ">" button to go to the next month
4. Click the "Today" button to return to the current month
5. Review the [Console] tab for errors

**Reproduction Result(s):**
1. The calendar navigation may fail or display an incorrect month after the dependency mismatch
2. JavaScript errors appear in the console when changing the month

**Fix Result(s):**
1. The "<", ">" and "Today" buttons change the month correctly
2. The calendar grid and the record markers are displayed for the correct month
3. No JavaScript error appears in the console

#### F1.3.Verify Record Selection and Detail View in Calendar View
**Prerequisite(s):**
1. At least one record exists in the target month of the affected page

**Step(s):**
1. Open an affected page with the calendar view
2. Select a record marker in the calendar
3. Open the record detail
4. Return to the calendar and search for records by condition
5. Change the display language

**Reproduction Result(s):**
1. The record detail may not open or the record data may be displayed incorrectly
2. Searching by condition and changing the language may raise a JavaScript error

**Fix Result(s):**
1. The record detail opens with the correct record information
2. Searching for records by condition returns the correct result
3. Changing the display language re-renders the calendar and the date labels correctly
4. No JavaScript error appears in the console during the above operations

#### F1.4.Verify All Affected Pages With Calendar View
**Prerequisite(s):**
1. Data exists for each affected page so the calendar view can be rendered
2. The browser developer tool is available

**Step(s):**
1. Open each affected page listed below and switch to the calendar view
2. Repeat the month navigation, record selection and language change on each page
3. Check the [Console] tab for each page
4. Check the moment.min.js version in the [Network] tab for each page

**Test Result(s):**
1. Every affected page loads moment.min.js version 2.31.0
2. The calendar view renders correctly on every affected page
3. No JavaScript error appears in the console on any affected page

| No. | Affected Page |
| --- | --- |
| 1 | System Management > Data Synchronization > Data Release Log > Calendar View |
| 2 | Audit Log Management > Purge Log History > Calendar View |
| 3 | HKJC Management > AD Group > AD Groups Sync Log > Calendar View |
| 4 | Member Management > Data Process Log > Calendar View |
| 5 | Outlet Management > Outlet Location > Copy Data Log > Calendar View |
| 6 | PII Management > Data Process Log > Calendar View |
| 7 | POS System > Data In > Data Upload Request |
| 8 | POS System > Data Out > Data Loss Report Log > Calendar View |
| 9 | SDP Management > Data Management > Data Purge Log > Calendar View |
| 10 | Table Management System > Data Management > Data Process Log > Calendar View |

#### F1.5.Verify Calendar Data Consistency After the Upgrade
**Prerequisite(s):**
1. Records exist in multiple months on the affected pages
2. The expected record count per month is known

**Step(s):**
1. Open an affected page with the calendar view
2. Compare the record marker count of a month with the expected record count
3. Open the list view and compare the records with the calendar view records
4. Switch to a month without any record

**Reproduction Result(s):**
1. The calendar may show an incorrect date offset for a record after the version mismatch
2. The record marker count does not match the list view records

**Fix Result(s):**
1. The calendar shows each record on the correct date
2. The record marker count matches the list view records
3. A month without records shows an empty calendar without error

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- All ten affected pages listed above and their calendar view rendering
- Date range search on the affected pages
- Language switching on the affected pages
- Other pages that load moment.js, such as dashboard and scheduling pages
- Browser console error monitoring on the affected pages
- Backend data synchronization, audit log purge and data process log functions
