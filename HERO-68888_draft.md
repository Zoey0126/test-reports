# HERO-68888 Test Report

## Functional Testing

### F1.Aspen Membership Interface — Display Owner Code Setup

#### F1.1.Verify Interface Setup For Display Owner Code In Member Enquiry Result

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (Windows 11 Enterprise) and / or a vertical mobile device (Android Mobile / Apple iPad Tablet / iPhone)
2. An Aspen XML membership interface is configured on Infrasys Cloud platform > Interface Control > Interfaces (vendor = Aspen XML membership interface)
3. The interface setup includes the "Display Owner Code in Member Enquiry Result" field
4. Test environment: Local / HQ test environment

**Step(s):**
1. Open the Infrasys Cloud platform > Interface Control > Interfaces
2. Click "Add New" to configure a new setup or edit the existing Aspen XML membership interface setup
3. Verify the interface setup includes the "Display Owner Code in Member Enquiry Result" field (with values Yes / No)
4. Set "Display Owner Code in Member Enquiry Result" = Yes and save the record
5. Set "Display Owner Code in Member Enquiry Result" = No and save the record
6. Leave "Display Owner Code in Member Enquiry Result" empty and save the record

**Test Result(s):**
1. The Aspen XML membership interface setup page shows the "Display Owner Code in Member Enquiry Result" field as per the new feature
2. The field can be set to Yes, No, or left empty and saved successfully
3. The configuration is persisted correctly across interface setup edits

### F2.Member Search List — Owner Code Column

#### F2.1.Verify Owner Code Column Is Added To Member Search List When Setup Is Yes

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (horizontal desktop view) and a vertical mobile device
2. An Aspen XML membership interface is configured with "Display Owner Code in Member Enquiry Result" = Yes
3. Multiple Aspen members exist on the external party so that the member search returns more than one result
4. At least one member has a non-empty ARIO_CODE value; at least one member has an empty / missing ARIO_CODE value
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a new check on the POS client on the horizontal desktop view
2. Click the function "Set Member"
3. Input the search criteria for member search
4. Click the button "Enter"
5. Verify the system triggers the external party API for Aspen member enquiry and returns more than one member
6. Verify the system shows the returned member search list with the "Owner Code" column added next to "Member No."
7. For each row in the list:
   - If the field "ARIO_CODE" of a member is returned with a non-empty value, verify the value is shown in the "Owner Code" column
   - If the field "ARIO_CODE" is missing or empty, verify a blank string is shown in the "Owner Code" column
8. Verify the existing "Member No." and "Name" columns continue to display correctly
9. Repeat the same flow on the vertical mobile view

**Test Result(s):**
1. The "Set Member" function opens the member search panel on both horizontal desktop view and vertical mobile view
2. After inputting the search criteria and clicking "Enter", the system triggers the external party API for Aspen member enquiry and returns more than one member
3. The member search list shows the "Owner Code" column added next to "Member No." on both horizontal desktop view and vertical mobile view
4. For each row, when ARIO_CODE is returned with a non-empty value, the value is shown in the "Owner Code" column; when ARIO_CODE is missing or empty, a blank string is shown
5. The existing "Member No." and "Name" columns continue to display correctly — no regression in the existing handling

#### F2.2.Verify Owner Code Column Is Not Added When Setup Is No Or Empty

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (horizontal desktop view) and a vertical mobile device
2. An Aspen XML membership interface is configured with "Display Owner Code in Member Enquiry Result" = No (or left empty)
3. Multiple Aspen members exist on the external party so that the member search returns more than one result
4. Test environment: Local / HQ test environment

**Step(s):**
1. Open a new check on the POS client on the horizontal desktop view
2. Click the function "Set Member"
3. Input the search criteria for member search
4. Click the button "Enter"
5. Verify the system triggers the external party API for Aspen member enquiry and returns more than one member
6. Verify the member search list does NOT show the "Owner Code" column — only "Member No." and "Name" columns are shown
7. Repeat the same flow on the vertical mobile view

**Test Result(s):**
1. The "Set Member" function opens the member search panel on both horizontal desktop view and vertical mobile view
2. After inputting the search criteria and clicking "Enter", the system triggers the external party API for Aspen member enquiry and returns more than one member
3. The member search list does NOT show the "Owner Code" column — only "Member No." and "Name" columns are shown, following the existing handling
4. The behaviour is consistent when "Display Owner Code in Member Enquiry Result" is set to No or left empty

### F3.Member Details Page — Owner Code Row

#### F3.1.Verify Owner Code Row Is Inserted On Member Details Page When Setup Is Yes

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (horizontal desktop view) and a vertical mobile device
2. An Aspen XML membership interface is configured with "Display Owner Code in Member Enquiry Result" = Yes
3. An Aspen member with a non-empty ARIO_CODE value exists on the external party
4. An Aspen member with an empty / missing ARIO_CODE value exists on the external party
5. Test environment: Local / HQ test environment

**Step(s):**
1. Open a new check on the POS client on the horizontal desktop view
2. Click the function "Set Member"
3. Input the search criteria for the member with a non-empty ARIO_CODE value
4. Click the button "Enter"
5. If multiple members are returned, select the target member; if only one member is returned, go to step 6
6. Verify the system shows the member details page for the member (tab "Basic Information")
7. Verify the system inserts a row "Owner Code" at the end of the member details
8. Verify the ARIO_CODE value of the member is shown in the "Owner Code" row
9. Repeat the flow for the member with an empty / missing ARIO_CODE value and verify the "Owner Code" row shows a blank string
10. Click the button "Set Member" and verify the member details including "Owner Code" are stored in the check
11. Repeat the same flow on the vertical mobile view

**Test Result(s):**
1. The "Set Member" function triggers the Aspen member enquiry and returns the target member on both horizontal desktop view and vertical mobile view
2. The member details page (tab "Basic Information") shows the member details
3. The system inserts a row "Owner Code" at the end of the member details on both horizontal desktop view and vertical mobile view
4. For the member with a non-empty ARIO_CODE value, the ARIO_CODE value is shown in the "Owner Code" row
5. For the member with an empty / missing ARIO_CODE value, a blank string is shown in the "Owner Code" row
6. After clicking "Set Member", the member details including "Owner Code" are stored in the check

#### F3.2.Verify Owner Code Row Is Not Inserted On Member Details Page When Setup Is No Or Empty

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (horizontal desktop view) and a vertical mobile device
2. An Aspen XML membership interface is configured with "Display Owner Code in Member Enquiry Result" = No (or left empty)
3. An Aspen member with a non-empty ARIO_CODE value exists on the external party
4. Test environment: Local / HQ test environment

**Step(s):**
1. Open a new check on the POS client on the horizontal desktop view
2. Click the function "Set Member"
3. Input the search criteria for the member with a non-empty ARIO_CODE value
4. Click the button "Enter"
5. If multiple members are returned, select the target member; if only one member is returned, go to step 6
6. Verify the system shows the member details page (tab "Basic Information")
7. Verify the system does NOT insert an "Owner Code" row at the end of the member details
8. Click the button "Set Member" and verify the member is set without the "Owner Code" stored in the check
9. Repeat the same flow on the vertical mobile view

**Test Result(s):**
1. The "Set Member" function triggers the Aspen member enquiry and returns the target member on both horizontal desktop view and vertical mobile view
2. The member details page (tab "Basic Information") shows the member details
3. The system does NOT insert an "Owner Code" row at the end of the member details — follows existing handling
4. After clicking "Set Member", the member is set without the "Owner Code" stored in the check
5. The behaviour is consistent when "Display Owner Code in Member Enquiry Result" is set to No or left empty

### F4.Regression — Aspen Membership And Reporting Flows

#### F4.1.Verify Existing Aspen Member Enquiry And Membership Reporting Are Preserved

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation (horizontal desktop view) and a vertical mobile device
2. An Aspen XML membership interface is configured with "Display Owner Code in Member Enquiry Result" = Yes
3. Test environment: Local / HQ test environment

**Step(s):**
1. Open a new check on the POS client, click "Set Member", perform an Aspen member enquiry and set the member (including the "Owner Code" stored in the check)
2. Order items, apply a payment and close the check
3. Open the Discount Report (ASPEN member) — CUS0356 — on the local server and verify the ASPEN member related redemption records are reflected correctly
4. Open the Menu Lookup Report By Shop and Status — CUS0304 — and the Item Listing Report (With Status) — CUS0353 — and verify the report data is consistent
5. Inspect the POS client and LS logs for any Aspen-membership-related errors

**Test Result(s):**
1. The Aspen member enquiry, set member (including "Owner Code" stored in the check), order, payment and close check flows complete successfully
2. The Discount Report (ASPEN member) — CUS0356 — reflects the ASPEN member related redemption records correctly — no regression is introduced by the Owner Code feature
3. The Menu Lookup Report By Shop and Status — CUS0304 — and the Item Listing Report (With Status) — CUS0353 — show consistent data — no regression is introduced
4. No Aspen-membership-related errors are observed in the POS client or LS logs

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-68888):**

- Available for Aspen membership interface; supports horizontal desktop view and vertical mobile view.
- System supports display of the Owner Code from the returned enquiry result in the member search list and details page of a member.
- Workflow:
  1. Open a new check
  2. Click function "Set Member"
  3. If setup "Display Owner Code in Member Enquiry Result" is empty or set to No, follows existing workflow (member search list shows "Member No." and "Name" only — no "Owner Code" column; member details page does not show an "Owner Code" row)
  4. If setup "Display Owner Code in Member Enquiry Result" is set to Yes, the system adds column "Owner Code" next to "Member No." in the member search list
  5. System adds column "Owner Code" next to "Member No." in the member search list
  6. Input search criteria for member search
  7. Click button "Enter"
  8. System triggers external party API for Aspen member enquiry
  9. If only one member is found, go to step 13
  10. If more than one member is found, system shows the returned member search list
  11. For each row in the list: if ARIO_CODE is non-empty, show the value in "Owner Code"; if ARIO_CODE is missing or empty, show blank string; system follows existing handling for "Member No." and "Name"
  12. Select target member
  13. System shows the member details page (tab "Basic Information")
  14. If ARIO_CODE is non-empty, system inserts a row "Owner Code" at the end of member details
  15. Click button "Set Member"
  16. System stores the member details including "Owner Code" in the check
- Setup: Go to Infrasys POS platform > Interface Control > Interfaces; configure Aspen XML membership interface with "Display Owner Code in Member Enquiry Result" = Yes / No.
