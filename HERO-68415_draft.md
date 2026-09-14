# Update external libraries of android client in 2026 Q4 Test Report

## Functional Testing

### 1.Android Client Startup And Library Verification

#### 1.1.Verify Client Launches Normally After Library Upgrade
**Prerequisite(s):**
1. Android client build with the upgraded libraries is installed on the POS device
2. The device is connected to the POS network

**Step(s):**
1. Launch the Android client on the POS device
2. Observe the startup process

**Test Result(s):**
1. The Android client launches successfully without crash
2. No dependency error or abnormal startup delay is observed

#### 1.2.Verify Upgraded Library Versions
**Prerequisite(s):**
1. Android client build with the upgraded libraries is installed

**Step(s):**
1. Check the library versions in the build artifacts
2. Compare with the target versions

**Test Result(s):**
1. The upgraded libraries match the following target versions:

| Library | Target Version |
| --- | --- |
| appcompat | 1.8.0 |
| commons-codec | 1.22.1 |
| jackson-core | 2.22.2 |
| jackson-annotations | 2.22 |
| jackson-databind | 2.22.2 |

2. No old library version remains in the build

### 2.UI And Core Flow Verification

#### 2.1.UI Rendering And Theme Display
**Prerequisite(s):**
1. Android client is running with appcompat 1.8.0

**Step(s):**
1. Navigate through the main pages including ordering panel, menu lookup and setting pages
2. Check the UI rendering, theme colors and control behavior

**Test Result(s):**
1. All pages are rendered correctly with consistent theme and layout
2. Buttons, dialogs and menus respond normally without display issues

#### 2.2.Login And Ordering Flow
**Prerequisite(s):**
1. Android client is running with the upgraded libraries
2. A valid user account is available

**Step(s):**
1. Login to the Android client
2. Open a check, add items and apply a discount
3. Complete the check with a payment

**Test Result(s):**
1. Login succeeds and the user enters the ordering panel
2. Items, discounts and payments work correctly and the check is completed

### 3.Data Processing And Communication

#### 3.1.JSON Parsing For Backend Communication
**Prerequisite(s):**
1. Android client is running with jackson-core 2.22.2 and jackson-databind 2.22.2
2. The backend service is reachable

**Step(s):**
1. Perform operations that exchange JSON data with the backend such as member enquiry and check sync
2. Check the parsed data on the client

**Test Result(s):**
1. JSON request and response are serialized and parsed correctly without errors
2. Data displayed on the client matches the backend data

#### 3.2.Encoding And Digest Operations With Commons Codec
**Prerequisite(s):**
1. Android client is running with commons-codec 1.22.1
2. A function involving encoding or digest calculation is available

**Step(s):**
1. Trigger the function that performs encoding or digest calculation
2. Verify the calculated value against the expected result

**Test Result(s):**
1. The encoding and digest operations produce correct results
2. The related function works end to end without errors
