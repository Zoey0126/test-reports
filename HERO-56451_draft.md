# The Autoreport interface settings need to be configured according to the config zone Test Report

## Functional Testing

### 1.Autoreport Interface Settings per Config Zone

#### 1.1.Autoreport interface settings follow the current Config Zone hierarchy

**Prerequisite(s):**
1. Autoreport feature is enabled at the Platform level.
2. Multiple Config Zones exist (e.g., 本层级, 下一层级, 上层级, 所有层级包括下层级).
3. Autoreport schedules with different interface settings (Email, FTP, SFTP) are configured at different Config Zone levels.

**Step(s):**
1. Log in to the Platform as a user associated with a specific Config Zone.
2. Navigate to System Management > Auto Report Configuration.
3. View the Autoreport schedules visible to the current user.
4. Verify the interface settings (Email/FTP/SFTP server, port, credentials) shown for each schedule.
5. Switch to a user in a different Config Zone and re-open the Auto Report Configuration page.
6. Compare the interface settings visible for each schedule between the two Config Zones.

**Reproduction Result(s):**
1. Autoreport interface settings are not aligned with the current Config Zone hierarchy (bug reproduced).
2. A schedule configured under a different Config Zone level exposes interface settings that do not match the current user's Config Zone.

**Fix Result(s):**
1. The Autoreport interface settings displayed follow the user's current Config Zone hierarchy.
2. Each Config Zone level shows interface settings that are properly scoped to that level.
3. There is no leakage of interface settings from one Config Zone to another.

#### 1.2.Autoreport schedule runs use the interface settings of the schedule's owning Config Zone

**Prerequisite(s):**
1. Autoreport feature is enabled.
2. At least one Autoreport schedule is configured with interface settings at the 本层级 Config Zone.
3. At least one Autoreport schedule is configured at the 所有层级包括下层级 Config Zone.

**Step(s):**
1. Trigger the Autoreport schedule configured at 本层级 and confirm the destination (Email/FTP/SFTP) and the sending server used.
2. Trigger the Autoreport schedule configured at 所有层级包括下层级 and confirm the destination and sending server used.
3. Verify both schedules use the interface settings of their respective owning Config Zones.

**Reproduction Result(s):**
1. The Autoreport schedule does not use the interface settings of its owning Config Zone (bug reproduced).
2. Interface settings from another Config Zone level may be used instead.

**Fix Result(s):**
1. Each Autoreport schedule uses the interface settings of its owning Config Zone.
2. Reports are delivered to the destination configured at the correct Config Zone level using the correct interface settings.

#### 1.3.Switching Config Zone updates the Autoreport interface settings accordingly

**Prerequisite(s):**
1. User has access to multiple Config Zones.
2. Autoreport schedules and their interface settings exist at more than one Config Zone level.

**Step(s):**
1. While logged in under Config Zone A, open Auto Report Configuration and record the interface settings.
2. Switch the user's Config Zone to Config Zone B.
3. Re-open Auto Report Configuration and record the interface settings again.
4. Compare the interface settings between the two Config Zones.

**Reproduction Result(s):**
1. Interface settings do not refresh properly when the user switches Config Zone (bug reproduced).

**Fix Result(s):**
1. Interface settings update immediately when the user's Config Zone is switched.
2. The Autoreport configuration reflects the correct Config Zone after each switch.

## Test Environment Information

- **Test Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
- **Test Browsers**: Chrome, Firefox, Microsoft Edge
- **Related Module**: System Management > Auto Report Configuration

## Appendix

### Acceptance Criteria (from JIRA)

The Autoreport interface settings need to be configured according to the config zone.

Expected Result:
- Autoreport interface settings follow the user's current Config Zone hierarchy.
- Each Autoreport schedule uses the interface settings of its owning Config Zone.
- Switching the user's Config Zone updates the Autoreport interface settings accordingly.
- There is no leakage of interface settings from one Config Zone to another.