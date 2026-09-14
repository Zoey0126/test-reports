# #1811112 - TR: incorrect passerby file format Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Passerby File Export |
| Code | gc_passerby_export |

## Report Functional Verification

### 1.Passerby File Content Format

#### 1.1.Verify the LST file contains the title line only once
**Prerequisite(s):**
  1. The gc_passerby_export interface is configured and enabled for the outlet.
  2. Additional Setup > Outlet Mapping is configured as [outlet_code],[Book360 POS code].
**Step(s):**
  1. Trigger the passerby export to generate a new ZIP package (e.g., SLPR_POS1_YYYYMMDD.zip containing the LST file).
  2. Unzip the package with the configured password.
  3. Open the LST file and check the title/header lines.
  4. Trigger the export again (retry) and re-check the regenerated LST file.
**Reproduction Result(s):**
  1. Before the fix, the title line appeared multiple times in the LST file because the LST file was always opened in append mode and the ZIP archive was created without overwrite, so a retry or a leftover local file duplicated the header.
**Fix Result(s):**
  1. After the fix, the generated LST file contains the title line exactly once.
  2. A retried or regenerated export does not duplicate the title line, and no errors are displayed.

#### 1.2.Verify the LST file content rows are not duplicated
**Prerequisite(s):**
  1. The gc_passerby_export interface is configured and enabled for the outlet.
  2. Passerby transaction data exists for the export period.
**Step(s):**
  1. Trigger the passerby export and unzip the generated package.
  2. Open the LST file and check the content rows.
  3. Trigger the export again (retry) and re-check the regenerated LST file.
  4. Compare the row count with the source passerby data of the period.
**Reproduction Result(s):**
  1. Before the fix, the content rows appeared multiple times in the LST file after a retry or when a leftover local file existed.
**Fix Result(s):**
  1. After the fix, each content row appears exactly once in the LST file.
  2. The row count matches the source passerby data, and no errors are displayed.

#### 1.3.Verify the ZIP package is regenerated correctly on repeated export
**Prerequisite(s):**
  1. The gc_passerby_export interface is configured and enabled for the outlet.
**Step(s):**
  1. Trigger the passerby export and keep the generated ZIP package.
  2. Trigger the passerby export again for the same outlet and period.
  3. Open the newly generated ZIP package and check its entries.
**Reproduction Result(s):**
  1. Before the fix, the ZIP archive was created without overwrite, so the regenerated package could contain duplicated or leftover entries from previous exports.
**Fix Result(s):**
  1. After the fix, the newly generated ZIP package overwrites the previous one cleanly and contains no duplicated entries.
  2. The package content is correct and no errors are displayed.

### 2.Verify the ZIP file can be unzipped with the configured password
**Prerequisite(s):**
  1. A common unzip tool is available on the test machine.
  2. The unzip password is configured for the passerby export.
**Step(s):**
  1. Trigger the passerby export to generate the ZIP package.
  2. Unzip the package with the configured password using the common unzip tool.
  3. Check the unzipped LST file content.
**Reproduction Result(s):**
  1. Before the fix, the ZIP used AES-256 encryption, which many unzip tools treat as a wrong password, so the package could not be unzipped with the configured password.
**Fix Result(s):**
  1. After the fix, the ZIP package can be unzipped successfully with the configured password using common unzip tools.
  2. The unzipped LST file content is correct and readable.

### 3.POS Code Mapping

#### 3.1.Verify POS_Code is exported as the Book360 POS code from Outlet Mapping
**Prerequisite(s):**
  1. Additional Setup > Outlet Mapping is configured as [outlet_code],[Book360 POS code].
**Step(s):**
  1. Trigger the passerby export for the mapped outlet.
  2. Unzip the package and open the LST file.
  3. Check the POS_Code value in the file.
**Reproduction Result(s):**
  1. Before the fix, POS_Code was written as the outlet name instead of the Outlet Mapping value, so the POS code in the file was incorrect.
**Fix Result(s):**
  1. After the fix, POS_Code is written as the Book360 POS code from Outlet Mapping.
  2. The POS_Code matches the configured mapping, and no errors are displayed.

#### 3.2.Verify the exported passerby file is accepted by Book360
**Prerequisite(s):**
  1. Book360 is available and the outlet POS code is configured in Book360.
  2. Additional Setup > Outlet Mapping is configured as [outlet_code],[Book360 POS code].
**Step(s):**
  1. Trigger the passerby export for the mapped outlet.
  2. Deliver/import the generated file to Book360.
  3. Check the import result in Book360.
**Reproduction Result(s):**
  1. Before the fix, Book360 rejected the file because the POS code was not exported correctly and the file format was incorrect.
**Fix Result(s):**
  1. After the fix, Book360 accepts the imported passerby file and the POS code is recognized correctly.
  2. The passerby data is imported completely, and no errors are displayed.

### 4.Regression Scope
- gc_passerby_export scheduled export for all configured outlets
- Manual re-trigger and retry of the passerby export
- ZIP package generation, password protection, and unzip verification
- Book360 import and POS code validation of the passerby file

## Test Environment
- **Version**: v1.0.0
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: HQ Test Environment, Local Test Environment
