# HERO-67276 Test Report

## Functional Testing

### F1.Default Amount Logic for GIG Mini Program Settle Voucher Payment

#### F1.1.Verify Default Amount Is Used When Greater Than Zero and Less Than or Equal to Remaining Total

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GIG Mini Program non-gaming interface (vendor key: gig_mini_program) is configured on the target outlet
3. The target payment method is attached with the GIG Mini Program non-gaming interface and the Payment Type is set as "Settle Voucher" under POS System > Interface Configurations > Payment Settings
4. A payment button for the target payment method is configured on the cashier display panel with a Default Amount greater than 0 (e.g. 50) under POS System > Station Setup > Display Panels
5. A printed check with a remaining total greater than or equal to the configured default amount exists

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap the configured GIG Mini Program Settle Voucher payment button
4. Observe whether the system prompts for a voucher amount
5. Observe the voucher QR code input prompt
6. Scan or manually input a valid voucher QR code
7. Trigger the external API for settle voucher and verify the saved payment amount

**Test Result(s):**
1. The system does NOT prompt for a voucher amount because the default amount is greater than 0
2. The system uses the configured default amount (e.g. 50) as the voucher amount
3. The system directly prompts for the voucher QR code
4. After the QR code is scanned/entered, the system triggers the external API for settle voucher with the default amount
5. The payment is saved with the configured default amount as the voucher amount

#### F1.2.Verify System Uses Remaining Total When Default Amount Is Greater Than Remaining Total

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GIG Mini Program non-gaming interface is configured on the target outlet
3. The target payment method is attached with the GIG Mini Program interface and the Payment Type is set as "Settle Voucher"
4. A payment button for the target payment method is configured with a Default Amount greater than 0 (e.g. 100) on the cashier display panel
5. A printed check exists with a remaining total LESS than the configured default amount (e.g. 80)

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap the configured GIG Mini Program Settle Voucher payment button
4. Observe whether the system prompts for a voucher amount
5. Observe the voucher QR code input prompt
6. Scan or manually input a valid voucher QR code
7. Trigger the external API for settle voucher and verify the saved payment amount

**Test Result(s):**
1. The system does NOT prompt for a voucher amount because the default amount is greater than 0
2. Because the default amount (100) is greater than the check remaining total (80), the system uses the remaining total (80) as the voucher amount
3. The system directly prompts for the voucher QR code
4. After the QR code is scanned/entered, the system triggers the external API for settle voucher with the remaining total as the voucher amount
5. The payment is saved with the remaining total as the voucher amount and the check is settled

#### F1.3.Verify System Prompts for Voucher Amount When Default Amount Is Zero or Not Configured

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GIG Mini Program non-gaming interface is configured on the target outlet
3. The target payment method is attached with the GIG Mini Program interface and the Payment Type is set as "Settle Voucher"
4. A payment button for the target payment method is configured with a Default Amount of 0 (or no Default Amount field configured) on the cashier display panel
5. A printed check with a remaining total exists

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap the configured GIG Mini Program Settle Voucher payment button
4. Observe whether the system prompts for a voucher amount
5. Input a valid voucher amount (less than or equal to the remaining total)
6. Click the "Enter" button
7. Observe the voucher QR code input prompt
8. Scan or manually input a valid voucher QR code
9. Trigger the external API for settle voucher and verify the saved payment amount

**Test Result(s):**
1. Because the default amount is 0 or not configured, the system prompts an input box for the voucher amount
2. The user is able to input the voucher amount manually
3. After clicking the "Enter" button, the system uses the manually entered voucher amount
4. The system prompts for the voucher QR code
5. After the QR code is scanned/entered, the system triggers the external API for settle voucher with the manually entered voucher amount
6. The payment is saved with the manually entered voucher amount

### F2.Voucher QR Code Handling

#### F2.1.Verify Voucher QR Code Scan and Manual Input

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GIG Mini Program non-gaming interface is configured on the target outlet with valid API connectivity to the external settle voucher API
3. A valid voucher QR code is available for scanning OR a valid voucher code value is available for manual input
4. A printed check with a remaining total exists

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap the configured GIG Mini Program Settle Voucher payment button (Default Amount = 0 to trigger amount prompt)
4. Input a valid voucher amount and click "Enter"
5. When the QR code input prompt appears, scan the QR code with the connected scanner
6. Observe the system triggering the external API
7. Repeat the flow with a manually entered voucher code value instead of scanning

**Test Result(s):**
1. The system successfully captures the scanned QR code
2. The system triggers the external API for settle voucher with the voucher code
3. The system saves the payment after the API returns a successful response
4. Manually entering the voucher code value also triggers the external API and saves the payment successfully

### F3.External API Trigger and Payment Saving

#### F3.1.Verify External API Is Triggered and Payment Is Saved Successfully

**Prerequisite(s):**
1. POS Client is installed and running on a Workstation (Windows 11 Enterprise) in the local test environment
2. The GIG Mini Program non-gaming interface is configured with valid API endpoint credentials
3. A printed check with a remaining total exists
4. A valid voucher QR code and amount are available

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap the GIG Mini Program Settle Voucher payment button
4. Provide the voucher amount (or rely on default amount) and the voucher QR code
5. Observe the system triggering the external API for settle voucher
6. Verify the payment is saved in the check after the API call

**Test Result(s):**
1. The system triggers the external settle voucher API with the correct payload (voucher amount + voucher code)
2. After the API returns a successful response, the system saves the payment in the check
3. The saved payment reflects the correct voucher amount
4. The check remaining total is reduced by the saved voucher amount

### F4.Regression Scope - Other Payment Methods and Display Panel Buttons

#### F4.1.Verify Other Payment Buttons and Payment Methods Remain Functional

**Prerequisite(s):**
1. POS Client with the fix/feature applied is installed and running on a Workstation (Windows 11 Enterprise)
2. Multiple payment methods configured on the cashier display panel including non-GIG Mini Program payment methods (e.g. cash, credit card) and GIG Mini Program with default amount = 0
3. A printed check with a remaining total exists

**Step(s):**
1. Open the printed check in POS
2. Click the "Paid" function to go to the cashier screen
3. Tap a non-GIG Mini Program payment method (e.g. cash) and verify it functions normally
4. Tap the GIG Mini Program payment button with default amount = 0 and verify the input prompt flow
5. Tap the GIG Mini Program payment button with default amount > 0 and verify the bypass flow
6. Verify no negative impact on other display panel buttons (e.g. discount, function keys)

**Test Result(s):**
1. Non-GIG Mini Program payment methods function normally without regression
2. The GIG Mini Program button with default amount = 0 continues to prompt for the voucher amount
3. The GIG Mini Program button with default amount > 0 bypasses the voucher amount prompt as expected
4. No negative impact on other display panel buttons is observed

## Compatibility Testing

### C1.Workstation and Network Compatibility

#### C1.1.Verify GIG Mini Program Default Amount Behavior Across Networks

**Prerequisite(s):**
1. POS Client is installed on a Workstation (Windows 11 Enterprise) and the GIG Mini Program interface is reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations
2. A payment button with default amount > 0 is configured for the GIG Mini Program payment method

**Step(s):**
1. Connect the POS Client to Cloud network and perform a GIG Mini Program Settle Voucher payment using the default amount flow
2. Repeat the test under Local, OnPremise, Wireless and Wired networks
3. Verify the external API is triggered and the payment is saved in each network configuration

**Test Result(s):**
1. The default amount flow works consistently across Cloud, Local, OnPremise, Wireless and Wired networks
2. The external settle voucher API is triggered successfully in each network configuration
3. The payment is saved correctly in each network configuration

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS
- **Issue Type**: New Feature
- **Interface**: GIG Mini Program non-gaming interface (vendor key: gig_mini_program)
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **QC Tester**: Zephyr Ji (zephyr.ji@shijigroup.com)
- **QC Reviewer**: Kathy Kuang (kathy.kuang@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-67276)

Expected Workflow:

- Set up a configurable default amount on the POS Display Panel (e.g., $50)
- User clicks the payment key associated with the Gigi Mini Program voucher interface
- System directly prompts for the Coupon Number (skipping the manual amount entry step)
- System automatically sends the payload containing the pre-set default amount (e.g., $50) alongside the entered coupon number to the Gigi Mini Program interface

Release Notes:

- It is available for GIG Mini Program non-gaming interface (vendor key: gig_mini_program)
- System supports to skip asking voucher amount if the default amount of the payment button of "Settle Voucher" is greater than 0

Workflow:

1. Open a printed check
2. Click function "Paid" to go to cashier
3. Select the payment method attached with GIG Mini Program non-gaming interface and payment type sets as "Settle Voucher"
4. If the default amount of the payment method button is greater than 0,
   - If the default amount > check remaining total, system uses the remaining total as the voucher amount and go to step 8
   - If the default amount <= check remaining total, the system uses the default amount as the voucher amount and go to step 8
5. If the default amount of the payment method button does not exist or is set as 0, the system prompts an input box for voucher amount
6. Input the voucher amount
7. Click button "Enter"
8. The system prompts an input box for voucher QR code
9. Scan the QR code or manually input the value
10. The system triggers the external API for settle voucher
11. The system saves the payment

Setup:

- Setup 1: Payment Type = Settle Voucher (POS System > Interface Configurations > Payment Settings)
- Setup 2: Default Amount = Target payment amount (POS System > Station Setup > Display Panels, cashier panel payment button)
