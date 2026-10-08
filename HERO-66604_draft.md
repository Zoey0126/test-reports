# HERO-66604 Test Report

## Functional Testing

### F1.Payment Member Extra Information Print Variable - Basic

#### F1.1.Verify Payment Member Extra Info Prints on Guest Check When Payment Tender Has a Linked Member Profile

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled on the target outlet
3. A print format for the guest check includes the new "Payment Member Extra Infos (PaymentMemberExtraInfos)" printing loop placed inside the payments loop, with the printing variables MemberExtraInfoKey and MemberExtraInfoValue configured under POS System > Printing Setup > Print Formats
4. A check member with extra information fields configured exists in the system
5. A payment tender with a different linked member profile that has extra information fields configured (e.g. membership stored-value card, loyalty payment) is available

**Step(s):**
1. Open a new check at a table and assign a check member with extra information fields
2. Add menu items to the check
3. Print the guest check
4. Verify the check member's extra information appears via the existing "MemberExtraInfoValue" print variable
5. Go to the cashier screen, pay using the payment tender that has the linked member profile
6. Reprint the guest check and verify the payment member's extra information appears via the new print variable

**Test Result(s):**
1. The guest check prints successfully with the check member's extra information fields via "MemberExtraInfoValue"
2. After applying a payment tender with a linked member profile, the reprint of the guest check displays the payment member's extra information via the new print variable within the PaymentMemberExtraInfos loop
3. Both the check member and the payment member extra information are visible on the same receipt
4. The output format of the new print variable matches the existing "MemberExtraInfoValue" format — each extra information field is a key-value pair

#### F1.2.Verify Payment Member Extra Info Prints on Payment Receipt

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled on the target outlet
3. A print format for the payment receipt includes the new "Payment Member Extra Infos (PaymentMemberExtraInfos)" printing loop placed inside the payments loop with the printing variables MemberExtraInfoKey and MemberExtraInfoValue configured
4. A check member with extra information fields configured exists in the system
5. A payment tender with a different linked member profile that has extra information fields configured is available

**Step(s):**
1. Open a new check at a table and assign a check member with extra information fields
2. Add menu items to the check
3. Go to the cashier screen, pay using the payment tender that has the linked member profile
4. Print the payment receipt
5. Verify both the check member's extra information (via "MemberExtraInfoValue") and the payment member's extra information (via the new print variable) appear on the receipt

**Test Result(s):**
1. The payment receipt prints successfully with the check member's extra information fields via "MemberExtraInfoValue"
2. The payment member's extra information is printed via the new print variable within the PaymentMemberExtraInfos loop
3. Both the check member and the payment member extra information are visible on the same receipt
4. The output format of the new print variable matches the existing "MemberExtraInfoValue" format

### F2.Edge Cases - Missing Data Scenarios

#### F2.1.Verify Empty Output When Payment Member Has No Extra Information Configured

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. Print formats for guest check and receipt include the new "PaymentMemberExtraInfos" loop with MemberExtraInfoKey/MemberExtraInfoValue variables
4. A check member WITH extra information fields configured exists
5. A payment tender with a linked member profile that has NO extra information fields configured is available

**Step(s):**
1. Open a new check at a table and assign the check member
2. Add menu items to the check
3. Pay using the payment tender whose linked member has NO extra information fields
4. Print the guest check and the payment receipt
5. Verify the payment member extra information output and the check member extra information output

**Test Result(s):**
1. The new print variable outputs an empty string for the payment member extra information (because no extra info is configured for the payment member)
2. The check member's extra information still prints normally via "MemberExtraInfoValue"
3. The receipt prints without errors

#### F2.2.Verify Empty Output for Check Member When Only Payment Member Has Extra Information

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. Print formats for guest check and receipt include the new "PaymentMemberExtraInfos" loop with MemberExtraInfoKey/MemberExtraInfoValue variables
4. A check member with NO extra information fields configured exists
5. A payment tender with a linked member profile that HAS extra information fields configured is available

**Step(s):**
1. Open a new check at a table and assign the check member with NO extra information fields
2. Add menu items to the check
3. Pay using the payment tender whose linked member HAS extra information fields
4. Print the guest check and the payment receipt
5. Verify the check member and payment member extra information outputs

**Test Result(s):**
1. The existing "MemberExtraInfoValue" outputs an empty string for the check member extra information (because no extra info is configured for the check member)
2. The new print variable prints the payment member's extra information correctly
3. The receipt prints without errors

#### F2.3.Verify No Errors When Payment Tender Has No Linked Member Profile

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. Print formats for guest check and receipt include the new "PaymentMemberExtraInfos" loop with MemberExtraInfoKey/MemberExtraInfoValue variables
4. A check member with extra information fields configured exists
5. A payment tender with NO linked member profile (e.g. cash, generic credit card, voucher not linked to a member) is available

**Step(s):**
1. Open a new check at a table and assign the check member
2. Add menu items to the check
3. Pay using the payment tender that has NO linked member profile
4. Print the guest check and the payment receipt
5. Verify the payment member extra information output and the check member extra information output

**Test Result(s):**
1. The new print variable outputs an empty string for the payment member extra information (because no member profile is linked to the payment tender)
2. The check member's extra information still prints normally via "MemberExtraInfoValue" if a check member is set
3. If no check member is set, both variables output empty strings and the receipt prints without errors

### F3.Multiple Payment Members on a Split-Payment Check

#### F3.1.Verify New Print Variable Outputs Extra Information for Each Payment Member in Order

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. Print formats for guest check and receipt include the new "PaymentMemberExtraInfos" loop with MemberExtraInfoKey/MemberExtraInfoValue variables
4. A check member with extra information fields configured exists
5. Two different payment tenders, each with a different linked member profile and each member having extra information fields configured, are available

**Step(s):**
1. Open a new check at a table and assign the check member
2. Add menu items to the check
3. Split-pay the check using the two different payment tenders in a specific order (e.g. tender A first, then tender B)
4. Print the guest check and the payment receipt
5. Verify the order of the payment members' extra information in the printed output

**Test Result(s):**
1. The new print variable outputs the extra information for each payment member in the order their payments were applied
2. The check member's extra information is printed once via "MemberExtraInfoValue"
3. Both payment members' extra information appears on the same receipt
4. The output format of the new print variable matches the existing "MemberExtraInfoValue" format for each payment member

### F4.Print Format Setup and Variable Resolution

#### F4.1.Verify New Print Variable Resolves Correctly Within the PaymentMemberExtraInfos Loop

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. A receipt template includes the new print variable placed within the "PaymentMemberExtraInfos" loop (which itself is placed inside the payments loop)
4. A payment tender with a linked member profile that has extra information fields configured is available

**Step(s):**
1. Open a new check at a table, add items and pay using the payment tender
2. Print the receipt
3. Verify the new print variable resolves correctly to the payment member's extra information fields within each iteration of the PaymentMemberExtraInfos loop
4. Verify the loop iterates once per payment member per payment

**Test Result(s):**
1. The new print variable correctly resolves to the payment member's extra information fields within the PaymentMemberExtraInfos loop iteration
2. The PaymentMemberExtraInfos loop iterates once per payment member per payment
3. The output format of the new print variable matches the existing "MemberExtraInfoValue" format

#### F4.2.Verify Receipt Template Without the New Print Variable Still Prints Normally

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The internal member module is enabled
3. A receipt template does NOT include the new "PaymentMemberExtraInfos" loop or the new print variable
4. A check member with extra information fields configured exists
5. A payment tender with a linked member profile that has extra information fields configured is available

**Step(s):**
1. Open a new check at a table, assign the check member, add items and pay using the payment tender
2. Print the receipt using the template that does NOT include the new print variable
3. Verify only the check member's extra information is printed

**Test Result(s):**
1. The receipt prints successfully
2. Only the check member's extra information appears via "MemberExtraInfoValue" (because the new print variable is not included in the template)
3. The payment member's extra information is absent from the printed receipt
4. No errors occur when the new print variable is not included in the template

## Compatibility Testing

### C1.Workstation and Network Compatibility

#### C1.1.Verify New Print Variable Works Across Networks

**Prerequisite(s):**
1. POS Client is installed on a Workstation (Windows 11 Enterprise)
2. The internal member module is enabled and POS backend is reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations
3. Print formats with the new PaymentMemberExtraInfos loop are configured

**Step(s):**
1. Connect the POS Client to Cloud network and verify the new print variable outputs the payment member's extra information
2. Repeat under Local, OnPremise, Wireless and Wired networks

**Test Result(s):**
1. The new print variable works correctly across all network configurations
2. The payment member's extra information is printed correctly in each network configuration

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS
- **Issue Type**: Improvement
- **Module**: Internal member module, Print Format
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **QC Tester**: Forrest Lin (forrest.lin@shijigroup.com)
- **QC Reviewer**: Viktor Peng (viktor.peng@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-66604)

User Story: As a POS operator, I want the guest check and payment receipt to display extra information for the payment member alongside the check member so that both members' details are visible on the same receipt when a member profile is linked to the payment tender.

Expected Behavior:

- A new print variable (following the existing "MemberExtraInfoValue" naming convention) is added to capture the extra information of the payment member
- When a payment tender has a member profile linked, both the guest check and payment receipt shall display extra information for both the check member (via the existing "MemberExtraInfoValue" variable) and the payment member (via the new variable)
- The new variable must resolve correctly within the existing "CheckMemberExtrainfos" loop context, which already iterates over all members (check and payment)
- The new variable outputs the same data format and structure as "MemberExtraInfoValue" (key-value pairs for each configured extra information field)

Acceptance Criteria:

- Payment member extra information prints alongside check member extra information
- New variable resolves correctly within the CheckMemberExtrainfos loop
- New variable outputs the same data format as MemberExtraInfoValue
- No output when payment tender has no linked member profile
- Multiple payment members on a split-payment check: new print variable outputs the extra information for each payment member in the order their payments were applied

Release Notes:

- Available for internal member module
- System supports the following new printing loop for printing internal member extra information on guest check and receipt
- The new printing loop needs to be placed inside the payments loop: Payment Member Extra Infos (PaymentMemberExtraInfos)
- Printing variable (existing): MemberExtraInfoKey, MemberExtraInfoValue

Setup:

- Add printing loop for "Payment Member Extra Infos (PaymentMemberExtraInfos)" under POS System > Printing Setup > Print Formats (Type = Loop, Key = PaymentMemberExtraInfos)
- Add printing variables: MemberExtraInfoKey (Loop belonged to: PaymentMemberExtraInfos, Available Slip: Check/Receipt, Description: Member extra info key), MemberExtraInfoValue (Loop belonged to: PaymentMemberExtraInfos, Available Slip: Check/Receipt, Description: Member extra info value)
