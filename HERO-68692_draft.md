# [Tech] PII - PII Search Tool - Disable anonymize member data in TMS/Reservation application Test Report

## Functional Testing

### 1.Member Anonymization Restriction in TMS/Reservation Application

#### 1.1.Anonymize option for member information is disabled with alert message
**Prerequisite(s):**
1. Access to the backend PII Management > Search Tools page is available
2. TMS/Reservation data containing reservations with corresponding member information exists, including records that are expiring

**Step(s):**
1. Set Application to TMS/Reservation on the Search Tools page
2. Input conditions to search records with member and run the search
3. Review the results showing expiring reservations, the corresponding members and the messages sent
4. Check the member information section PII Data (Member) and attempt to select and anonymize the member data

**Reproduction Result(s):**
1. Before the fix, the member information could be selected and anonymized in the TMS/Reservation application even though only the reservation was expiring and not the member itself, so the member could be mistakenly anonymized during TMS/Reservation anonymization processes

**Fix Result(s):**
1. The anonymize checkbox option for member information is disabled and the member cannot be anonymized in the TMS/Reservation application
2. The alert message Member data cannot be anonymized in this application is displayed under PII Data (Member)

#### 1.2.Ticking member records does not allow anonymization
**Prerequisite(s):**
1. Application is set to TMS/Reservation and search results containing member records are displayed

**Step(s):**
1. Tick some reservation records in the search results
2. Tick some member records and attempt to proceed with the Anonymize button
3. Untick all member records and click the Anonymize button

**Reproduction Result(s):**
1. Before the fix, ticked member records could be included in the anonymization together with the reservation records

**Fix Result(s):**
1. Member records cannot be selected for anonymization or are excluded from the anonymization action
2. Only the selected reservation records are anonymized and the member data remains intact

### 2.Expired Record Selection Behavior

#### 2.1.Select All Expired Records with member that has expired data
**Prerequisite(s):**
1. Application is set to TMS/Reservation
2. A member whose related data is expiring is available in the search results

**Step(s):**
1. Input conditions to search expired records with member
2. Click the Select All Expired Records In Current Page button
3. Review the selected records and proceed with anonymization

**Reproduction Result(s):**
1. Before the fix, the selection could include the member data and the member could be anonymized together with the expired reservation records

**Fix Result(s):**
1. Only the expired reservation records are selected and anonymized
2. The member data is excluded from the anonymization and the alert message is shown under PII Data (Member)

#### 2.2.Select All Expired Records with member that has no expired data
**Prerequisite(s):**
1. Application is set to TMS/Reservation
2. A member without expired data is related to expiring reservation records

**Step(s):**
1. Search expired records with member where the member itself has no expired data
2. Click Select All Expired Records In Current Page and review the selection

**Reproduction Result(s):**
1. Before the fix, the member without expired data could still be mistakenly anonymized through the member selection option

**Fix Result(s):**
1. The member without expired data is not selected and not anonymized
2. Only the expiring reservation data is processed

### 3.Reservation Anonymization Regression

#### 3.1.Reservation records can still be anonymized
**Prerequisite(s):**
1. Application is set to TMS/Reservation with expiring reservation records available

**Step(s):**
1. Search and tick expired reservation records
2. Click the Anonymize button and confirm

**Reproduction Result(s):**
1. Before the fix, the anonymization of reservation records in the TMS/Reservation application worked correctly

**Fix Result(s):**
1. The anonymization of reservation records still works as before the change
2. Anonymized reservation data no longer exposes personal information while the member data is untouched

### 4.Member Application Anonymization Regression

#### 4.1.Member application still supports member anonymization
**Prerequisite(s):**
1. Application can be switched to Member on the Search Tools page

**Step(s):**
1. Change Application to Member and search records
2. Tick some member records and click the Anonymize button

**Reproduction Result(s):**
1. Before the fix, member anonymization in the Member application worked correctly

**Fix Result(s):**
1. The Anonymize button remains available and functional in the Member application
2. The selected member records are anonymized successfully

### 5.Regression Scope
1. PII Search Tool in the TMS/Reservation application - member data display, selection and anonymize restriction
2. Alert message display under PII Data (Member)
3. Select All Expired Records In Current Page function with members having and not having expired data
4. Reservation record anonymization in the TMS/Reservation application
5. Member record anonymization in the Member application
