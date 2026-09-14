# KDS2.0 - UI Handling - Change the design of KDS settings page Test Report

## Functional Testing

### 1.Settings Entry and Drop-down Menu Display

#### 1.1.Settings page opens from the top right corner as a drop-down menu

**Prerequisite(s):**
1. The KDS device is running the new KDS layout with the updated settings page design.
2. The KDS device is connected and the workspace is loaded.

**Step(s):**
1. Open the new KDS layout.
2. Click the settings entry in the top right corner of the screen.

**Test Result(s):**
1. The settings page pops up as a drop-down menu from the top right corner.
2. The settings menu is styled according to the new design.

#### 1.2.Settings menu lists all setting categories

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Check the entries listed in the settings drop-down menu.
2. Click each entry to open its page.

**Test Result(s):**
1. The settings include device settings, display settings, connectivity settings, ticket settings, session settings, logs, and system information.
2. Each entry opens the corresponding settings page in the new style.

### 2.Settings Page Functions

#### 2.1.Device settings function unchanged

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Open the device settings page.
2. Modify a device setting and save.
3. Compare the available options and behavior with the old layout.

**Test Result(s):**
1. The device settings page shows the same functions as the old layout.
2. The modified device setting is saved and takes effect.

#### 2.2.Display settings function unchanged

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Open the display settings page.
2. Modify a display setting (e.g., font size or theme) and save.
3. Compare the available options and behavior with the old layout.

**Test Result(s):**
1. The display settings page shows the same functions as the old layout.
2. The modified display setting is saved and applied to the ticket display.

#### 2.3.Connectivity settings function unchanged

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Open the connectivity settings page.
2. Check and update the connection configuration (e.g., server address) and save.

**Test Result(s):**
1. The connectivity settings page shows the same functions as the old layout.
2. The connection configuration is saved and the KDS keeps connecting to the configured server.

#### 2.4.Ticket and session settings functions unchanged

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Open the ticket settings page, modify a ticket setting, and save.
2. Open the session settings page, modify a session setting, and save.
3. Verify the ticket display and session behavior after the changes.

**Test Result(s):**
1. The ticket settings and session settings pages show the same functions as the old layout.
2. The modified settings are saved and take effect on ticket display and session handling.

#### 2.5.Logs and system information accessible

**Prerequisite(s):**
1. The settings drop-down menu is open in the new KDS layout.

**Step(s):**
1. Open the logs entry and view or export the KDS logs.
2. Open the system information entry and check the displayed details.

**Test Result(s):**
1. The KDS logs can be viewed and exported as in the old layout.
2. The system information (e.g., version, device details) is displayed correctly in the new style.

### 3.Regression Scope

1. Ticket display and bump bar actions on the new KDS layout.
2. Existing settings values carried over and still effective after the design update.
3. Other KDS pages are not affected by the settings page restyle.
4. KDS connection stability after changing connectivity or session settings.
