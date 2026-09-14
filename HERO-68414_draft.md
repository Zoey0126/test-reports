# Update external libraries of window client in 2026 Q4 Test Report

## Functional Testing

### 1.Windows Client Startup And Library Verification

#### 1.1.Verify Client Launches Normally After Library Upgrade
**Prerequisite(s):**
1. Windows client build with the upgraded libraries is installed on the POS station
2. The station is connected to the POS network

**Step(s):**
1. Launch the Windows client on the POS station
2. Observe the startup process

**Test Result(s):**
1. The Windows client launches successfully without crash
2. No dependency error or abnormal startup delay is observed

#### 1.2.Verify Upgraded Library Versions
**Prerequisite(s):**
1. Windows client build with the upgraded libraries is installed

**Step(s):**
1. Check the library versions in the build artifacts
2. Compare with the target versions

**Test Result(s):**
1. The upgraded libraries match the following target versions:

| Library | Target Version |
| --- | --- |
| jackson-core | 2.22.2 |
| jackson-databind | 2.22.2 |
| bcprov-jdk18on | 1.85.2 |
| json | 20260814 |
| jna | 5.19.1 |
| jna-platform | 5.19.1 |
| commons-codec | 1.22.1 |
| commons-logging | 1.4.0 |

2. No old library version remains in the build

### 2.Core Flow And Data Exchange

#### 2.1.Login And Ordering Flow
**Prerequisite(s):**
1. Windows client is running with the upgraded libraries
2. A valid user account is available

**Step(s):**
1. Login to the Windows client
2. Open a check, add items and apply a discount
3. Complete the check with a payment

**Test Result(s):**
1. Login succeeds and the user enters the ordering panel
2. Items, discounts and payments work correctly and the check is completed

#### 2.2.JSON Data Parsing And Backend Communication
**Prerequisite(s):**
1. Windows client is running with jackson-core 2.22.2, jackson-databind 2.22.2 and json 20260814
2. The backend service is reachable

**Step(s):**
1. Perform operations that exchange JSON data with the backend such as check sync and member enquiry
2. Check the parsed data on the client

**Test Result(s):**
1. JSON request and response are serialized and parsed correctly without errors
2. Data displayed on the client matches the backend data

### 3.Security And Native Access

#### 3.1.Encryption Operations With Upgraded Bouncy Castle
**Prerequisite(s):**
1. Windows client is running with bcprov-jdk18on 1.85.2
2. A function involving encryption or decryption is available

**Step(s):**
1. Trigger the function that performs encryption or decryption
2. Verify the encrypted output can be decrypted back to the original value

**Test Result(s):**
1. The encryption and decryption operations succeed without provider errors
2. The related function works end to end with correct data

#### 3.2.Native Library Access Via JNA
**Prerequisite(s):**
1. Windows client is running with jna 5.19.1 and jna-platform 5.19.1
2. A function relying on native library calls is available

**Step(s):**
1. Trigger the function that relies on native library access
2. Check the function behavior and any related device or system interaction

**Test Result(s):**
1. The native library is loaded successfully through JNA
2. The related function operates normally without native call errors

### 4.Logging And Encoding Utilities
**Prerequisite(s):**
1. Windows client is running with commons-logging 1.4.0 and commons-codec 1.22.1

**Step(s):**
1. Perform several client operations to generate logs
2. Check the log output and any function using encoding utilities

**Test Result(s):**
1. Client logs are written correctly with the expected level and format
2. Encoding utilities produce correct results and no logging error is observed
