# [Tech] PII - PII Member Anonymization Text Change Test Report

## Functional Testing

### 1.Scheduled Job Anonymization Display

#### 1.1.Newly anonymized member displays [ANONYMIZED-YYYYMMDD] after scheduled anonymization job

**Prerequisite(s):**
1. Access to Profile Management with permission to create profiles.
2. Access to TMS Operation and Reservation Operation for creating reservations with members.
3. Database access to the pii_data_properties table.
4. Access to Backend PII Management > Schedule Expired Data Anonymization.

**Step(s):**
1. In Profile Management > Profile Listing > Create Profile, create two records (Member A and Member B) where the booker information and additional email/phone are both filled in.
2. In TMS Operation > New Reservation, make a reservation with message and create a member linked to Member A.
3. In Reservation Operation > New Reservation, make a reservation with message and create a member linked to Member B.
4. In the pii_data_properties table, query the records created by the members and modify the dpro_expiry_time field to an expired time.
5. In Backend > PII Management > Schedule Expired Data Anonymization > Add New, add a record to anonymize the member and wait for the shell process to complete.
6. Go to PII Management > Search Tools > Member Module and search for the records.
7. In Member Management > Members, select the newly anonymized member record and view the details, then check the Date of Birth value.

**Test Result(s):**
1. The scheduled anonymization job completes and the expired member record is anonymized.
2. The Date of Birth value on the just-anonymized member detail page displays [ANONYMIZED-YYYYMMDD], where YYYYMMDD is the date on which the anonymization was performed.

#### 1.2.Previously anonymized member continues to display {Erased PII Data}

**Prerequisite(s):**
1. At least one member record that was anonymized before this release is available in the system.
2. Access to Member Management > Members and PII Management > Search Tools.

**Step(s):**
1. Go to PII Management > Search Tools and search for records that were previously anonymized.
2. Go to Member Management > Members and select a previously anonymized member record to view the details.
3. Check the Date of Birth value on the detail page.

**Test Result(s):**
1. Previously anonymized records can still be located through the search tools.
2. The Date of Birth value on the previously anonymized member detail page continues to display {Erased PII Data} and is not changed to the new text.

#### 1.3.Scheduled anonymization covers records from both TMS and Reservation applications

**Prerequisite(s):**
1. Reservations with messages and members have been created via both TMS Operation and Reservation Operation.
2. Database access to the pii_data_properties table to expire the records.
3. Access to Backend PII Management > Schedule Expired Data Anonymization.

**Step(s):**
1. In the pii_data_properties table, modify the dpro_expiry_time field of the records created by both TMS and Reservation to an expired time.
2. Add a scheduled expired data anonymization record in Backend and wait for the shell process to complete.
3. In PII Management > Search Tools, change Application to TMS and search for records; then change Application to Reservation and search for records.
4. Check the display of the newly anonymized records found under each application.

**Test Result(s):**
1. Records created under both the TMS and Reservation applications are anonymized by the scheduled job.
2. All newly anonymized records found in the search display the [ANONYMIZED-YYYYMMDD] text in their anonymized fields.

### 2.Manual Anonymization Display

#### 2.1.Manual anonymization via Anonymize button displays [ANONYMIZED-YYYYMMDD]

**Prerequisite(s):**
1. At least one non-anonymized member record exists in PII Management > Search Tools > Member Module.
2. The user has permission to perform manual anonymization.

**Step(s):**
1. Go to PII Management > Search Tools > Member Module and search for records.
2. Tick one record and click the "Anonymize" button, then confirm the operation.
3. Go to Member Management > Members and select the just-anonymized member to view the details.
4. Check the Date of Birth value.

**Test Result(s):**
1. The manual anonymization completes successfully for the selected record.
2. The Date of Birth value on the just-anonymized member detail page displays [ANONYMIZED-YYYYMMDD].

#### 2.2.Manual anonymization of reservation and message records under TMS and Reservation applications

**Prerequisite(s):**
1. Non-anonymized reservation records exist under the TMS application and non-anonymized member and message records exist under the Reservation application.
2. The user has permission to perform manual anonymization in PII Management > Search Tools.

**Step(s):**
1. In PII Management > Search Tools, change Application to TMS and search for records.
2. Tick one reservation record and click the "Anonymize" button, then confirm the operation.
3. Change Application to Reservation and search for records.
4. Tick one member and one message record and click the "Anonymize" button, then confirm the operation.
5. Search for the processed records again and check the display of their anonymized fields.

**Test Result(s):**
1. The reservation record under TMS and the member and message records under Reservation are all anonymized successfully.
2. All the newly anonymized records display [ANONYMIZED-YYYYMMDD] in their anonymized fields.

#### 2.3.Cancelling manual anonymization keeps record data unchanged

**Prerequisite(s):**
1. At least one non-anonymized member record exists in PII Management > Search Tools > Member Module.
2. The anonymization confirmation prompt is enabled for the Anonymize button.

**Step(s):**
1. Go to PII Management > Search Tools > Member Module and search for records.
2. Tick one record and click the "Anonymize" button.
3. Cancel the confirmation prompt instead of confirming.
4. Search for the record again and view its detail data.

**Test Result(s):**
1. The record is not anonymized after the confirmation is cancelled.
2. The original data is retained and no [ANONYMIZED-YYYYMMDD] or {Erased PII Data} text appears on the record.

### 3.Display Consistency and Edge Scenarios

#### 3.1.Anonymization text format and date portion validation

**Prerequisite(s):**
1. At least one non-anonymized member record is available for manual anonymization.
2. The current system date is known to the tester.

**Step(s):**
1. Anonymize one member record manually via the "Anonymize" button on the current date.
2. Open the anonymized member detail page in Member Management > Members.
3. Check the exact text shown for the Date of Birth value.

**Test Result(s):**
1. The displayed text exactly matches the [ANONYMIZED-YYYYMMDD] format, including the square brackets.
2. The YYYYMMDD portion equals the date on which the anonymization was executed, for example [ANONYMIZED-20260914].

#### 3.2.Mixed old and new anonymized records displayed in member listing

**Prerequisite(s):**
1. The system contains at least one member anonymized before this release and one member anonymized after this release.
2. Access to Member Management > Members.

**Step(s):**
1. Go to Member Management > Members and locate both the previously anonymized member and the newly anonymized member in the listing.
2. Open the details of each member and compare the Date of Birth values.

**Test Result(s):**
1. The newly anonymized member displays [ANONYMIZED-YYYYMMDD] for Date of Birth.
2. The previously anonymized member displays {Erased PII Data} for Date of Birth.
3. Both records are listed without any display error in the listing page.

#### 3.3.Search tools still locate previously anonymized records

**Prerequisite(s):**
1. Records anonymized before this release exist under TMS and Reservation applications.

**Step(s):**
1. In PII Management > Search Tools, search for some records that were previously anonymized.
2. Open one of the found records to view its details.

**Test Result(s):**
1. The previously anonymized records are returned by the search.
2. Their details continue to show {Erased PII Data} without any conversion to the new text and without any error.
