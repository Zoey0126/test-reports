# POS Feature - Enable "Change Cover" on mobile/ workstation view (Dev) Test Report

## Functional Testing

### 1.Change Cover Function Key Display

#### 1.1.Change Cover function key display in horizontal desktop view

**Prerequisite(s):**
1. User is logged in to the Infrasys POS workstation with access to ordering functions
2. The Change Cover function key is placed in the general function key layout
3. A check with at least one ordered item is open

**Step(s):**
1. Open the check on the horizontal desktop view in day mode
2. Navigate to the function key page where Change Cover is placed
3. Switch the theme to night mode and repeat step 2

**Test Result(s):**
1. The Change Cover function key is displayed in the general function key layout in both day mode and night mode
2. The function key renders with the new day and night theme correctly and can be tapped in both themes

#### 1.2.Change Cover function key display in vertical mobile view

**Prerequisite(s):**
1. User is logged in to the Infrasys POS mobile view with access to ordering functions
2. The Change Cover function key is placed in the general function key layout
3. A check with at least one ordered item is open

**Step(s):**
1. Open the check on the vertical mobile view in day mode
2. Navigate to the function key page where Change Cover is placed
3. Switch the theme to night mode and repeat step 2

**Test Result(s):**
1. The Change Cover function key is displayed in the vertical mobile view layout in both day mode and night mode
2. The function key layout fits the vertical view without truncation or overlapping elements in both themes

### 2.Change Cover Operation

#### 2.1.Change cover to a valid cover count on an open check

**Prerequisite(s):**
1. Config by Location > Pay Check Auto Functions > Change Cover (Ask cover before payment) is configured for the target location
2. A check is open with cover count set to 2 and at least one ordered item

**Step(s):**
1. Open the check in the workstation view
2. Tap the Change Cover function key
3. Enter 4 as the new cover count
4. Confirm the change

**Test Result(s):**
1. The cover count of the check is updated from 2 to 4 successfully
2. The new cover count is displayed correctly on the check in both horizontal desktop view and vertical mobile view

#### 2.2.Change cover rejected when zero is entered with "not allow 0" configuration

**Prerequisite(s):**
1. Config by Location > Pay Check Auto Functions > Change Cover (Ask cover before payment and not allow 0) is enabled for the target location
2. A check is open with a valid cover count

**Step(s):**
1. Open the check and tap the Change Cover function key
2. Enter 0 as the new cover count
3. Confirm the change

**Test Result(s):**
1. System rejects the input and prompts an alert that zero is not allowed
2. The original cover count is retained on the check and no change is applied

#### 2.3.Change cover with children cover input allowed

**Prerequisite(s):**
1. Config by Location > Allow Children Cover Input is set to Yes for the target location (Apply To = Target location, Support = Yes)
2. A check is open with cover count 2

**Step(s):**
1. Open the check and tap the Change Cover function key
2. Enter 2 as the adult cover and 1 as the children cover
3. Confirm the change

**Test Result(s):**
1. The children cover input is available and accepted
2. The cover count is updated to include both adult and children covers and is displayed correctly

#### 2.4.Change cover with children cover input disabled

**Prerequisite(s):**
1. Config by Location > Allow Children Cover Input is set to No for the target location
2. A check is open with a valid cover count

**Step(s):**
1. Open the check and tap the Change Cover function key
2. Check whether the children cover input is available
3. Attempt to enter a children cover and confirm

**Test Result(s):**
1. The children cover input is not available or cannot be filled in
2. Only the adult cover can be changed and the check is updated accordingly

### 3.Ask Cover Before Payment

#### 3.1.Ask Cover prompt triggered before payment

**Prerequisite(s):**
1. Config by Location > Pay Check Auto Functions > Change Cover (Ask cover before payment) is enabled for the target location
2. A check is open with at least one ordered item

**Step(s):**
1. Order items on the check
2. Tap Paid to proceed to the cashier screen
3. Modify the cover on the Ask Cover prompt and continue

**Test Result(s):**
1. The Ask Cover prompt is displayed before the payment workflow continues
2. After the cover is confirmed, the payment workflow proceeds normally with the updated cover

#### 3.2.No Ask Cover prompt when configuration is disabled

**Prerequisite(s):**
1. Config by Location > Pay Check Auto Functions > Change Cover (Ask cover before payment) is not enabled for the target location
2. A check is open with at least one ordered item

**Step(s):**
1. Order items on the check
2. Tap Paid to proceed to the cashier screen

**Test Result(s):**
1. No Ask Cover prompt is displayed
2. The payment workflow proceeds directly to the cashier screen without interruption

### 4.Ask Cover in Bar Mode

#### 4.1.Ask Cover function key in bar mode with day and night themes

**Prerequisite(s):**
1. The outlet is configured as bar mode
2. The Ask Cover function key is placed in the general function key layout
3. Config by Location > Allow Children Cover Input is enabled for the target location

**Step(s):**
1. Open a check in bar mode on the horizontal desktop view in day mode and tap Ask Cover
2. Repeat step 1 in night mode
3. Repeat steps 1 and 2 on the vertical mobile view

**Test Result(s):**
1. The Ask Cover prompt is displayed correctly in bar mode for both horizontal and vertical views
2. The new day and night theme is applied to the Ask Cover layout in both views
3. The children cover input behaves according to the Allow Children Cover Input configuration
