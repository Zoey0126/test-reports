# HERO-69777 Bug Fix Test Report - HKJC TMS Custom Header Edit Columns Shows Preference Instead of Table Size Wait For

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Column Name in Custom Header Edit Columns

#### F1.1.Verify Column Name in Edit Columns Matches the Reservation List Header
**Prerequisite(s):**
1. HKJC TMS Operation is available and the user can log in
2. The Custom Header function is accessible
3. Reservation records with table size wait for value exist

**Step(s):**
1. Open HKJC Operation
2. Open Custom Header, then click "Edit Columns", then open the "Current/Calendar" tab
3. Locate the column previously named "Preference"
4. Compare the column name with the reservation list header on Waitlist, Cancelled and All views

**Reproduction Result(s):**
1. Edit Columns shows the column name "Preference"
2. The reservation list header on Waitlist, Cancelled and All views shows "Table Size Wait For"
3. The column name is inconsistent between Edit Columns and the reservation list

**Fix Result(s):**
1. Edit Columns shows the column name "Table Size Wait For"
2. The column name is identical to the reservation list header on Waitlist, Cancelled and All views
3. No column named "Preference" is displayed in Edit Columns on the Current/Calendar tab

#### F1.2.Verify Column Display in the Reservation List After Saving the Header Setting
**Prerequisite(s):**
1. The Custom Header Edit Columns page is accessible
2. Reservation records with table size wait for value exist

**Step(s):**
1. Open Custom Header > Edit Columns > Current/Calendar tab
2. Enable the column "Table Size Wait For" and save the setting
3. Open the reservation list on Current, Calendar, Waitlist, Cancelled and All views
4. Review the header and the value of the column

**Reproduction Result(s):**
1. The column header may not match the column configured in Edit Columns
2. The column configuration is applied inconsistently across the reservation list views

**Fix Result(s):**
1. The column header "Table Size Wait For" is displayed in the reservation list
2. The column value is displayed for each reservation record correctly
3. The saved column configuration is applied consistently on Current, Calendar, Waitlist, Cancelled and All views

#### F1.3.Verify Column Is Not Shown in Search and Member Reservation Lists
**Prerequisite(s):**
1. The column "Table Size Wait For" is enabled in Custom Header Edit Columns
2. The Search reservation list and the Member reservation list are accessible

**Step(s):**
1. Open HKJC Operation
2. Open the Search reservation list and review the column headers
3. Open the Member reservation list and review the column headers
4. Open the Current/Calendar, Waitlist, Cancelled and All reservation lists

**Reproduction Result(s):**
1. The column may be displayed in lists where it is not expected

**Fix Result(s):**
1. The column is not displayed in the Search reservation list
2. The column is not displayed in the Member reservation list
3. The column is still displayed only on Waitlist, Cancelled and All views of the Current/Calendar reservation list

#### F1.4.Verify Column Sorting and Filtering After the Rename
**Prerequisite(s):**
1. The column "Table Size Wait For" is displayed in the reservation list
2. Reservation records contain different table size wait for values

**Step(s):**
1. Open the reservation list on Waitlist or All view
2. Click the "Table Size Wait For" column header to sort the records
3. Apply a search condition related to the table size value
4. Review the sort order and the filtered result

**Reproduction Result(s):**
1. Sorting the renamed column may not work correctly after the wording change
2. The filtered result may not match the entered condition

**Fix Result(s):**
1. The records are sorted by the table size wait for value in ascending and descending order
2. The filtered result matches the entered condition
3. The column name is displayed correctly in the sort menu and the column selection list

#### F1.5.Verify Language Switching
**Prerequisite(s):**
1. The system supports multiple display languages
2. The column "Table Size Wait For" is displayed in the reservation list

**Step(s):**
1. Open Custom Header > Edit Columns > Current/Calendar tab
2. Switch the display language
3. Review the column name in Edit Columns and in the reservation list

**Reproduction Result(s):**
1. The column name may be missing or shown with a different wording in another language

**Fix Result(s):**
1. The column name "Table Size Wait For" is displayed correctly in Edit Columns for the supported languages
2. The reservation list header uses the same wording as Edit Columns

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Custom Header > Edit Columns for all tabs
- Reservation list header display on Current, Calendar, Waitlist, Cancelled and All views
- Column selection and column sequence saving in Custom Header
- Search reservation list and Member reservation list columns
- Sorting and filtering on the reservation list columns
- Multi-language display of Custom Header column names
- Reservation detail display with table size wait for value
