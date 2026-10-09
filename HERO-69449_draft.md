# HERO-69449 Bug Fix Test Report - Spelling Mistake on Item Department and Item Category Page

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Selectable Column Label Spelling

#### F1.1.Verify "Selectable" Column Label Spelling on Item Departments Page

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. At least one item department is configured in the system

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Departments
2. View the Item Departments listing page
3. Locate the "Selectable" column in the table
4. Verify the column header label spelling and translation

**Reproduction Result(s):**
1. The label on the "Selectable" column is mis-spelt as "not_selecatble"
2. The label does not display correct spelling or proper translation

**Fix Result(s):**
1. The label on the "Selectable" column is correctly spelt as "Selectable" (or properly translated)
2. The label displays with correct spelling and proper translation
3. The column header is no longer showing "not_selecatble"

#### F1.2.Verify "Selectable" Column Label Spelling on Item Categories Page

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. At least one item category is configured in the system

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Categories
2. View the Item Categories listing page
3. Locate the "Selectable" column in the table
4. Verify the column header label spelling and translation

**Reproduction Result(s):**
1. The label on the "Selectable" column is mis-spelt as "not_selecatble"
2. The label does not display correct spelling or proper translation

**Fix Result(s):**
1. The label on the "Selectable" column is correctly spelt as "Selectable" (or properly translated)
2. The label displays with correct spelling and proper translation
3. The column header is no longer showing "not_selecatble"

#### F1.3.Verify "Selectable" Label Translation in Multiple Languages

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. Multiple language packs are installed (e.g., English, Simplified Chinese, Traditional Chinese)
4. At least one item department and one item category are configured

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Departments
2. Switch the system/user language to English
3. Verify the "Selectable" column label displays correctly in English
4. Switch the system/user language to Simplified Chinese
5. Verify the "Selectable" column label displays with correct Simplified Chinese translation
6. Switch the system/user language to Traditional Chinese
7. Verify the "Selectable" column label displays with correct Traditional Chinese translation
8. Navigate to Menu Management > Groupings > Item Categories
9. Repeat steps 3-7 for the Item Categories page

**Reproduction Result(s):**
1. The label shows "not_selecatble" regardless of the selected language
2. The translation is not applied correctly for any language

**Fix Result(s):**
1. The "Selectable" label displays correctly in English
2. The "Selectable" label displays with correct Simplified Chinese translation
3. The "Selectable" label displays with correct Traditional Chinese translation
4. The label is properly translated for all available languages on both Item Departments and Item Categories pages

### F2.Regression Testing - Menu Management Groupings Display

#### F2.1.Verify Item Department Listing and Selectable Column Data Integrity

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. Multiple item departments are configured with different Selectable settings (some selectable, some not)

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Departments
2. Verify the Item Departments listing page loads correctly
3. Verify the "Selectable" column label is correctly displayed
4. Verify the "Selectable" column data values are correct for each item department
5. Verify item departments marked as selectable show the correct value (e.g., Yes/checked)
6. Verify item departments marked as not selectable show the correct value (e.g., No/unchecked)
7. Edit an item department and toggle the Selectable setting
8. Verify the change is saved and reflected in the listing

**Reproduction Result(s):**
1. (If regression occurs) The item department listing does not load correctly
2. (If regression occurs) The Selectable column data values are incorrect or not displayed
3. (If regression occurs) Toggling the Selectable setting does not save correctly

**Fix Result(s):**
1. The Item Departments listing page loads correctly
2. The "Selectable" column label is correctly displayed with proper spelling
3. The Selectable column data values are correct for each item department
4. Item departments marked as selectable show the correct value
5. Item departments marked as not selectable show the correct value
6. Toggling the Selectable setting saves and reflects correctly in the listing

#### F2.2.Verify Item Category Listing and Selectable Column Data Integrity

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. Multiple item categories are configured with different Selectable settings (some selectable, some not)

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Categories
2. Verify the Item Categories listing page loads correctly
3. Verify the "Selectable" column label is correctly displayed
4. Verify the "Selectable" column data values are correct for each item category
5. Verify item categories marked as selectable show the correct value (e.g., Yes/checked)
6. Verify item categories marked as not selectable show the correct value (e.g., No/unchecked)
7. Edit an item category and toggle the Selectable setting
8. Verify the change is saved and reflected in the listing

**Reproduction Result(s):**
1. (If regression occurs) The item category listing does not load correctly
2. (If regression occurs) The Selectable column data values are incorrect or not displayed
3. (If regression occurs) Toggling the Selectable setting does not save correctly

**Fix Result(s):**
1. The Item Categories listing page loads correctly
2. The "Selectable" column label is correctly displayed with proper spelling
3. The Selectable column data values are correct for each item category
4. Item categories marked as selectable show the correct value
5. Item categories marked as not selectable show the correct value
6. Toggling the Selectable setting saves and reflects correctly in the listing

#### F2.3.Verify Item Department and Category Selectable Setting Reflects in Item Assignment

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. User is logged in with permissions to access Menu Management module
3. Item departments and categories are configured with Selectable settings
4. Menu items are available for assignment

**Step(s):**
1. Navigate to Menu Management > Groupings > Item Departments
2. Verify an item department is set as Selectable = Yes
3. Navigate to Menu Management and open an item for editing
4. Verify the selectable item department appears in the department dropdown/selection
5. Navigate to Menu Management > Groupings > Item Departments
6. Set the same item department to Selectable = No and save
7. Navigate back to the item editing page
8. Verify the non-selectable item department does not appear in the department dropdown/selection
9. Repeat steps 1-8 for Item Categories

**Reproduction Result(s):**
1. (If regression occurs) The Selectable setting does not correctly control which departments/categories appear in item assignment
2. (If regression occurs) Changing the Selectable setting does not reflect in item editing

**Fix Result(s):**
1. Item departments set as Selectable = Yes appear in item assignment dropdown
2. Item departments set as Selectable = No do not appear in item assignment dropdown
3. Item categories set as Selectable = Yes appear in item assignment dropdown
4. Item categories set as Selectable = No do not appear in item assignment dropdown
5. The Selectable setting correctly controls item department and category visibility

#### F2.4.Verify Sales Mix Summary Report - CUS0172 Reflects Correct Item Department and Category Data

**Prerequisite(s):**
1. Infrasys POS Backend is accessible (version 1.2.116.0 or later)
2. Item departments and categories are configured and assigned to items
3. Sales have been recorded on the current business day
4. Sales Mix Summary (Breakdown By Item Department And Item Category) - CUS0172 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Sales Mix Summary (Breakdown By Item Department And Item Category) - CUS0172
3. Select Date Type = Current Business Day
4. Select the target shop and outlet
5. Generate the report
6. Verify the Group column shows correct item department and item category names
7. Verify the report data is consistent with the configured item department and category settings

**Reproduction Result(s):**
1. (If regression occurs) The report shows incorrect item department or category names or groupings
2. (If regression occurs) The report data is inconsistent with the menu configuration

**Fix Result(s):**
1. The Sales Mix Summary Report (CUS0172) correctly displays item department and item category names
2. The Group column shows the correct breakdown by department and category
3. The report data is consistent with the configured item department and category settings
4. "Undefined" is shown for items without department or category as expected

## Appendix

### Bug Description (HERO-69449)

**Component:** CORE

**Current Behavior:**
Under Menu Management > Groupings > Item Departments and Menu Management > Groupings > Item Categories, the label on the "Selectable" column is mis-spelt as "not_selecatble".

**Expected Result:**
The label should spell correctly with proper translation.

### Release Notes (Version 1.2.116.0)

- **Release Date:** 2026-11-04
- **Bug Fix:** [JIRA-69449] [Bug fix] Mix spelling mistake on item Department and item Category page
- **Description:** Under Menu Management > Groupings > Item Departments and Menu Management > Groupings > Item Categories, the Label on the "Selectable" column is mis-spelt as "not_selecatble".
- **Setup:** No new setup is required
- **Fix:** The Label on the "Selectable" column is properly translated now on both Menu Management > Groupings > Item Departments and Menu Management > Groupings > Item Categories pages.

### Regression Test Scope - Related Modules and Functions

| Module / Function | Related Report | Regression Focus |
| --- | --- | --- |
| Menu Management > Groupings > Item Departments | - | Verify "Selectable" column label spelling, translation, and data integrity |
| Menu Management > Groupings > Item Categories | - | Verify "Selectable" column label spelling, translation, and data integrity |
| Multi-language Translation System | - | Verify "Selectable" label is properly translated across all installed languages |
| Item Department Selectable Setting | - | Verify Selectable setting controls department visibility in item assignment |
| Item Category Selectable Setting | - | Verify Selectable setting controls category visibility in item assignment |
| Sales Mix Summary Report | CUS0172 | Verify report reflects correct item department and category groupings |
| Employee Itemized Sales Report | CUS0168 | Verify report reflects correct department and category data |
| Item Department Sales by Outlet | POS0134 | Verify report reflects correct item department data |
| Item Department Totals by Outlet | POS0131 | Verify report reflects correct item department data |
| Item Department By Period Report | POS0118 | Verify report reflects correct item department data |
