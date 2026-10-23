# HERO-69866 Bug Fix Test Report - TMS 2.0 Search View Page "Next" Button Does Not Work

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Search View Pagination

#### F1.1.Verify "Next" Button Navigates to the Next Page in Search View
**Prerequisite(s):**
1. TMS Operation is available and the user can log in to outlet Elvar-Hong Kong (E0416)
2. Multiple pages of reservation data exist for 2026-10-13

**Step(s):**
1. Log in to TMS Operation > Elvar-Hong Kong (E0416) > Search View
2. Set the reservation date to 2026-10-13 and click the "Search" button
3. Confirm that the result contains more than one page of reservation records
4. Click the "Next" button

**Reproduction Result(s):**
1. After clicking the "Next" button, the page does not navigate to the next page
2. The reservation list keeps displaying the records of the first page
3. No error message is shown to the user

**Fix Result(s):**
1. After clicking the "Next" button, the page navigates to the next page normally
2. The reservation list displays the reservation records of the next page
3. The page number indicator reflects the current page correctly

#### F1.2.Verify "Previous" Button Navigates Back to the Previous Page
**Prerequisite(s):**
1. The Search View result contains at least three pages of reservation records

**Step(s):**
1. Log in to TMS Operation > Search View and search for reservation records
2. Click the "Next" button twice to reach the third page
3. Click the "Previous" button once

**Reproduction Result(s):**
1. The "Previous" button has the same pagination issue while moving between pages
2. The page number is not updated after the navigation

**Fix Result(s):**
1. The "Previous" button navigates back to the second page correctly
2. The reservation list and the page number indicator are updated correctly

#### F1.3.Verify Pagination With Different Page Sizes
**Prerequisite(s):**
1. More than 100 reservation records exist for the target date
2. The page size can be changed in the Search View

**Step(s):**
1. Log in to TMS Operation > Search View and search for the reservation records
2. Set the page size to the minimum value and click "Next" through several pages
3. Change the page size to a larger value and click "Next" again
4. Return to the first page

**Reproduction Result(s):**
1. The "Next" button does not navigate correctly for any page size
2. The record range displayed does not match the selected page size

**Fix Result(s):**
1. The "Next" button navigates correctly for every page size
2. The record range displayed matches the selected page size
3. Returning to the first page displays the first records of the result set

#### F1.4.Verify Pagination With Search Filters Applied
**Prerequisite(s):**
1. Reservation records with different statuses, outlets and members exist for the target date

**Step(s):**
1. Log in to TMS Operation > Search View
2. Apply search filters such as status, outlet and member name
3. Click the "Search" button
4. Click the "Next" button and then the "Previous" button
5. Change one search filter and search again

**Reproduction Result(s):**
1. The "Next" button does not navigate after the filters are applied
2. The page index is not reset when the search criteria change

**Fix Result(s):**
1. The "Next" and "Previous" buttons navigate correctly with all filters applied
2. The page index is reset to the first page when the search criteria change
3. The filtered result set is paginated correctly

#### F1.5.Verify Pagination When the Result Contains a Single Page
**Prerequisite(s):**
1. A search criterion returns only one page of reservation records

**Step(s):**
1. Log in to TMS Operation > Search View
2. Apply a criterion that returns fewer records than the page size
3. Click the "Search" button
4. Observe the pagination controls

**Reproduction Result(s):**
1. The pagination controls are still displayed when only one page exists
2. Clicking "Next" does not respond

**Fix Result(s):**
1. The pagination controls are disabled or correctly reflect that only one page exists
2. No unexpected response or error occurs when the "Next" button is clicked
3. The single page of reservation records is displayed correctly

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- TMS Operation > Search View reservation list display
- Search View filters such as date, status, outlet and member
- Search View sorting on every displayed column
- Reservation list pagination in Current, Waitlist, Cancelled and All views
- Pagination in Third Party Payment List and Pending Reservation List
- Reservation detail opening from the paginated list
- Search View behaviour after a reservation is created, modified or cancelled
