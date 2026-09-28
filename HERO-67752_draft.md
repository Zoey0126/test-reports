# HERO-67752 Test Report - Add Receipt Printing Options for Self Order Kiosk Mode in Config by Location

## Functional Testing

### F1.Verify New Config Entry Display and Setup in Config by Location
#### F1.1.Display New Config Entry in Config by Location
  **Prerequisite(s):**
    1. User is logged in with admin privileges
    2. POS System module is accessible
    3. Config by Location setup page is accessible
  **Step(s):**
    1. Navigate to POS System -> Ordering Setup -> Config by Location
    2. Check the list of available configuration entries
    3. Verify "Do Not Print Receipt If Self Order Kiosk Mode" entry is displayed
  **Test Result(s):**
    1. The new config entry "Do Not Print Receipt If Self Order Kiosk Mode" is visible in the list
    2. The entry is placed logically with other receipt-related configuration entries
    3. No UI errors or display issues occur

#### F1.2.Add New Config Record with Apply To = All Locations
  **Prerequisite(s):**
    1. User is logged in with admin privileges
    2. "Do Not Print Receipt If Self Order Kiosk Mode" config entry is accessible
  **Step(s):**
    1. Select setup "Do Not Print Receipt If Self Order Kiosk Mode"
    2. Click "Add New" to add a record
    3. Select Apply To = All Locations
    4. Select Value = Allow to print
    5. Click "Save" to save the record
  **Test Result(s):**
    1. New record is saved successfully
    2. Record is applied to all locations
    3. Success notification is displayed
    4. Saved record is displayed in the list

#### F1.3.Add New Config Record with Apply To = Shop
  **Prerequisite(s):**
    1. User is logged in with admin privileges
    2. Target shop exists in the system
  **Step(s):**
    1. Select setup "Do Not Print Receipt If Self Order Kiosk Mode"
    2. Click "Add New" to add a record
    3. Select Apply To = Shop
    4. Select Target shop from Shop dropdown
    5. Select Value = Do not allow to print
    6. Click "Save" to save the record
  **Test Result(s):**
    1. New record is saved successfully
    2. Record is applied to the selected shop only
    3. Shop dropdown is enabled and populated correctly when Apply To = Shop is selected
    4. Success notification is displayed

#### F1.4.Add New Config Record with Apply To = Outlet
  **Prerequisite(s):**
    1. User is logged in with admin privileges
    2. Target outlet exists in the system
  **Step(s):**
    1. Select setup "Do Not Print Receipt If Self Order Kiosk Mode"
    2. Click "Add New" to add a record
    3. Select Apply To = Outlet
    4. Select Target outlet from Outlet dropdown
    5. Select Value = Ask option to print receipt
    6. Click "Save" to save the record
  **Test Result(s):**
    1. New record is saved successfully
    2. Record is applied to the selected outlet only
    3. Outlet dropdown is enabled and populated correctly when Apply To = Outlet is selected
    4. Success notification is displayed

#### F1.5.Add New Config Record with Apply To = Station
  **Prerequisite(s):**
    1. User is logged in with admin privileges
    2. Target station exists in the system
  **Step(s):**
    1. Select setup "Do Not Print Receipt If Self Order Kiosk Mode"
    2. Click "Add New" to add a record
    3. Select Apply To = Station
    4. Select Target station from Station dropdown
    5. Select Value = Send email receipt only
    6. Click "Save" to save the record
  **Test Result(s):**
    1. New record is saved successfully
    2. Record is applied to the selected station only
    3. Station dropdown is enabled and populated correctly when Apply To = Station is selected
    4. Success notification is displayed

### F2.Verify Receipt Printing Behavior in Self Order Kiosk Mode
#### F2.1.Allow to Print Option Behavior
  **Prerequisite(s):**
    1. Config by Location has "Do Not Print Receipt If Self Order Kiosk Mode" set to "Allow to print" for the target location/station
    2. Self Order Kiosk mode is enabled and operational
    3. Items are configured for ordering
  **Step(s):**
    1. Open a new check on Self-order Kiosk mode
    2. Order any items
    3. Click function "Paid" to go to the cashier screen
    4. Select a payment method
    5. Input the payment amount
    6. Click button "Enter"
  **Test Result(s):**
    1. System processes payment successfully
    2. Receipt is printed automatically according to the precedence of receipt-related setups
    3. No errors occur during the payment and receipt printing process

#### F2.2.Do Not Allow to Print Option Behavior
  **Prerequisite(s):**
    1. Config by Location has "Do Not Print Receipt If Self Order Kiosk Mode" set to "Do not allow to print" for the target location/station
    2. Self Order Kiosk mode is enabled and operational
  **Step(s):**
    1. Open a new check on Self-order Kiosk mode
    2. Order any items
    3. Click function "Paid" to go to the cashier screen
    4. Select a payment method
    5. Input the payment amount
    6. Click button "Enter"
  **Test Result(s):**
    1. System processes payment successfully
    2. Physical receipt is not printed
    3. No receipt printing prompts appear

#### F2.3.Ask Option to Print Receipt Behavior
  **Prerequisite(s):**
    1. Config by Location has "Do Not Print Receipt If Self Order Kiosk Mode" set to "Ask option to print receipt" for the target location/station
    2. Self Order Kiosk mode is enabled and operational
  **Step(s):**
    1. Open a new check on Self-order Kiosk mode
    2. Order any items
    3. Click function "Paid" to go to the cashier screen
    4. Select a payment method
    5. Input the payment amount
    6. Click button "Enter"
  **Test Result(s):**
    1. System processes payment successfully
    2. A prompt appears asking whether to print receipt
    3. User can select to print or not print
    4. System behaves according to user's selection

#### F2.4.Send Email Receipt Only Option Behavior
  **Prerequisite(s):**
    1. Config by Location has "Do Not Print Receipt If Self Order Kiosk Mode" set to "Send email receipt only" for the target location/station
    2. Self Order Kiosk mode is enabled and operational
    3. Customer email is available or can be entered
  **Step(s):**
    1. Open a new check on Self-order Kiosk mode
    2. Order any items
    3. Click function "Paid" to go to the cashier screen
    4. Select a payment method
    5. Input the payment amount
    6. Click button "Enter"
  **Test Result(s):**
    1. System processes payment successfully
    2. Physical receipt is not printed
    3. Email receipt is triggered or prompt for email input appears
    4. Email receipt is sent successfully if email is provided

### F3.Verify Receipt Precedence and Existing Configurations Impact
#### F3.1.Receipt Handling Precedence Verification
  **Prerequisite(s):**
    1. Multiple receipt-related setups are configured (Payment Methods Receipt Type, Config by Location Tip And Receipt Handling, Payment button Receipt Type, POS Functions Toggle Print Receipt, Config by Location Do Not Print Receipt If Self Order Kiosk Mode)
    2. Self Order Kiosk mode is enabled and operational
  **Step(s):**
    1. Open a new check on Self-order Kiosk mode
    2. Order any items
    3. Click function "Paid" to go to the cashier screen
    4. Select a payment method with specific receipt type
    5. Input the payment amount
    6. Click button "Enter"
  **Test Result(s):**
    1. System handles receipt according to the defined precedence:
       1. Payment Methods Receipt Type
       2. Config by Location Tip And Receipt Handling Receipt option selected by user
       3. Payment button in display panel page Receipt Type
       4. POS Functions Toggle Print Receipt and Toggle Email Receipt
       5. Config by Location Do Not Print Receipt If Self Order Kiosk Mode
    2. Higher precedence settings override lower precedence settings correctly

#### F3.2.No Impact on Existing Configurations
  **Prerequisite(s):**
    1. Existing configurations for Fine Dining, Fast Food, and Bar Modes are already set up
    2. "Do Not Print Receipt If Self Order Kiosk Mode" is configured
  **Step(s):**
    1. Verify receipt printing configurations for Fine Dining mode remain unchanged
    2. Verify receipt printing configurations for Fast Food mode remain unchanged
    3. Verify receipt printing configurations for Bar Mode remain unchanged
    4. Perform payment operations in Fine Dining, Fast Food, and Bar Modes
  **Test Result(s):**
    1. Existing configurations for Fine Dining, Fast Food, and Bar Modes are not modified
    2. Receipt printing behavior in Fine Dining mode remains as before
    3. Receipt printing behavior in Fast Food mode remains as before
    4. Receipt printing behavior in Bar Mode remains as before
    5. No cross-contamination of configurations between modes

### F4.Verify Exclusion of Simple Ordering Kiosk Mode Sub Type
#### F4.1.Simple Ordering Kiosk Mode Not Covered
  **Prerequisite(s):**
    1. Ordering Mode Sub Type "Simple Ordering Kiosk Mode" exists under Ordering Mode "Self-order Kiosk"
    2. "Do Not Print Receipt If Self Order Kiosk Mode" is configured
  **Step(s):**
    1. Navigate to Ordering Mode setup
    2. Verify "Simple Ordering Kiosk Mode" sub type exists
    3. Perform ordering operation in "Simple Ordering Kiosk Mode"
    4. Complete payment in "Simple Ordering Kiosk Mode"
  **Test Result(s):**
    1. "Simple Ordering Kiosk Mode" sub type remains available
    2. Config by Location "Do Not Print Receipt If Self Order Kiosk Mode" does not apply to "Simple Ordering Kiosk Mode"
    3. Receipt behavior in "Simple Ordering Kiosk Mode" remains unchanged and independent of the new config

## Compatibility Testing

### C1.Workstation Compatibility
#### C1.1.Workstation Basic Functionality
  **Prerequisite(s):**
    1. Workstation with Windows 11 Enterprise is available
    2. Infrasys POS Client is installed and operational
    3. Config by Location setup page is accessible
  **Step(s):**
    1. Log in to Infrasys POS Client on Workstation
    2. Navigate to Config by Location
    3. Add and save "Do Not Print Receipt If Self Order Kiosk Mode" configuration
    4. Perform Self Order Kiosk mode ordering and payment
  **Test Result(s):**
    1. Config by Location page loads correctly
    2. New config entry is displayed and editable
    3. Configuration is saved successfully
    4. Receipt behavior in Self Order Kiosk mode follows the configured setting
    5. No UI rendering issues on Workstation

### C2.Mobile Device Compatibility
#### C2.1.Hi20 Android Device Basic Functionality
  **Prerequisite(s):**
    1. Hi20 device with Android 13 is available
    2. Infrasys POS Mobile Client is installed and operational
    3. Config by Location setup is already configured on backend
  **Step(s):**
    1. Log in to Infrasys POS Mobile Client on Hi20
    2. Perform Self Order Kiosk mode ordering and payment
  **Test Result(s):**
    1. Self Order Kiosk mode functions correctly on Hi20
    2. Receipt behavior follows the configured setting
    3. No device-specific issues occur

#### C2.2.iPad Device Basic Functionality
  **Prerequisite(s):**
    1. iPad with OS 27 is available
    2. Infrasys POS Mobile Client is installed and operational
  **Step(s):**
    1. Log in to Infrasys POS Mobile Client on iPad
    2. Perform Self Order Kiosk mode ordering and payment
  **Test Result(s):**
    1. Self Order Kiosk mode functions correctly on iPad
    2. Receipt behavior follows the configured setting
    3. No device-specific issues occur

## Test Environment Information

- Local Test Environment
- HQ Test Environment

## Appendix

- The "Config by Location" menu includes a new entry "Do not print receipt if Self Order Kiosk Mode"
- The behavior of these options in Self Order Kiosk Mode matches their current implementation in Fine Dining, Fast Food and Bar Modes
- No existing configurations are impacted by the implementation of this feature
