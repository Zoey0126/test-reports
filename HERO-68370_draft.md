# Issues of POS WSPS feature Test Report

## Functional Testing

### 1.WSPS Role Start on Selected Online Device

#### 1.1.Start WSPS on the Selected Online Device

**Prerequisite(s):**
1. The fixed POS launcher and POS Client (Windows) are deployed
2. At least two eligible online stations appear in the Online Devices bar of the Print Service Information panel
3. The current login POS is different from the target station to be selected

**Step(s):**
1. Open the Print Service Information panel on the current POS
2. Select another online station in the Online Devices bar
3. Click the Start WSPS button for the selected station
4. Check the WSPS role status on the selected station

**Reproduction Result(s):**
1. The action button always worked for the current login POS even when another POS was selected, so the selected device never took the WSPS role

**Fix Result(s):**
1. The selected online station takes the WSPS role and its status changes to Starting and then Running
2. The Start action applies to the station selected in the Online Devices bar, not to the current login POS

#### 1.2.Start WSPS on a Station with Embed Printing Service Disabled

**Prerequisite(s):**
1. One online station has the Enable Embed Printing Core Service setting set to Disable
2. Another online station has the setting enabled

**Step(s):**
1. Open the Print Service Information panel and check the Online Devices bar
2. Try to select the disabled station and start WSPS on it
3. Start WSPS on the enabled station

**Reproduction Result(s):**
1. Device eligibility was unclear, and stations with the embed printing setting disabled could still appear selectable, leading to failed starts

**Fix Result(s):**
1. The station with Enable Embed Printing Core Service set to Disable is not nominated and cannot take the WSPS role
2. Only stations following the Enable Embed Printing Core Service setting are eligible to hold the WSPS role

#### 1.3.Pause and Restore Act Only on the Current Workstation

**Prerequisite(s):**
1. The current POS is holding the WSPS role and printing is active
2. Another online station is selected in the Online Devices bar

**Step(s):**
1. With another station selected, click Pause on the Print Service Information panel
2. Observe which workstation is paused
3. Click Restore and observe again

**Reproduction Result(s):**
1. Panel actions were inconsistently targeted, making it unclear which workstation was controlled by Pause and Restore

**Fix Result(s):**
1. Pause and Restore apply only to the current workstation regardless of the selection in the Online Devices bar
2. The other selected station is not paused or restored by these actions

### 2.WSPS Panel Status Display

#### 2.1.Starting Status Is Shown After Role Assignment Before Printing Starts

**Prerequisite(s):**
1. The fixed POS launcher and POS Client (Windows) are deployed
2. An eligible online station is available and not holding the WSPS role

**Step(s):**
1. Start WSPS on the eligible station
2. Observe the status on the Print Service Information panel immediately after the role is assigned and before print traffic starts

**Reproduction Result(s):**
1. Right after Start, the status often looked like Standby even though the role had just been assigned and printing had not started, which was misleading

**Fix Result(s):**
1. The panel shows Starting while the role is assigned but printing has not started yet
2. The status changes to Running once print traffic is active

#### 2.2.Status Automatically Changes to Standby After About 90 Seconds of No Contact

**Prerequisite(s):**
1. A station currently holds the WSPS role with status Running
2. The station stops having any print contact (no print traffic)

**Step(s):**
1. Keep the Print Service Information panel open on the observing POS
2. Stop all print contact from the WSPS role holder and wait for more than 90 seconds
3. Observe the status of the role holder

**Reproduction Result(s):**
1. The system was unable to automatically change the status to Standby when the WSPS role was assigned but there was no contact for more than 90 seconds

**Fix Result(s):**
1. The WSPS status automatically changes from Running to Standby when there is no activity for about 90 seconds

#### 2.3.No False Standby After Start and Status Refreshes While Panel Is Open

**Prerequisite(s):**
1. The current login POS has no WSPS role assigned
2. The Print Service Information panel is available

**Step(s):**
1. Click Start WSPS on the unassigned-role POS and observe the status right after the click
2. Leave the panel open and let the station take the role and start printing
3. Observe whether the status updates by itself while the panel stays open

**Reproduction Result(s):**
1. The status was displayed as Standby after clicking Start WSPS on an unassigned-role POS, and the health status could look stale until the panel was reopened

**Fix Result(s):**
1. No false Standby status is shown right after Start; the status shows Starting or Running according to the actual state
2. The status refreshes automatically while the panel stays open without needing to reopen the panel

### 3.WSPS Port Failure Handling

**Prerequisite(s):**
1. The POS launcher is configured with an invalid or occupied WSPS port so that the port cannot be opened
2. The POS login and launcher services are otherwise healthy

**Step(s):**
1. Start the POS launcher with the unopenable WSPS port
2. Log in to the POS and use normal launcher functions
3. Check the WSPS state for the session

**Reproduction Result(s):**
1. A WSPS port failure could block the whole launcher and affect POS login

**Fix Result(s):**
1. POS login and normal launcher services continue without being blocked by the WSPS port failure
2. WSPS stays off for that session while the rest of the launcher keeps working

### 4.Regression Scope

**Prerequisite(s):**
1. The fixed POS launcher and POS Client (Windows) are deployed together on the test stations
2. At least two stations with the Enable Embed Printing Core Service setting enabled are online

**Step(s):**
1. Retest WSPS start, pause, restore and stop on both the current workstation and selected online stations
2. Retest the status transitions Starting, Running and Standby, including the 90-second inactivity change and the panel refresh
3. Retest normal embedded printing jobs end to end while a station holds the WSPS role
4. Retest POS login and launcher startup with a valid WSPS port and with an unopenable WSPS port
5. Retest stations with the embed printing setting disabled to confirm they are never nominated

**Fix Result(s):**
1. All WSPS role transitions and panel actions behave as described, and printing through the embedded service is unaffected
2. POS login, launcher services and non-WSPS printing keep working normally, confirming no regression is introduced by the fix
