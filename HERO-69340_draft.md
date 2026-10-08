# HERO-69340 Test Report

## Functional Testing

### F1.POS Client (Windows) Version Description Reference Update

#### F1.1.Verify POS Client (Windows) Version Description Is Updated For Internal Reference

**Prerequisite(s):**
1. The fixed POS Client (Windows) build is deployed on a Workstation running Windows 11 Enterprise
2. The POS Client (Windows) is able to start and connect to the local server (Cloud / Local / Wired network)
3. Test environment: Local test environment with the fixed build installed

**Step(s):**
1. Launch the POS Client (Windows) on the Workstation and log in with a valid operator account
2. Open the POS client "About" / version info screen (or any reference location that displays the POS client version description)
3. Verify the POS client version description string is updated to the new value for dev team internal reference
4. Compare the displayed version description with the expected reference value defined for this release

**Test Result(s):**
1. The POS Client (Windows) launches and logs in successfully on the Workstation
2. The POS client version description displayed on the "About" / version info screen matches the expected updated reference value for the current release
3. The updated version description is shown consistently on Workstation devices (Windows 11 Enterprise)

#### F1.2.Verify POS Client Version Is Reported Correctly On HQ Station Reports

**Prerequisite(s):**
1. The fixed POS Client (Windows) build is deployed on a Workstation and connected to the LS
2. The LS and HQ environment are connected (Cloud / Local network)
3. The launcher is running on the LS so the periodic device info sync (including POS client version) from LS to HQ is active
4. Test environment: Local / HQ test environment

**Step(s):**
1. Launch the POS Client (Windows) on the Workstation and log in
2. Allow the launcher to run on the LS and wait for the periodic device info sync cycle
3. Open the HQ Summary of Workstations Report by Shop (POS0280) and locate the row for the Workstation
4. Verify the "POS Client Version" column shows the updated POS client version description for the Workstation
5. Repeat the verification on the Summary of Workstations Report by Merchant (POS0258) for cross-validation
6. Confirm the workstation device info fields (Windows machine name, OS version, OS UDID, IPv4, IPv6, last connect date / time) are also synced and shown correctly

**Test Result(s):**
1. The POS Client (Windows) version description is uploaded to HQ via the periodic device info sync
2. The HQ Summary of Workstations Report by Shop (POS0280) shows the updated POS client version description in the "POS Client Version" column for the Workstation
3. The Summary of Workstations Report by Merchant (POS0258) shows the same updated POS client version description, confirming consistent reporting across station reports
4. The workstation device info fields (Windows machine name, OS version, OS UDID, IPv4, IPv6, last connect date / time) are synced and displayed correctly on HQ station reports

### F2.Regression — POS Client (Windows) Functional Flows After Version Description Update

#### F2.1.Verify Standard POS Operations Are Preserved After The Version Description Update

**Prerequisite(s):**
1. The fixed POS Client (Windows) build is deployed on a Workstation
2. A valid operator account and an outlet with menu / item / payment setup are available
3. Test environment: Local test environment

**Step(s):**
1. Launch the POS Client (Windows) and log in
2. Perform a sequence of standard POS operations (open outlet, daily start, open check, order items, apply payment, close check, daily close)
3. Trigger printing of standard reports and verify the print jobs are not impacted by the version description update
4. Inspect the POS client and LS logs for any errors related to the version description update

**Test Result(s):**
1. All standard POS operations (open outlet, daily start, open check, order items, apply payment, close check, daily close) complete successfully on the POS Client (Windows)
2. Standard report print jobs complete without impact from the version description update
3. No errors related to the version description update are observed in the POS client or LS logs
4. The version description update does not impact existing POS Client (Windows) functional flows

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69340):**

- POS version information for dev team internal reference regular update.
- The POS Client (Windows) version description must be updated to the new reference value and displayed consistently on the "About" / version info screen.
- The updated POS client version description must be reported correctly on HQ station reports (POS0280, POS0258) after the periodic device info sync.
- The version description update must not impact existing POS Client (Windows) functional flows.
