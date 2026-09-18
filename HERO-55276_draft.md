# HERO-55276 KDS2.0 - Order Handling - Change Cover and Change Table Test Report

## Functional Testing

### 1. Change Table Function Verification

#### 1.1. Display Original and Latest Table Number After POS Change Table

**Prerequisite(s):**
  1. KDS2.0 is installed and connected to POS system
  2. A check is opened on a table in POS client
  3. The check is sent to KDS and displayed on the expo screen

**Step(s):**
  1. In POS client, perform a Change Table operation for the open check (e.g., move from Table A to Table B)
  2. Navigate to KDS expo screen and locate the corresponding ticket
  3. Observe the table number information displayed on the ticket

**Test Result(s):**
  1. The ticket displays both the original table number and the latest table number
  2. The latest table number reflects the new table assigned in POS (Table B)
  3. The original table number remains visible for reference
  4. No errors are displayed on KDS

#### 1.2. Verify Table Number Update is Reflected in Real-Time

**Prerequisite(s):**
  1. KDS2.0 expo screen is open and displaying tickets
  2. A check is active on a table

**Step(s):**
  1. In POS client, change the table of the active check
  2. Immediately observe the KDS expo screen for the corresponding ticket

**Test Result(s):**
  1. The table number on the KDS ticket updates in real-time after the POS change
  2. The update occurs without requiring a manual refresh on KDS
  3. The latest table number is displayed correctly

### 2. Change Cover Function Verification

#### 2.1. Update Cover Directly When POS Changes Cover

**Prerequisite(s):**
  1. A check is opened on a table with an initial cover (number of guests) value
  2. The check is sent to KDS and displayed on the expo screen
  3. The initial cover value is visible on the KDS ticket

**Step(s):**
  1. In POS client, perform a Change Cover operation to update the number of guests (e.g., from 2 to 5)
  2. Navigate to KDS expo screen and locate the corresponding ticket
  3. Observe the cover (number of guests) value displayed on the ticket

**Test Result(s):**
  1. The cover value on the KDS ticket is updated directly to the latest value (5)
  2. The changed cover value is shown on the right side of the ticket
  3. The initial cover value is still displayed for reference
  4. No errors are displayed on KDS

#### 2.2. Verify Cover Update is Reflected in Real-Time

**Prerequisite(s):**
  1. KDS2.0 expo screen is open and displaying tickets with cover values
  2. A check is active with a known cover value

**Step(s):**
  1. In POS client, change the cover of the active check to a new value
  2. Immediately observe the KDS expo screen for the corresponding ticket

**Test Result(s):**
  1. The cover value on the KDS ticket updates in real-time after the POS change
  2. The latest cover value is displayed correctly on the right side
  3. No manual refresh is required on KDS

### 3. Change Table Extension Verification

#### 3.1. Display Table Extension Changes on KDS

**Prerequisite(s):**
  1. A check is opened on a table with a table extension
  2. The check is sent to KDS and displayed on the expo screen

**Step(s):**
  1. In POS client, change the table extension for the open check
  2. Navigate to KDS expo screen and locate the corresponding ticket
  3. Observe the table extension information displayed on the ticket

**Test Result(s):**
  1. The table extension update is reflected in the KDS ticket
  2. The initial table extension value is displayed
  3. The changed table extension value is shown on the right side
  4. No errors are displayed on KDS

### 4. Acceptance Criteria - Latest Check Info in Expo Screen

#### 4.1. Verify Kitchen Staff Sees Latest Check Information

**Prerequisite(s):**
  1. Multiple checks are active on different tables
  2. Some checks have undergone table, cover, or table extension changes

**Step(s):**
  1. Log in to KDS2.0 as a kitchen staff user
  2. Open the expo screen
  3. Review all displayed tickets and their table/cover information
  4. Compare the displayed information with the latest POS data

**Test Result(s):**
  1. All tickets on the expo screen display the latest check information
  2. Table numbers, cover values, and table extensions reflect the most recent POS changes
  3. Kitchen staff can identify the current table and guest count for each ticket
  4. No outdated or stale information is displayed

## Compatibility Testing

### 5. POS Client Compatibility

#### 5.1. Verify Change Table/Cover Sync Across Supported POS Clients

**Prerequisite(s):**
  1. KDS2.0 is connected to the POS system
  2. POS clients on supported platforms (Windows, Android, iOS) are available

**Step(s):**
  1. On a Windows POS client, open a check and change the table
  2. Verify the KDS ticket reflects the change
  3. On an Android POS client, open a check and change the cover
  4. Verify the KDS ticket reflects the change
  5. On an iOS POS client (if applicable), open a check and change the table extension
  6. Verify the KDS ticket reflects the change

**Test Result(s):**
  1. Changes from all supported POS client platforms are correctly synced to KDS
  2. Table number, cover, and table extension updates are displayed correctly regardless of the POS client platform
  3. No platform-specific synchronization errors occur

## Test Environment
- **Version**: v1.0.0
- **POS Client**: Windows, Android, iOS
- **KDS Version**: KDS2.0
- **Environment**: Local Test Environment, HQ Test Environment
