# HERO-63345 Support to sync SLC Member Information from TMS to POS for auto Set Member

## Functional Testing

### F1.API Portal Send Check with SLC Member
#### F1.1.Send Check Request with SLC Member Number and Last Name
  **Prerequisite(s):**
    1. API Portal is configured and accessible
    2. SLC membership interface is enabled and configured
    3. External party Member Enquiry API and Fetch Profile API are available
    4. POS outlet is configured with SLC membership interface code
  **Step(s):**
    1. Send API Portal send check request with valid SLC member number and member last name
    2. Verify system processes send check request
    3. Verify system triggers external party Member Enquiry API with SLC member number and last name
    4. Verify system uses first member profile to trigger Fetch Profile API when member exists
    5. Verify SLC member is attached to the check successfully
  **Test Result(s):**
    1. Send check request is processed successfully
    2. Member Enquiry API is triggered with correct parameters
    3. Fetch Profile API is triggered with first member profile when enquiry returns success with member profiles
    4. SLC member is attached to the check
    5. API response does not contain "fail_to_attach_member" warning

#### F1.2.Send Check Request without SLC Interface Code
  **Prerequisite(s):**
    1. API Portal is configured and accessible
    2. POS outlet is configured without SLC membership interface code or with different interface code
  **Step(s):**
    1. Send API Portal send check request without interface code
    2. Send API Portal send check request with non-SLC interface code
    3. Verify system behavior for both scenarios
  **Test Result(s):**
    1. System skips SLC member attachment logic
    2. System continues remaining send check procedures
    3. Send check is completed successfully without member attachment

#### F1.3.Send Check with External Party Connection Error
  **Prerequisite(s):**
    1. API Portal is configured and accessible
    2. SLC membership interface is enabled
    3. External party Member Enquiry API is unavailable or returns error
  **Step(s):**
    1. Send API Portal send check request with valid SLC member number and last name
    2. Simulate connection problem with external party API
    3. Simulate external party returns error response
    4. Simulate invalid response from external party
    5. Verify system behavior and API response
  **Test Result(s):**
    1. System handles connection error gracefully
    2. System continues remaining send check procedures
    3. API response includes "fail_to_attach_member" as warning message
    4. Send check is completed successfully despite member attachment failure

#### F1.4.Send Check with Non-existent SLC Member
  **Prerequisite(s):**
    1. API Portal is configured and accessible
    2. SLC membership interface is enabled
    3. External party Member Enquiry API returns success with no member profile
  **Step(s):**
    1. Send API Portal send check request with SLC member number that does not exist
    2. Verify Member Enquiry API response
    3. Verify system behavior and API response
  **Test Result(s):**
    1. Member Enquiry API returns success with empty member profile list
    2. System skips Fetch Profile API call
    3. System continues remaining send check procedures
    4. API response includes "fail_to_attach_member" as warning message

### F2.API Portal Mark Arrival with SLC Member
#### F2.1.Mark Arrival Request Saves SLC Member Info
  **Prerequisite(s):**
    1. API Portal is configured and accessible
    2. SLC membership interface is enabled and configured
    3. POS outlet table exists for the reservation
  **Step(s):**
    1. Send API Portal mark arrival request with SLC member number and member last name
    2. Verify system saves interface ID into POS outlet table
    3. Verify system saves SLC member number and member last name into POS outlet table
    4. Verify existing handling for non-SLC interface is not affected
  **Test Result(s):**
    1. Mark arrival request is processed successfully
    2. Interface ID is saved to POS outlet table record
    3. SLC member number and member last name are saved to POS outlet table record
    4. Existing mark arrival workflow remains intact for non-SLC interfaces

### F3.POS Client Auto Attach SLC Member on Open Check
#### F3.1.Open Check with Mark Arrival Status and SLC Member
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Table has mark arrival status with SLC member information saved in POS outlet table
    3. SLC membership interface is configured for the outlet
    4. External party Member Enquiry API and Fetch Profile API are available
  **Step(s):**
    1. Click a table with mark arrival status at POS client
    2. Verify system checks POS outlet table saved membership interface ID
    3. Verify system checks SLC member number in POS outlet table record
    4. Verify system triggers external party Member Enquiry API with SLC member number
    5. Verify system triggers Fetch Profile API with first member profile
    6. Verify SLC member is attached to the check
    7. Verify system continues remaining procedures of opening a check
  **Test Result(s):**
    1. System detects SLC membership interface ID from POS outlet table
    2. System detects SLC member number from POS outlet table record
    3. Member Enquiry API is triggered with correct SLC member number
    4. Fetch Profile API is triggered with first member profile when enquiry returns success
    5. SLC member is attached to the check automatically
    6. Open check procedure continues normally

#### F3.2.Open Check with Non-SLC Membership Interface
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Table has mark arrival status with non-SLC membership interface ID saved in POS outlet table
  **Step(s):**
    1. Click a table with mark arrival status at POS client
    2. Verify system checks POS outlet table saved membership interface ID
    3. Verify system behavior when interface ID is not SLC
  **Test Result(s):**
    1. System detects non-SLC membership interface ID from POS outlet table
    2. System follows existing handling and ends SLC member attachment workflow
    3. Existing open check workflow continues normally

#### F3.3.Open Check without SLC Member Number in Outlet Table
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Table has mark arrival status with SLC membership interface ID but no SLC member number saved
  **Step(s):**
    1. Click a table with mark arrival status at POS client
    2. Verify system checks SLC member number in POS outlet table record
    3. Verify system behavior when SLC member number is absent
  **Test Result(s):**
    1. System detects SLC membership interface ID from POS outlet table
    2. System detects absence of SLC member number in POS outlet table record
    3. System skips external API calls and continues remaining procedures of opening a check

#### F3.4.Open Check with External Party API Failure
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Table has mark arrival status with SLC member information saved
    3. External party Member Enquiry API or Fetch Profile API is unavailable or returns error
  **Step(s):**
    1. Click a table with mark arrival status at POS client
    2. Simulate connection problem with external party API during Member Enquiry
    3. Simulate external party returns error during Member Enquiry
    4. Simulate invalid response from external party during Member Enquiry
    5. Simulate connection problem with Fetch Profile API
    6. Simulate external party returns error during Fetch Profile
    7. Verify system behavior and station log
  **Test Result(s):**
    1. System handles API failure gracefully
    2. System writes station log about fail to attach member
    3. System follows existing handling and continues remaining procedures of opening a check
    4. Open check procedure completes successfully despite member attachment failure

#### F3.5.Open Check with Non-existent SLC Member
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Table has mark arrival status with SLC member information saved
    3. External party Member Enquiry API returns success with no member profile
  **Step(s):**
    1. Click a table with mark arrival status at POS client
    2. Verify Member Enquiry API response with non-existent member
    3. Verify system behavior and station log
  **Test Result(s):**
    1. Member Enquiry API returns success with empty member profile list
    2. System skips Fetch Profile API call
    3. System writes station log about fail to attach member
    4. System continues remaining procedures of opening a check

### F4.SVC Enquiry Member Display
#### F4.1.SLC Member Display in SVC Enquiry
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Check has SLC member attached either via API Portal send check or mark arrival auto-attachment
    3. SLC membership interface is configured
  **Step(s):**
    1. Open a check with SLC member attached
    2. Click function "SVC Enquiry"
    3. Select target SLC membership interface
    4. Verify member display in member enquiry UI
  **Test Result(s):**
    1. SVC Enquiry function is accessible
    2. SLC membership interface is selectable
    3. Attached SLC member is displayed in member enquiry UI
    4. Member information matches the attached member profile

#### F4.2.SVC Enquiry without Attached SLC Member
  **Prerequisite(s):**
    1. POS client is logged in and operational
    2. Check does not have SLC member attached
    3. SLC membership interface is configured
  **Step(s):**
    1. Open a check without SLC member attached
    2. Click function "SVC Enquiry"
    3. Select target SLC membership interface
    4. Verify member enquiry UI behavior
  **Test Result(s):**
    1. SVC Enquiry function is accessible
    2. SLC membership interface is selectable
    3. System follows existing handling when no member is attached
    4. Member enquiry UI displays according to existing workflow

## Compatibility Testing

### C1.POS Client Compatibility
#### C1.1.Workstation Client Compatibility
  **Prerequisite(s):**
    1. Workstation with Windows 11 Enterprise is available
    2. POS client is installed and configured
    3. SLC membership interface is enabled
  **Step(s):**
    1. Test API Portal send check with SLC member on Workstation
    2. Test mark arrival with SLC member on Workstation
    3. Test POS client auto-attachment on Workstation
    4. Test SVC Enquiry on Workstation
  **Test Result(s):**
    1. All SLC member related functions work correctly on Workstation
    2. UI displays correctly on Workstation screen
    3. Performance is acceptable on Workstation

#### C1.2.Mobile Device Compatibility
  **Prerequisite(s):**
    1. Hi20 (Android 13) or iPad (OS 27) or iPhone 15 (OS 27) is available
    2. POS client is installed and configured
    3. SLC membership interface is enabled
  **Step(s):**
    1. Test API Portal send check with SLC member on mobile device
    2. Test mark arrival with SLC member on mobile device
    3. Test POS client auto-attachment on mobile device
    4. Test SVC Enquiry on mobile device
  **Test Result(s):**
    1. All SLC member related functions work correctly on mobile device
    2. UI displays correctly on mobile screen
    3. Touch interactions work correctly
    4. Performance is acceptable on mobile device

### C2.API Portal Protocol Compatibility
#### C2.1.HTTP and HTTPS Protocol Compatibility
  **Prerequisite(s):**
    1. API Portal is configured for both HTTP and HTTPS
    2. SLC membership interface is enabled
  **Step(s):**
    1. Send API Portal send check request via HTTP with SLC member info
    2. Send API Portal send check request via HTTPS with SLC member info
    3. Send API Portal mark arrival request via HTTP with SLC member info
    4. Send API Portal mark arrival request via HTTPS with SLC member info
  **Test Result(s):**
    1. All requests are processed successfully via HTTP
    2. All requests are processed successfully via HTTPS
    3. Member attachment logic works correctly for both protocols
    4. API responses are correct for both protocols

## Test Environment Information

### E1.Test Environment
#### E1.1.Local Test Environment
  **Prerequisite(s):**
    1. Local test environment is set up with POS, Platform, and TMS
    2. SLC membership interface is configured
    3. External party API sandbox is available
  **Step(s):**
    1. Verify local POS client installation
    2. Verify local Platform backend services
    3. Verify local TMS services
    4. Verify external API connectivity from local environment
  **Test Result(s):**
    1. POS client runs correctly in local environment
    2. Platform backend services are operational
    3. TMS services are operational
    4. External party APIs are accessible from local environment

### E2.HQ Test Environment
#### E2.1.HQ Test Environment Verification
  **Prerequisite(s):**
    1. HQ test environment is available
    2. SLC membership interface is configured in HQ environment
  **Step(s):**
    1. Verify HQ environment POS client functionality
    2. Verify HQ environment Platform backend services
    3. Verify HQ environment TMS services
    4. Perform integration testing in HQ environment
  **Test Result(s):**
    1. All SLC member features work correctly in HQ environment
    2. Integration between TMS, API Portal, and POS functions correctly
    3. Performance meets requirements in HQ environment

## Appendix

### A1.Release Notes Reference
  **Prerequisite(s):**
    1. Release notes document is available
  **Step(s):**
    1. Review release notes for HERO-63345
    2. Verify all described workflows are covered in test cases
    3. Verify setup requirements are documented
  **Test Result(s):**
    1. Send check workflow is fully tested
    2. Mark arrival workflow is fully tested
    3. POS client auto-attachment workflow is fully tested
    4. SVC Enquiry workflow is fully tested
    5. Error handling for all external API failures is tested
    6. Setup requirements are confirmed as "None"