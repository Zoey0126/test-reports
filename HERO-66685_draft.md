# Last PIN logged-in user records lost if setup data sync from HQ to LS Test Report

## Functional Testing

### 1.Last Users Record Preservation After Setup Data Sync

#### 1.1.Last Users tab retains prior PIN login records when HQ publishes setup data and LS performs data update

**Prerequisite(s):**
1. PIN rules configured at System Management > System Configuration > User Module (default settings acceptable).
2. Target station configured with "Login name and Password" and "User Pin" at POS System > Station Setup > Stations > User Login Method.
3. Users prepared in User Management and assigned to a User Group permitted for PIN login.
4. POS System > Ordering Setup > Config by Location > PIN Login Settings configured: Apply To = target location; User List Display Info = "First Name and Last Name"; Default Tab = "Last Users"; User Groups Allowed to Log In by PIN = target group.
5. POS client configured to use "Direct connect" log-in type at General > Server Setting > Log-in type.
6. HQ platform available for publishing setup data via Manage Date Release.
7. Local server available for Data Update to download the published data from HQ.

**Step(s):**
1. On the HQ platform, go to Manage Date Release and publish setup data to the target local server.
2. On the local server platform, go to Data Update and download the published data from HQ.
3. At the POS client initial screen, click "Direct Connect".
4. Click the portrait icon.
5. On the Select PIN Login User page, select the "All Users" tab.
6. Select a target user and enter the PIN.
7. Click "Enter" and confirm the floor plan is displayed.
8. On the HQ platform, publish another setup data release via Manage Date Release.
9. On the local server platform, perform Data Update to download the new release from HQ.
10. At the POS client, click "Exit" to leave the floor plan.
11. Click the portrait icon again to open the Select PIN Login User page.
12. Select the "Last Users" tab.

**Reproduction Result(s):**
1. Setup data is successfully published from HQ to the local server and the local server completes Data Update.
2. The PIN login succeeds and the floor plan is displayed after the first login.
3. After HQ republishes setup data and the local server performs Data Update, the "Last Users" tab displays an empty list (bug reproduced).

**Fix Result(s):**
1. The previously logged-in user (from step 6) is displayed in the "Last Users" tab list after HQ republishes setup data and the local server performs Data Update.
2. No PIN login record loss occurs across the HQ→LS sync cycle.

#### 1.2.Last Users tab continues to retain prior PIN login records when setup data sync from HQ to LS is repeated multiple times

**Prerequisite(s):**
1. All prerequisites from 1.1 are satisfied.
2. A user has successfully logged into the POS client via PIN at least once.

**Step(s):**
1. Publish setup data from HQ via Manage Date Release.
2. Download the published data on the local server via Data Update.
3. At the POS client, exit and click the portrait icon, then open the "Last Users" tab.
4. Repeat steps 1–3 for additional sync cycles.

**Reproduction Result(s):**
1. After each HQ→LS sync cycle, the "Last Users" tab displays an empty list.

**Fix Result(s):**
1. The originally logged-in user remains visible in the "Last Users" tab after each successive HQ→LS sync cycle.

#### 1.3.Last Users tab works correctly when no setup data is published from HQ after PIN login

**Prerequisite(s):**
1. All prerequisites from 1.1 are satisfied.
2. A user has successfully logged into the POS client via PIN at least once.

**Step(s):**
1. At the POS client, exit and click the portrait icon to open the Select PIN Login User page.
2. Select the "Last Users" tab without performing any HQ→LS data sync.
3. Verify the previously logged-in user is listed.
4. Log in with a different user via PIN and exit again.
5. Reopen the "Last Users" tab and verify the most recent users are listed.

**Reproduction Result(s):**
1. The "Last Users" tab correctly displays the previously logged-in users when no HQ→LS sync is involved.

**Fix Result(s):**
1. The "Last Users" tab continues to correctly display the most recent PIN login users regardless of whether an HQ→LS sync occurs.

## Test Environment Information

- **Test Environment**: Local Test Environment, HQ Test Environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Related Version**: Infrasys Cloud v1.2.73.0 and later (refer to release note "Infrasys_cloud_pos_release_note_1_0_73_0(Infrasys Cloud v1.2.73.0).docx")

## Appendix

### Acceptance Criteria (from JIRA)

When data update is performed on a local server after the HQ server publishes setup data, the previous PIN login records on the local server are erased.

Workflow:
1. Go to Manage Date Release on the platform of HQ server to publish data.
2. Go to Data Update on the platform of local server to download data from HQ server.
3. Click the button "Direct Connect" on the initial screen of POS client.
4. Click the portrait icon.
5. Select "Last Users" tab.
6. System displays an empty list.

Expected result:
Users previously logged into the station should be displayed in the last users list.

It is fixed in current version.

Related: [Jira-30354, Jira-31280, Jira-31281 / Epic-30236] POS Feature - Show last 10 PIN user logins (POS Dev 3/3).