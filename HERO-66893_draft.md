# POS Export - BI Interface - Add pos_check_extra_infos data file (2/2) Test Report

## Functional Testing

### 1.Export File Generation and Delivery

#### 1.1.Daily scheduled batch exports the pos_check_extra_infos file

**Prerequisite(s):**
1. A BI interface has been added in System Management > Scheduled Tasks with the corresponding parameters as required.
2. The SFTP server used by existing BI export files is reachable.
3. Source data exists in the pos_check_extra_infos table.

**Step(s):**
1. Wait for the scheduled daily batch to run automatically.
2. Check the files delivered to the SFTP server directory used by existing BI export files.

**Test Result(s):**
1. A single CSV file named real_pos_check_extra_infos_YYYYMMDDHHMMSS.csv (e.g., real_pos_check_extra_infos_20260330090841.csv) is produced.
2. The file is delivered via SFTP to the same server and directory path as the existing BI export files, on the same daily batch schedule.
3. The file contains data from all three sections: pms/payment, discount, and discount_reason.

#### 1.2.Manual Run from the Scheduled Tasks view generates the file

**Prerequisite(s):**
1. A BI interface has been added in System Management > Scheduled Tasks with the corresponding parameters as required.

**Step(s):**
1. Go to System Management > Scheduled Tasks.
2. Click the Run button on the view page.
3. Wait for Infrasys BI to finish running and check the FTP server.

**Test Result(s):**
1. The export job runs and the pos_check_extra_infos file is generated and delivered to the FTP server.
2. The exported folder contains pos_check_extra_infos.csv regardless of whether the prefix is real or full.

#### 1.3.Existing BI export files remain unchanged

**Prerequisite(s):**
1. The BI export job is configured with the existing set of export files plus the new pos_check_extra_infos file.
2. A previous daily batch result is available for comparison.

**Step(s):**
1. Run the daily batch export.
2. Compare the existing BI export files with the previous batch result.

**Test Result(s):**
1. The pos_check_extra_infos file is added as a brand-new file to the existing set of BI export files.
2. All existing BI export files are generated and delivered unchanged.

### 2.Section Filter Logic

#### 2.1.pms/payment section row-level filter

**Prerequisite(s):**
1. Records are prepared in the pos_check_extra_infos source table:
   a. ckei_section = pms, ckei_by = payment, ckei_status = a
   b. ckei_section = pms, ckei_by = payment, ckei_status = d
   c. ckei_section = pms, ckei_by = refund, ckei_status = a

**Step(s):**
1. Run the BI export job.
2. Open the exported pos_check_extra_infos file and check the pms/payment rows.

**Test Result(s):**
1. Only records where ckei_section = pms AND ckei_by = payment AND ckei_status is not null, not empty, and not d are included in the pms/payment section.
2. Record b is excluded because ckei_status = d, and record c is excluded because ckei_by is not payment.

#### 2.2.discount section rows and variables

**Prerequisite(s):**
1. Records with ckei_section = discount and valid ckei_status exist in the source table, including member_number and reference variables.

**Step(s):**
1. Run the BI export job.
2. Open the exported file and check the discount section rows.

**Test Result(s):**
1. Only records where ckei_section = discount AND ckei_status is not null, not empty, and not d are included.
2. The ckei_variable values member_number and reference are present in the exported discount rows.

#### 2.3.discount_reason section rows and variables

**Prerequisite(s):**
1. Records with ckei_section = discount_reason and valid ckei_status exist in the source table, including the id variable.

**Step(s):**
1. Run the BI export job.
2. Open the exported file and check the discount_reason section rows.

**Test Result(s):**
1. Only records where ckei_section = discount_reason AND ckei_status is not null, not empty, and not d are included.
2. The ckei_variable value id is present in the exported discount_reason rows.

#### 2.4.Global exclusion of null, empty, and deleted status

**Prerequisite(s):**
1. Records are prepared in the source table with ckei_status = NULL, ckei_status = empty string, and ckei_status = d, across different ckei_section values.

**Step(s):**
1. Run the BI export job.
2. Search the exported file for the prepared records.

**Test Result(s):**
1. Records with ckei_status NULL, empty string, or d are excluded from all sections regardless of ckei_section or ckei_by values.

#### 2.5.Section-specific filter isolation

**Prerequisite(s):**
1. Records exist where ckei_section = discount or ckei_section = discount_reason with various ckei_by values and valid ckei_status.

**Step(s):**
1. Run the BI export job.
2. Check the discount and discount_reason rows in the exported file.

**Test Result(s):**
1. The discount and discount_reason sections use only the ckei_status filter plus their own section identifiers.
2. The ckei_by = payment condition is applied only to the pms section and does not filter rows of the other sections.

### 3.File Format and Edge Conditions

#### 3.1.File naming and format consistency with existing BI exports

**Prerequisite(s):**
1. The BI export job has completed successfully at least once.

**Step(s):**
1. Check the file name of the exported pos_check_extra_infos file.
2. Compare the CSV format, encoding, and delimiter with the existing BI export files.
3. Check the header row of the new file.

**Test Result(s):**
1. The file name follows the pattern real_pos_check_extra_infos_YYYYMMDDHHMMSS.csv.
2. The file uses the same CSV format, encoding, and delimiter conventions as the existing BI export files.
3. The file includes a header row with column names matching the variable names from the source data.

#### 3.2.No matching records produces a header-only file

**Prerequisite(s):**
1. No records in the pos_check_extra_infos table match the filter criteria for the current batch.

**Step(s):**
1. Run the daily batch export.
2. Check the SFTP delivery directory.

**Test Result(s):**
1. An empty file with headers only is still produced and delivered.
2. The existing BI export files are not affected.

### 4.Regression Scope

1. Generation and SFTP delivery of the existing set of BI export files.
2. Other BI interfaces configured in System Management > Scheduled Tasks.
3. Manual Run and automatic scheduled execution of export tasks.
4. Downstream consumers of the existing BI export files.
