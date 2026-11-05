# HERO-69854 Test Report

The reported defect applies to the Device Manager V3 Peripheral Device interface for Octopus Reader 3.0. The system incorrectly closes the Octopus device upgrade process and freezes the UI when the auto-closing screen timer expires while the upgrade process is running, after an Octopus transaction file is generated and the Octopus device returns a success response with the upgrade field.

## Functional Testing

### 1. UI Freezes When the Octopus Device Upgrade Process Is Closed by the Auto-Closing Screen

**Prerequisite(s):**
1. Valid client login credentials are available.
2. The outlet is configured with the Device Manager V3 Peripheral Device interface for Octopus Reader 3.0.
3. An Octopus Reader 3.0 device is connected and reachable from the client.
4. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field.

**Step(s):**
1. Login to the client.
2. Click function "Admin".
3. Select tab "Octopus Operation".
4. Click function "Generate OCTOPUS transaction file".
5. Wait for the system to follow the existing workflow to generate Octopus transaction files.
6. Observe the Octopus device returning a success response for generating Octopus transaction files with the upgrade field.
7. Observe the system starting the timer for auto-closing screen (5 seconds).
8. Click button "Exit".
9. The system follows the existing workflow to upgrade the Octopus device.
10. Wait until the 5-second auto-closing screen timer expires.

**Reproduction Result(s):**
1. After 5 seconds, the auto-closing screen action is performed while the Octopus device upgrade process is in progress.
2. The system closes the Octopus device upgrade process.
3. The UI freezes and no longer responds to user actions.

**Fix Result(s):**
1. Clicking button "Exit" at step 8 removes the timer for auto-closing screen.
2. No auto-closing screen action is performed after 5 seconds.
3. The system continues the Octopus device upgrade process and the UI remains responsive.

### 2. Octopus Operation Auto-Closing Screen Handling

#### 2.1. Auto-Closing Screen Timer Is Removed After Clicking Exit

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field.

**Step(s):**
1. Click function "Admin" and select tab "Octopus Operation".
2. Click function "Generate OCTOPUS transaction file" and wait for the existing generation workflow to complete.
3. Observe the success response from the Octopus device containing the upgrade field and the start of the 5-second auto-closing screen timer.
4. Click button "Exit".
5. Wait for at least 30 seconds without any user action and observe the screen.

**Reproduction Result(s):**
1. Before the fix, the timer is not removed and the auto-closing screen action is performed 5 seconds after the timer starts.
2. The upgrade process is closed by the auto-closing screen action and the UI freezes.

**Fix Result(s):**
1. The auto-closing screen timer is removed when button "Exit" is clicked.
2. The Octopus Operation screen is not closed automatically at 5 seconds or later.
3. The Octopus device upgrade process started after "Exit" is not interrupted.

#### 2.2. Auto-Closing Screen Still Closes the Screen When Exit Is Not Clicked

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field.

**Step(s):**
1. Click function "Admin" and select tab "Octopus Operation".
2. Click function "Generate OCTOPUS transaction file" and wait for the existing generation workflow to complete.
3. Observe the success response from the Octopus device containing the upgrade field and the start of the 5-second auto-closing screen timer.
4. Do not click button "Exit".
5. Wait for 5 seconds and observe the screen.

**Reproduction Result(s):**
1. Before the fix, the auto-closing screen action is performed at 5 seconds and the upgrade process is closed, leaving the UI frozen.
2. The user cannot continue any operation from the frozen screen.

**Fix Result(s):**
1. The auto-closing screen action is still performed at 5 seconds when button "Exit" is not clicked, following the existing behavior.
2. The screen is closed and the system returns to the previous screen.
3. No Octopus device upgrade process is closed by the timer and the UI remains responsive.

### 3. Octopus Device Upgrade Process Continuation

#### 3.1. Octopus Device Upgrade Continues Beyond the Auto-Closing Interval

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field.

**Step(s):**
1. Click function "Admin" and select tab "Octopus Operation".
2. Click function "Generate OCTOPUS transaction file" and wait for the existing generation workflow to complete.
3. Observe the success response from the Octopus device containing the upgrade field.
4. Click button "Exit" and let the system follow the existing workflow to upgrade the Octopus device.
5. Keep observing the upgrade progress for a duration longer than 5 seconds.

**Reproduction Result(s):**
1. Before the fix, the upgrade process is closed 5 seconds after the auto-closing screen timer starts.
2. The upgrade is interrupted and the client UI is frozen.

**Fix Result(s):**
1. The upgrade process is not interrupted at the 5-second mark.
2. The system continues the existing Octopus device upgrade workflow until it finishes.
3. No auto-closing screen action is triggered during the upgrade process.

#### 3.2. Client Remains Usable After the Octopus Device Upgrade Process

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device upgrade process has been started after an Octopus transaction file with the upgrade field was generated.

**Step(s):**
1. Click button "Exit" and wait for the Octopus device upgrade process to finish.
2. Select tab "Octopus Operation" again.
3. Click function "Admin" and navigate to another admin tab.
4. Open a new check from the ordering screen.

**Reproduction Result(s):**
1. Before the fix, the client stops responding after the auto-closing screen action closes the upgrade process.
2. No further function can be selected from the frozen screen.

**Fix Result(s):**
1. The client responds to every action after the upgrade process is finished.
2. The "Octopus Operation" tab and other admin tabs can be opened normally.
3. A new check can be opened and used normally.

### 4. Octopus Transaction File Generation Regression

#### 4.1. Octopus Transaction File Is Generated When the Upgrade Field Is Returned

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field.

**Step(s):**
1. Click function "Admin" and select tab "Octopus Operation".
2. Click function "Generate OCTOPUS transaction file".
3. Observe the generation workflow and the response from the Octopus device.

**Reproduction Result(s):**
1. Before the fix, the transaction file generation workflow is completed but the following upgrade process is closed by the auto-closing screen timer.
2. The UI freezes after the upgrade process is closed.

**Fix Result(s):**
1. The system follows the existing workflow to generate Octopus transaction files.
2. The Octopus device returns a success response for generating Octopus transaction files with the upgrade field.
3. Button "Exit" is available for the Octopus device upgrade process and clicking it starts the upgrade without interruption.

#### 4.2. No Upgrade Process Is Started When the Response Has No Upgrade Field

**Prerequisite(s):**
1. A user is logged in to the client.
2. The Device Manager V3 Peripheral Device interface with Octopus Reader 3.0 is configured for the outlet.
3. The Octopus device is prepared to return a success response for generating Octopus transaction files without the upgrade field.

**Step(s):**
1. Click function "Admin" and select tab "Octopus Operation".
2. Click function "Generate OCTOPUS transaction file".
3. Observe the response from the Octopus device and the next screen.
4. Wait for at least 5 seconds and observe the screen.

**Reproduction Result(s):**
1. Before the fix, the transaction file generation workflow ends with an auto-closing screen action at 5 seconds and the UI freezes.
2. The user cannot continue any operation from the frozen screen.

**Fix Result(s):**
1. The system follows the existing workflow to generate Octopus transaction files and no Octopus device upgrade process is started.
2. The screen behaves as before and the UI remains responsive.
3. The user can continue to operate the client after the generation workflow.

## Test Environment

Testing is performed on the Infrasys POS client with the Device Manager V3 Peripheral Device interface configured and an Octopus Reader 3.0 device connected to the client. The Octopus device is prepared to return a success response for generating Octopus transaction files with the upgrade field, the function "Generate OCTOPUS transaction file" is available in the "Octopus Operation" tab under "Admin", and the auto-closing screen timer is set to 5 seconds. A second run uses an Octopus device response without the upgrade field for the regression case.
