# HERO-66233 TMS2.0 Reservation List Print View Bug Fix Verification Report

## Functional Testing

### 1. Reservation List Print View Column Configuration Cache Bug

#### 1.1 Verify Deleted Column Should Not Be Displayed in Print Preview

**Prerequisite(s):**
1. Multiple reservations are created on the test date (e.g., 2026-06-24).
2. User has access to Admin > Table Management System and TMS2.0 Operation.
3. The outlet config currently includes the "Table No." column in Reservation List Headers.

**Step(s):**
1. Go to Admin > Table Management System > Other Settings > Config by Location > Reservation List's Headers on Current / Calendar View.
2. Select the target outlet and add "Table No." column to the outlet config.
3. Login TMS2.0 Operation > Current View, click the print icon on the Reservation List.
4. In the Print Reservation List Dialog, select to show "Table No." and click the [Print] button.
5. Verify "Table No." appears on the printed output.
6. Return to Admin > Table Management System > Other Settings > Config by Location > Reservation List's Headers.
7. Delete "Table No." column from the outlet config.
8. Logout TMS2.0 Operation and login again.
9. Go to Calendar View > Pick a date, click the print icon on the Reservation List.
10. Observe whether the print preview still shows the "Table No." column.

**Reproduction Result(s) (Bug Present):**
1. In step 10, the print dialog still displays "Table No." as a selectable option.
2. After selecting and printing, the "Table No." column appears in the print preview, even though the column has been removed from the header configuration.
3. The delete operation did not clear the cached print header settings — stale configuration was persisted across logouts.

**Fix Result(s) (Expected After Fix):**
1. In step 10, the Print Reservation List Dialog no longer includes "Table No." among the selectable columns, since it has been removed from the outlet config.
2. The print preview correctly reflects only the currently configured headers.
3. Cached print data is cleared upon logout/login so the default fields are displayed.
4. Removing a column from the Reservation List Headers also removes it from the print header options.

### 2. Reservation List Sort Order Consistency Between Dialog and Print Preview

#### 2.1 Verify Column Sort Order is Preserved After Printing

**Prerequisite(s):**
1. Multiple reservations exist on the test date (e.g., 2026-06-26).
2. Two of the reservations share the same table number (e.g., Booking A on table 120; Booking B on tables 120 and 122).
3. Outlet config includes "Table No." column in Reservation List Headers.

**Step(s):**
1. Login TMS2.0 Operation > Calendar View > select 2026-06-26.
2. Click the print icon on the Reservation List.
3. In the Print Reservation List Dialog, select to show "Table No.".
4. Click the "Table No." column header twice to sort the list in descending order.
5. Click the [Print] button.
6. Compare the row order shown in step 4 (dialog) with the row order shown in step 6 (print preview).

**Reproduction Result(s) (Bug Present):**
1. The Reservation List rows in the Print Reservation List Dialog follow the descending "Table No." order (table 122 rows appear before table 120 rows, rows with the same table number ordered consistently).
2. After clicking [Print], the print preview rows do **not** preserve this descending order — the sort is either ascending or unsorted.
3. The print preview re-queried or re-rendered data without applying the selected column sort from the dialog.

**Fix Result(s) (Expected After Fix):**
1. The Reservation List sort order selected in the Print Reservation List Dialog is preserved exactly in the print preview.
2. Descending/ascending toggle state of each column is sent along with the print request.
3. Rows sharing the same table number maintain a stable relative order across dialog and print preview.

### 3. Print Preview Column Auto-Shrinking for Many Headers

#### 3.1 Verify Column Width Auto-Adjustment When Many Headers Are Selected

**Prerequisite(s):**
1. Outlet config includes more than 10 headers in Reservation List.
2. Reservation List Print Dialog is accessible.

**Step(s):**
1. Login TMS2.0 Operation, open the Print Reservation List Dialog.
2. Select approximately 12 headers to be displayed.
3. Click the [Print] button.
4. Observe whether the print preview automatically shrinks column widths so more columns fit on the page.

**Reproduction Result(s) (Bug Present):**
1. When many headers are selected, columns retain their default widths and exceed the paper width, causing some columns to be cut off or overflow.
2. Horizontal scrolling is required in the print preview to see all selected columns.

**Fix Result(s) (Expected After Fix):**
1. The print preview automatically shrinks column widths to display as many selected headers as possible within the paper size.
2. If the number of selected headers still exceeds the printable threshold for the chosen paper size, a logical subset is displayed without horizontal overflow.

### 4. Regression Test: Reservation Status Field Not Accidentally Removed

#### 4.1 Verify Reservation Status Persists in Print When Hidden From Main View Headers

**Prerequisite(s):**
1. Outlet config excludes "Reservation Status" from the main Current / Calendar View headers.
2. At least one reservation exists with a non-default status.

**Step(s):**
1. Confirm "Reservation Status" is NOT in the outlet's Current / Calendar View header config.
2. Login TMS2.0 Operation > Current View.
3. Click the print icon on the Reservation List.
4. Observe the print preview for the presence of the Reservation Status column/indicator.

**Fix Result(s) (Expected After Fix):**
1. The print preview still includes the Reservation Status information even when the field is not present in the normal view headers.
2. Reservation Status is treated as a system-critical field that should not be stripped by the header-removal fix.

### 5. Edge Case: Cached Print Data Cleared After Logout/Login

#### 5.1 Verify Logout/Login Resets Print Column Cache

**Prerequisite(s):**
1. A specific column set has been selected in a previous print session.
2. A configuration change (adding or removing columns) has happened since.

**Step(s):**
1. Configure outlet headers to include "VIP Flag".
2. Login TMS2.0 and open Print Reservation List Dialog — select "VIP Flag" and print.
3. Return to Admin and remove "VIP Flag" from outlet headers.
4. Logout TMS2.0 Operation.
5. Login again and open Print Reservation List Dialog.

**Fix Result(s) (Expected After Fix):**
1. In step 5, "VIP Flag" no longer appears among the selectable print headers.
2. The Print Reservation List Dialog starts with the current default field set, not the stale selection from step 2.
3. Newly added columns correctly appear in the print header options after logout/login.

## Test Environment Information

| Environment | Details |
|---|---|
| Backend | TMS2.0 Reservation Module |
| Test Environment | Local test / Outlet config |
| Browser (Platform) | Chrome (latest stable) |
| Test Date | 2026-06-24 ~ 2026-06-26 |

## Appendix

### Requirement Source
- **JIRA Issue**: HERO-66233
- **Type**: Bug
- **Module**: TMS2.0 Reservation
- **Related Components**: Print, Outlet Config by Location, Reservation List Headers
- **Release Notes Key Changes**:
  - Clear cached print data after logout/login so that newly configured default fields are displayed.
  - Reservation List sort order before and after printing must be consistent.
  - Print preview automatically shrinks to fit more selected columns within the paper size.
  - Reservation Status field must not be removed by main-view header changes.
