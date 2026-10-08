# HERO-67816 Test Report

## Functional Testing

### F1.POS Device Info Sync From LS To HQ Under SSO Login

#### F1.1.Reproduce The POS Device Info Upload Failure With SSO Login

**Prerequisite(s):**
1. The site is configured to use the SSO login method for POS client operators
2. The local server (LS) and HQ environment are deployed and connected (Cloud / Local network)
3. The launcher is started on the LS so the periodic pos device info sync from LS to HQ is active
4. A POS client workstation is configured with device info (machine name, OS version, OS UDID, IPv4 / IPv6 addresses) so there is data to be uploaded
5. Test environment: Local / HQ test environment with the legacy (pre-fix) build installed for reproduction purposes

**Step(s):**
1. Log in to the POS client using the SSO login method
2. Allow the launcher to run on the LS and wait for the next periodic pos device info sync cycle
3. Monitor the LS-to-HQ sync status for the pos device info record of the logged-in workstation
4. Check the HQ side (for example the Summary of Workstations Report by Shop — POS0280 / Summary of Workstations Report by Merchant — POS0258) for the device info of the SSO-logged-in workstation

**Reproduction Result(s):**
1. The POS client logs in successfully with the SSO login method
2. The periodic pos device info sync from LS to HQ fails — the device info record for the SSO-logged-in workstation is not uploaded to HQ
3. On the HQ side, the device info (machine name, OS version, OS UDID, IPv4 / IPv6, last connect date / time) of the SSO-logged-in workstation is missing or out of date
4. The upload failure is specific to the SSO login method — non-SSO logins upload the pos device info data normally

#### F1.2.Verify The Fix Uploads POS Device Info From LS To HQ Under SSO Login

**Prerequisite(s):**
1. The fixed POS client / LS build is deployed on the test environment
2. The site is configured to use the SSO login method for POS client operators
3. The LS and HQ environment are connected (Cloud / Local network)
4. The launcher is started on the LS so the periodic pos device info sync from LS to HQ is active
5. A POS client workstation is configured with device info (machine name, OS version, OS UDID, IPv4 / IPv6 addresses)
6. Test environment: Local / HQ test environment with the fixed build installed

**Step(s):**
1. Log in to the POS client using the SSO login method
2. Allow the launcher to run on the LS and wait for the next periodic pos device info sync cycle
3. Confirm the pos device info sync from LS to HQ completes successfully
4. Open the HQ Summary of Workstations Report by Shop (POS0280) and locate the SSO-logged-in workstation row
5. Verify the device info fields (machine name, OS version, OS UDID, IPv4 / IPv6, last connect date / time, POS client version) are uploaded and match the workstation data on the LS
6. Repeat the SSO login flow on multiple workstations (Workstation, Android Mobile, Apple iPad) to confirm the upload works across device types

**Fix Result(s):**
1. The POS client logs in successfully with the SSO login method
2. The periodic pos device info sync from LS to HQ completes successfully — the device info record for the SSO-logged-in workstation is uploaded to HQ
3. The HQ Summary of Workstations Report by Shop (POS0280) shows the correct device info (machine name, OS version, OS UDID, IPv4 / IPv6, last connect date / time, POS client version) for the SSO-logged-in workstation, matching the LS data
4. The SSO login method now uploads the pos device info data from LS to HQ as expected — equivalent to the non-SSO login method behaviour
5. The upload works for all configured device types (Workstation, Android Mobile, Apple iPad)

#### F1.3.Regression — Verify POS Device Info Sync Behaviour Is Preserved For Non-SSO Logins

**Prerequisite(s):**
1. The fixed POS client / LS build is deployed on the test environment
2. The site is configured with at least one non-SSO (standard) login method operator
3. The LS and HQ environment are connected (Cloud / Local network)
4. The launcher is started on the LS so the periodic pos device info sync from LS to HQ is active
5. A POS client workstation is configured with device info (machine name, OS version, OS UDID, IPv4 / IPv6 addresses)
6. Test environment: Local / HQ test environment with the fixed build installed

**Step(s):**
1. Log in to the POS client using the standard (non-SSO) login method
2. Allow the launcher to run on the LS and wait for the next periodic pos device info sync cycle
3. Confirm the pos device info sync from LS to HQ completes successfully
4. Open the HQ Summary of Workstations Report by Merchant (POS0258) and verify the non-SSO-logged-in workstation device info is uploaded correctly
5. Compare the device info fields shown on HQ with the workstation data on the LS

**Fix Result(s):**
1. The standard (non-SSO) login method continues to upload the pos device info data from LS to HQ successfully after the fix — no regression is introduced
2. The HQ Summary of Workstations Report by Merchant (POS0258) shows the correct device info (machine name, OS version, OS UDID, IPv4 / IPv6, last connect time / date, POS client version) for the non-SSO-logged-in workstation, matching the LS data
3. Both SSO and non-SSO login methods upload the pos device info data from LS to HQ consistently after the fix

## Test Environment Information

- Local test environment
- HQ test environment

## Appendix

**JIRA Acceptance Criteria (HERO-67816):**

- Normally, the pos device info record is synced from LS to HQ periodically when the launcher is started in LS.
- If the site has the SSO login method being used, the system fails to upload the data from LS to HQ.
- Workflow: 1) Login to POS client; 2) Wait for pos device info data update; 3) System fails to upload the data from LS to HQ. Expected: System uploads the pos device info data from LS to HQ successfully.
- After the fix, the SSO login method must upload the pos device info data from LS to HQ as expected, equivalent to the non-SSO login method behaviour.
