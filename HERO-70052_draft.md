# HERO-70052 Test Report

A user reported that on the Table Fees by Date page, deleting a record deletes the wrong record. With the same period type, the delete action only deleted the latest date of the same period type instead of the selected record.

## Functional Testing

### 1. Deleting a Row That Is Not the Latest Date Deletes Another Row

**Prerequisite(s):**
1. A user with access to the admin page is available.
2. Table Fees by Date contains at least two records with the same period type: 2026-11-18 Lunch and 2026-11-30 Lunch.
3. The records of both rows are recorded before the test.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Locate the record with date 2026-11-18 and period type Lunch, which is not the latest date with the same period type.
3. Click delete on that row and confirm the deletion.
4. Observe the rows remaining in the listing.

**Reproduction Result(s):**
1. The row with date 2026-11-30, which is the latest date of the same period type, is removed.
2. The selected row with date 2026-11-18 remains in the listing.
3. The deleted record does not match the record that the user selected.

**Fix Result(s):**
1. The selected row with date 2026-11-18 is deleted.
2. The row with date 2026-11-30 remains in the listing.
3. The delete action removes the exact record that the user selected.

### 2. Deletion of the Selected Table Fees by Date Record

#### 2.1. Row That Is Not the Latest Date with the Same Period Type Is Deleted

**Prerequisite(s):**
1. Table Fees by Date contains records for 2026-11-18 Lunch and 2026-11-30 Lunch.
2. The user is logged in to the admin page.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Click delete on the 2026-11-18 Lunch row and confirm.
3. Refresh the listing and check the remaining rows.

**Reproduction Result(s):**
1. Before the fix, the delete action removes the 2026-11-30 Lunch row because it is the latest date of the same period type.
2. The selected 2026-11-18 Lunch row stays in the listing.

**Fix Result(s):**
1. The 2026-11-18 Lunch row is deleted.
2. The 2026-11-30 Lunch row stays unchanged.
3. The listing after refresh shows only the removal of the selected row.

#### 2.2. Latest Row with the Same Period Type Is Deleted

**Prerequisite(s):**
1. Table Fees by Date contains records for 2026-11-18 Lunch and 2026-11-30 Lunch.
2. The user is logged in to the admin page.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Click delete on the 2026-11-30 Lunch row, which is the latest date of the same period type, and confirm.
3. Refresh the listing and check the remaining rows.

**Reproduction Result(s):**
1. Before the fix, the delete action removes the latest date of the same period type, which happens to match the selected row in this scenario.
2. The result hides the wrong-row behavior because the selected row is the latest date.

**Fix Result(s):**
1. The 2026-11-30 Lunch row is deleted.
2. The 2026-11-18 Lunch row stays unchanged.
3. The delete action still succeeds for a selected row that is the latest date of the same period type.

#### 2.3. Row with a Different Period Type Is Deleted

**Prerequisite(s):**
1. Table Fees by Date contains records with different period types, for example 2026-11-18 Lunch and 2026-11-18 Dinner.
2. The user is logged in to the admin page.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Click delete on the 2026-11-18 Lunch row while a 2026-11-18 Dinner row exists.
3. Confirm the deletion and refresh the listing.

**Reproduction Result(s):**
1. Before the fix, the delete action does not target the selected record and only deletes the latest date of the same period type.
2. The remaining rows do not reflect the selection made by the user.

**Fix Result(s):**
1. The 2026-11-18 Lunch row is deleted.
2. The 2026-11-18 Dinner row stays unchanged.
3. Only the selected record is removed regardless of the period type of the other records.

### 3. Data Integrity After Deletion

#### 3.1. Remaining Rows and Values Are Unchanged After Deletion

**Prerequisite(s):**
1. Table Fees by Date contains several records with different dates and period types.
2. The dates, period types and fee amounts of all rows are recorded before the test.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date and record all rows.
2. Delete one selected row and confirm.
3. Refresh the listing and compare the remaining rows with the recorded values.

**Reproduction Result(s):**
1. Before the fix, a row other than the selected row is removed, so the remaining rows do not match the recorded values minus the selected record.
2. The record of the latest date of the same period type disappears although it was not selected.

**Fix Result(s):**
1. Only the selected row is removed from the listing.
2. All other rows keep their dates, period types and fee amounts.
3. The listing count matches the recorded count minus one.

#### 3.2. Sequential Deletions Remove Only the Selected Rows

**Prerequisite(s):**
1. Table Fees by Date contains at least three records with the same period type on different dates.
2. The user is logged in to the admin page.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Delete the earliest record of the same period type and confirm.
3. Without leaving the page, delete another record that is not the latest date and confirm.
4. Refresh the listing and check the remaining rows.

**Reproduction Result(s):**
1. Before the fix, each delete action removes the latest date of the same period type instead of the selected record.
2. After two deletions, the remaining rows do not match the two rows that the user selected.

**Fix Result(s):**
1. The first delete action removes only the first selected record.
2. The second delete action removes only the second selected record.
3. The remaining rows match the original list minus the two selected records.

### 4. Table Fees by Date Listing Before Deletion

**Prerequisite(s):**
1. Table Fees by Date contains records with several dates and period types.
2. The user is logged in to the admin page.

**Step(s):**
1. Go to the admin page > TMS > Basic Setup > Table Fees by Date.
2. Check the displayed dates, period types and fee amounts of the listed rows.
3. Note the latest date of each period type.

**Reproduction Result(s):**
1. Before the fix, the listing displays all records correctly, but the delete action addresses the latest date of the same period type instead of the selected row.
2. The listing itself is not affected by the defect.

**Fix Result(s):**
1. The listing displays all records with their dates, period types and fee amounts.
2. The latest date of each period type is identified correctly from the listing.
3. The listing is unchanged by the delete fix.

## Test Environment

Testing is performed on HKJC TMS on Infrasys Cloud with the admin page, TMS > Basic Setup > Table Fees by Date page available to the logged-in user. The environment contains Table Fees by Date records with several dates and period types, including 2026-11-18 Lunch and 2026-11-30 Lunch, whose dates, period types and fee amounts are recorded before the test.
