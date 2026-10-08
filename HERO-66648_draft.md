# HERO-66648 Test Report

## Functional Testing

### F1.Patron Inquiry - Display Photo & Signature First

#### F1.1.Verify Photo & Signature Tab Is Active First When Setup 1 = "Photo and Signature"

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface (vendor key: gems) is configured on the target outlet under Interface Control > Interfaces
3. Setup 1 (Default Tab) is configured as "Photo and signature" for the GEMS interface
4. A patron record with both a photo and signature image exists in the GEMS system and is reachable via the external API
5. User is logged in with credentials that have Patron Inquiry permission

**Step(s):**
1. Open an old check or a new check in POS
2. Click the "Patron Inquiry" function
3. Input a valid patron number
4. Click the "Enquiry" button to trigger the external party API
5. Observe the Patron Enquiry result screen
6. Verify the active first tab and the displayed photo and signature

**Test Result(s):**
1. The system triggers the external party API for enquiry as per existing workflow
2. The "Photo and signature" tab is shown as the active first tab on the Patron Enquiry result screen
3. Both the photo and signature images are clearly visible without requiring the cashier to navigate to another tab
4. The existing patron detail tabs (Basic Information, etc.) remain accessible in their original positions
5. The display works in both desktop horizontal view and mobile vertical view

#### F1.2.Verify Basic Information Tab Is Active First When Setup 1 = "Basic Information" or Does Not Exist

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is set as "Basic Information" (or the setup record does not exist)
4. A patron record exists in the GEMS system
5. User is logged in with credentials that have Patron Inquiry permission

**Step(s):**
1. Open an old check or a new check in POS
2. Click the "Patron Inquiry" function
3. Input a valid patron number
4. Click the "Enquiry" button
5. Observe the Patron Enquiry result screen

**Test Result(s):**
1. The system triggers the external party API for enquiry as per existing workflow
2. The "Basic information" tab is shown as the active first tab on the Patron Enquiry result screen
3. The existing patron detail tabs remain accessible in their original positions
4. The "Photo and signature" tab is available but is not the active first tab

#### F1.3.Verify "Photo and Signature Not Available" Message When No Photo/Signature Returned

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is configured as "Photo and signature"
4. A patron record exists in GEMS that does NOT contain a photo, a signature, or both
5. User is logged in with credentials that have Patron Inquiry permission

**Step(s):**
1. Open an old check or a new check in POS
2. Click the "Patron Inquiry" function
3. Input the patron number that has no photo and/or signature
4. Click the "Enquiry" button
5. Observe the Patron Enquiry result screen and the Photo and signature tab content

**Test Result(s):**
1. The "Photo and signature" tab is shown as the active first tab
2. Because no photo and/or signature is returned from the external party, the system displays the message "Photo and signature not available" on the "Photo and signature" tab
3. The patron details (name, tier, points, etc.) are still displayed and accessible on their respective tabs
4. The display works in both desktop horizontal view and mobile vertical view

### F2.Post Online Comp Payment - Display Photo & Signature First

#### F2.1.Verify Photo & Signature Tab Is Active First on Post Online Comp Payment Enquiry

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is configured as "Photo and signature"
4. A payment method is configured with Payment Type = "Post Online Comp" under POS System > Interface Configurations > Payment Settings, attached to the GEMS gaming interface
5. A printed check with a remaining total exists
6. A patron record with both a photo and signature image exists in GEMS

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Select the payment method with Payment Type = "Post Online Comp"
4. Input the patron number
5. Click the "Enquiry" button to trigger the external party API
6. Observe the enquiry result screen
7. Verify the active first tab and the displayed photo and signature

**Test Result(s):**
1. The system triggers the external party API for enquiry as per existing workflow
2. The "Photo and signature" tab is shown as the active first tab on the enquiry result screen
3. Both the photo and signature images are clearly visible without requiring navigation to another tab
4. The existing patron detail tabs remain accessible in their original positions
5. The display works in both desktop horizontal view and mobile vertical view

#### F2.2.Verify Basic Information Tab Is Active First on Post Online Comp Payment When Setup 1 = "Basic Information"

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is set as "Basic Information" (or the setup record does not exist)
4. A payment method with Payment Type = "Post Online Comp" is configured and attached to the GEMS gaming interface
5. A printed check with a remaining total exists
6. A patron record exists in GEMS

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Select the payment method with Payment Type = "Post Online Comp"
4. Input the patron number
5. Click the "Enquiry" button
6. Observe the enquiry result screen

**Test Result(s):**
1. The system triggers the external party API for enquiry as per existing workflow
2. The "Basic information" tab is shown as the active first tab on the enquiry result screen
3. The "Photo and signature" tab is available but is not the active first tab
4. The existing patron detail tabs remain accessible

### F3.Auto-Refresh on Patron Change

#### F3.1.Verify Enquiry Result Screen Auto-Refreshes When a Different Patron Is Selected

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is configured as "Photo and signature"
4. Two patron records (Patron A and Patron B) with different photo and signature images exist in GEMS

**Step(s):**
1. Open an old check or a new check in POS
2. Click the "Patron Inquiry" function
3. Input Patron A's patron number, click "Enquiry" and observe Patron A's photo and signature on the first tab
4. Without closing the enquiry result, input Patron B's patron number or swipe a different patron card
5. Click "Enquiry" for Patron B
6. Observe the refreshed Patron Enquiry result screen

**Test Result(s):**
1. The screen automatically refreshes to display Patron B's photo and signature on the first (active) tab
2. All patron detail fields update to reflect the newly selected Patron B
3. No residual data (photo, signature, name, tier, points) from Patron A remains visible
4. The display works in both desktop horizontal view and mobile vertical view

### F4.No Impact on Transaction Processing

#### F4.1.Verify Transaction Processing Is Unaffected by the New Setting

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GEMS gaming interface is configured on the target outlet
3. Setup 1 (Default Tab) is configured as "Photo and signature"
4. A patron record exists in GEMS
5. A check is open with menu items and discounts

**Step(s):**
1. Open the check in POS
2. Trigger a Patron Enquiry and dismiss the enquiry result screen
3. Add a new menu item to the order
4. Apply a discount
5. Process a void item / void check
6. Process a payment
7. Verify all transaction processing functions work correctly throughout

**Test Result(s):**
1. The Patron Enquiry result screen can be dismissed normally without blocking or delaying transaction workflows
2. Order entry, payment, void, comp and discount functions operate correctly with the new setting ON
3. All transaction processing is unaffected by the photo-and-signature display setting
4. No errors or unexpected behaviors are introduced by the new setting

## Compatibility Testing

### C1.Device and View Compatibility

#### C1.1.Verify Photo & Signature Display Works on Workstation, Mobile Devices and Both View Orientations

**Prerequisite(s):**
1. POS Client is installed on a Workstation (Windows 11 Enterprise), Hi20 (Android 13), iPad (OS 27), iPhone 15 (OS 27)
2. The GEMS gaming interface is configured on the target outlet with Setup 1 = "Photo and signature"
3. A patron record with both photo and signature exists in GEMS

**Step(s):**
1. On the Workstation, perform a Patron Inquiry and verify the Photo and signature tab is shown first in desktop horizontal view
2. On the Hi20 (Android 13), perform a Patron Inquiry in mobile vertical view and verify the Photo and signature tab is shown first
3. On the iPad (OS 27) and iPhone 15 (OS 27), perform a Patron Inquiry and verify the Photo and signature tab is shown first in mobile vertical view
4. Verify the "Not Available" placeholder also displays correctly on each device when no photo/signature is returned

**Test Result(s):**
1. The Photo and signature tab is shown as the active first tab on Workstation, Hi20, iPad and iPhone 15
2. The display works correctly in desktop horizontal view (Workstation) and mobile vertical view (Hi20, iPad, iPhone)
3. The "Not Available" placeholder is correctly displayed when no photo/signature is returned on each device type

### C2.Network Compatibility

#### C2.1.Verify Patron Inquiry Works Across Network Configurations

**Prerequisite(s):**
1. POS Client is installed and running with the GEMS gaming interface configured
2. POS backend and the GEMS external API are reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations

**Step(s):**
1. Connect the POS Client to Cloud network and perform a Patron Inquiry
2. Repeat under Local, OnPremise, Wireless and Wired networks
3. Verify the enquiry result screen displays correctly in each network configuration

**Test Result(s):**
1. The Patron Inquiry workflow functions correctly in Cloud, Local, OnPremise, Wireless and Wired network configurations
2. The Photo and signature tab is shown as the active first tab in each network configuration
3. No network-related compatibility issues are introduced by the new setting

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise), Hi20 (Android 13), iPad (OS 27), iPhone 15 (OS 27)
- **Component**: INTERFACE, POS
- **Issue Type**: Improvement
- **Interface**: GEMS gaming interface (vendor key: gems)
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **QC Tester**: Zephyr Ji (zephyr.ji@shijigroup.com)
- **QC Reviewer**: Kathy Kuang (kathy.kuang@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-66648)

User Story: As a Cashier, I want the patron's photo and signature to be displayed prominently on the Patron Enquiry result screen without navigating to another tab, so that I can visually verify the patron's identity before processing any transaction and reduce the risk of fraudulent or unauthorized activity.

Expected Behavior:

- A new toggle setting — "Display Photo & Signature First" — is added in the GEMS interface configuration
- When the toggle is ON, the Patron Enquiry result screen displays the patron's photo and signature on the first/active tab alongside patron details, without requiring the cashier to navigate
- When the photo and/or signature are not available in the GEMS response, a "Not Available" message is shown on the dedicated photo-and-signature tab
- When the toggle is OFF, the existing behavior remains completely unchanged
- The display automatically refreshes whenever a different patron account is selected or swiped
- This enhancement applies both when the cashier manually invokes the Patron Inquiry function key and when a patron card is swiped/tapped during check open or ordering
- Transaction processing (order entry, payment, voids, comps, etc.) is not affected

Acceptance Criteria:

- GEMS Interface Setting - Toggle ON: setting saved per outlet, takes effect for all stations in that outlet without restart, photo-and-signature tab is active first
- GEMS Interface Setting - Toggle OFF: Patron Enquiry result screen reverts to the existing default tab layout
- Display Photo and Signature First - Both Available: photo and signature clearly visible without navigation, existing patron detail tabs remain accessible
- Display Photo and Signature First - Photo or Signature Not Available: "Not Available" message displayed on the photo-and-signature tab, patron details still accessible
- Auto-Refresh on Patron Change: screen automatically refreshes to display the new patron's photo and signature
- No Impact on Transaction Processing: order entry, payment, void, comp, discount, refund functions operate correctly

Release Notes:

- Available for GEMS gaming interface (vendor key: gems)
- System supports display of the default tab of "Basic Information" or "Photo and Signature" first on the "Patron Inquiry" screen or "Post online comp" payment enquiry screen for both desktop horizontal view and mobile vertical view

Workflow for Patron Inquiry:

1. Open an old check or on a new check
2. Click function "Patron Inquiry"
3. Go to "Sub workflow of Enquiry"

Workflow for "Post Online Comp" payment:

1. Open an old check
2. Click function "paid"
3. Select a payment method with Payment Type "Post Online Comp"
4. Go to "Sub workflow of Enquiry"

Sub workflow of Enquiry:

1. Input the patron number
2. Click button "Enquiry"
3. System follows existing workflow to trigger external party API for enquiry
4. If Setup 1 does not exist or is set as "Basic information", the system shows "Basic information" tab first
5. If Setup 1 is set as "Photo and signature", the system shows "Photo and signature" tab first
   - If no photo and signature is returned from external party, the system displays the message "Photo and signature not available" in the "Photo and signature" tab
6. Continue to all existing workflow

Setup:

- Setup 1: Default Tab (Basic Information Default / Photo and signature) under Interface Control > Interfaces > GEMS gaming interface > Enquiry Display Settings
- Setup 2: Payment Type = "Post Online Comp" under POS System > Interface Configurations > Payment Settings
