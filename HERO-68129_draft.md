# Report - Add Syndicate Owner Indicator field into TMS025 Unsuccessful Waiting List Report (By Outlet) Report Test Report

## Functional Testing

### 1.Field Position Verification

#### 1.1.Syndicate Owner Indicator column appears immediately LEFT of Syndicate Owner column

**Prerequisite(s):**
  1. The build containing the HERO-68129 enhancement is deployed to the SQL Server (MSSQL) test environment.
  2. Reservations with Syndicate Owner Indicator values (Y, N and blank) exist for the selected business date range and outlets.
  3. The TMS025 Unsuccessful Waiting List Report (By Outlet) is accessible.

**Step(s):**
  1. Open Reports and run TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select the default parameters and enter a business date range that returns reservations with Syndicate Owner data.
  3. Click "Run" button.
  4. Inspect the column order in the report header.
  5. Confirm the "Syndicate Owner Indicator" column is placed immediately to the LEFT of the existing "Syndicate Owner" column.
  6. Confirm the column that was previously to the left of "Syndicate Owner" is now to the left of "Syndicate Owner Indicator".

**Test Result(s):**
  1. Report loads successfully on the MSSQL data service.
  2. The "Syndicate Owner Indicator" column is displayed immediately to the left of the "Syndicate Owner" column.
  3. No existing column is removed, renamed or reordered beyond the insertion of the new column.
  4. No errors displayed.

#### 1.2.Syndicate Owner Indicator column added in every table/section that contains Syndicate Owner

**Prerequisite(s):**
  1. The TMS025 report layout is reviewed to confirm whether "Syndicate Owner" appears in more than one table or section (e.g. summary section, detail section, subtotal section).
  2. Reservations with Syndicate Owner Indicator values exist for the selected parameters.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select parameters that produce a report output containing every table/section that includes the "Syndicate Owner" column.
  3. Run the report.
  4. For each table or section that contains a "Syndicate Owner" column, verify that a "Syndicate Owner Indicator" column has been added immediately to its left.

**Test Result(s):**
  1. Every table or section that previously contained a "Syndicate Owner" column now also contains a "Syndicate Owner Indicator" column immediately to its left.
  2. No occurrence of "Syndicate Owner" is missing the new indicator column.
  3. No errors displayed.

#### 1.3.Column order is preserved across outlet groupings

**Prerequisite(s):**
  1. Multiple outlets with unsuccessful waiting list reservations exist within the selected business date range.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select "All Outlets" or "Multiple Outlets" so the report groups results by outlet.
  3. Run the report.
  4. Verify the column order within each outlet grouping.
  5. Confirm the "Syndicate Owner Indicator" column is immediately left of "Syndicate Owner" in every outlet grouping.

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column appears immediately left of "Syndicate Owner" in every outlet grouping.
  2. Column order is consistent across all outlet groupings.
  3. No errors displayed.

### 2.Data Accuracy

#### 2.1.Indicator value Y displays correctly and matches reservation data

**Prerequisite(s):**
  1. Reservations with rmem_syndicate_owner_indicator = Y exist for the selected business date range and outlets.
  2. Direct database access to hkjc_resv_members.rmem_syndicate_owner_indicator is available for verification.

**Step(s):**
  1. Query the database to identify reservations with rmem_syndicate_owner_indicator = Y that appear on the unsuccessful waiting list for the selected date range.
  2. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  3. Select the parameters that cover those reservations.
  4. Run the report.
  5. For each reservation with indicator Y, compare the value shown in the "Syndicate Owner Indicator" column with the database value.

**Test Result(s):**
  1. Reservations with rmem_syndicate_owner_indicator = Y display "Y" in the Syndicate Owner Indicator column.
  2. Indicator values match the reservation member record data for every listed reservation.
  3. No errors displayed.

#### 2.2.Indicator value N displays correctly and matches reservation data

**Prerequisite(s):**
  1. Reservations with rmem_syndicate_owner_indicator = N exist for the selected business date range and outlets.
  2. Direct database access to hkjc_resv_members.rmem_syndicate_owner_indicator is available for verification.

**Step(s):**
  1. Query the database to identify reservations with rmem_syndicate_owner_indicator = N that appear on the unsuccessful waiting list for the selected date range.
  2. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  3. Select the parameters that cover those reservations.
  4. Run the report.
  5. For each reservation with indicator N, compare the value shown in the "Syndicate Owner Indicator" column with the database value.

**Test Result(s):**
  1. Reservations with rmem_syndicate_owner_indicator = N display "N" in the Syndicate Owner Indicator column.
  2. Indicator values match the reservation member record data for every listed reservation.
  3. No errors displayed.

#### 2.3.Mixed indicator values Y, N and blank display correctly in the same report

**Prerequisite(s):**
  1. The selected business date range and outlets contain a mix of reservations with indicator Y, indicator N and reservations without an indicator value (blank).

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select parameters covering reservations with Y, N and blank indicator values.
  3. Run the report.
  4. Verify that Y, N and blank indicator cells are all displayed correctly in the same report output.
  5. Cross-check several rows against the database values in hkjc_resv_members.rmem_syndicate_owner_indicator.

**Test Result(s):**
  1. Rows with indicator Y show Y, rows with indicator N show N and rows without an indicator value show blank.
  2. Indicator values match the reservation member record for every listed reservation.
  3. Column position of the indicator stays immediately left of Syndicate Owner for all rows.
  4. No errors displayed.

#### 2.4.Indicator values are correct when filtering by a single outlet

**Prerequisite(s):**
  1. One outlet has unsuccessful waiting list reservations with a mix of indicator values Y, N and blank.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select only that single outlet.
  3. Set the business date range that includes the prepared reservations.
  4. Run the report.
  5. Compare every indicator cell with the rmem_syndicate_owner_indicator value on the reservation member record.

**Test Result(s):**
  1. Indicator values match the reservation member record for every listed reservation.
  2. The report filters by the selected outlet correctly.
  3. No errors displayed.

### 3.Report Format and Layout

#### 3.1.Column header naming and label

**Prerequisite(s):**
  1. The build containing the HERO-68129 enhancement is deployed.
  2. Reservations with Syndicate Owner Indicator values exist for the selected parameters.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Run the report with parameters that return reservations with Syndicate Owner data.
  3. Inspect the header of the new column.
  4. Verify the header text reads "Syndicate Owner Indicator".
  5. Verify the header is consistent across every table or section that contains the column.

**Test Result(s):**
  1. The new column header reads "Syndicate Owner Indicator".
  2. The header text is consistent across all tables and sections.
  3. No existing column header is renamed or removed.
  4. No errors displayed.

#### 3.2.Column alignment and width

**Prerequisite(s):**
  1. Reservations with indicator values Y, N and blank exist for the selected parameters.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Run the report with the prepared parameters.
  3. Verify the alignment of the "Syndicate Owner Indicator" column header and data cells.
  4. Verify the column width is sufficient to display Y, N and blank values without truncation.
  5. Verify the column does not overlap or push the "Syndicate Owner" column out of the report layout.

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column header and data cells are aligned consistently with the surrounding columns.
  2. The column width displays Y, N and blank values without truncation.
  3. The "Syndicate Owner" column and other existing columns remain within the report layout.
  4. No errors displayed.

#### 3.3.Export to CSV, Excel, Excel (.xlsx), PDF, Word, PostScript and PowerPoint (.pptx) includes the indicator column

**Prerequisite(s):**
  1. Reservations with indicator values Y, N and blank exist for the selected parameters.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Run the report with the prepared parameters.
  3. Select "Export" from the menu and export to CSV.
  4. Open the exported CSV file and verify the "Syndicate Owner Indicator" column.
  5. Repeat the export for Excel, Excel (.xlsx), PDF, Word, PostScript and PowerPoint (.pptx).
  6. For each exported file, verify the "Syndicate Owner Indicator" column is present, positioned immediately left of "Syndicate Owner" and contains the correct values.

**Test Result(s):**
  1. Each export is successful.
  2. The "Syndicate Owner Indicator" column appears in every exported format at the same position as on screen.
  3. Indicator values Y, N and blank match the on-screen report in every exported format.
  4. No errors displayed.

#### 3.4.Print to HTML and PDF includes the indicator column

**Prerequisite(s):**
  1. Reservations with indicator values Y, N and blank exist for the selected parameters.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Run the report with the prepared parameters.
  3. Verify the on-screen preview shows the "Syndicate Owner Indicator" column.
  4. Select "Print" from the menu and print the report to PDF.
  5. Verify the printed PDF shows the "Syndicate Owner Indicator" column.
  6. Print the report to HTML and verify the output.

**Test Result(s):**
  1. On-screen preview shows the "Syndicate Owner Indicator" column with correct values.
  2. Printed PDF shows the "Syndicate Owner Indicator" column with correct values.
  3. Printed HTML shows the "Syndicate Owner Indicator" column with correct values.
  4. Blank indicator cells stay blank in both print formats.
  5. No errors displayed.

### 4.Edge Cases

#### 4.1.Empty or null indicator value

**Prerequisite(s):**
  1. Older or legacy reservations with no rmem_syndicate_owner_indicator value (NULL or empty) exist within the selected business date range.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select the business date range that includes the legacy reservations without an indicator value.
  3. Run the report.
  4. Verify the "Syndicate Owner Indicator" cell for the legacy reservations.
  5. Verify the rest of the row (Syndicate Owner and other columns) still displays correctly.

**Test Result(s):**
  1. The "Syndicate Owner Indicator" cell is left blank for reservations with no indicator value.
  2. No error values such as 0, null, "NULL" or "undefined" text are shown in the indicator cell.
  3. The rest of the row still displays correctly.
  4. No errors displayed.

#### 4.2.Special or unexpected characters in the indicator value

**Prerequisite(s):**
  1. Test data is prepared (or database is updated in the test environment) so that hkjc_resv_members.rmem_syndicate_owner_indicator contains a non-standard value (e.g. lowercase y/n, whitespace, or an unexpected character) for at least one reservation on the unsuccessful waiting list.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select parameters that cover the reservation with the non-standard indicator value.
  3. Run the report.
  4. Verify how the "Syndicate Owner Indicator" cell renders the non-standard value.
  5. Verify the column layout is not broken by the unexpected value.

**Test Result(s):**
  1. The "Syndicate Owner Indicator" cell renders the stored value without breaking the report layout.
  2. The column width and alignment remain consistent with surrounding columns.
  3. No garbled characters or rendering errors are shown.
  4. No errors displayed.

#### 4.3.Multiple syndicate owners on the same reservation

**Prerequisite(s):**
  1. A reservation on the unsuccessful waiting list has multiple syndicate owner member records, some with different indicator values.

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select parameters that cover the reservation with multiple syndicate owners.
  3. Run the report.
  4. Verify each syndicate owner row shows its own indicator value.
  5. Cross-check each row against the rmem_syndicate_owner_indicator value on its reservation member record.

**Test Result(s):**
  1. Each syndicate owner row displays its own indicator value from the corresponding reservation member record.
  2. Indicator values are correct per row even when multiple syndicate owners exist for the same reservation.
  3. No errors displayed.

#### 4.4.Pagination - indicator column appears on every page

**Prerequisite(s):**
  1. The selected parameters produce a report output that spans multiple pages (enough reservations to trigger pagination).

**Step(s):**
  1. Open TMS025 Unsuccessful Waiting List Report (By Outlet).
  2. Select parameters that produce a multi-page report.
  3. Run the report.
  4. Navigate through every page of the report.
  5. On each page, verify the "Syndicate Owner Indicator" column is present in the header and is positioned immediately left of "Syndicate Owner".
  6. Verify indicator values on each page match the reservation member record data.

**Test Result(s):**
  1. The "Syndicate Owner Indicator" column appears in the header on every page.
  2. The column is positioned immediately left of "Syndicate Owner" on every page.
  3. Indicator values are correct and consistent across all pages.
  4. No errors displayed.

## Test Environment

- **Version**: Sprint build containing the HERO-68129 enhancement
- **Data Service**: MSSQL (SQL Server) - this enhancement is MSSQL only
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment (SQL Server), ER Test Environment
