# TMS 2.0 - Incorrect change log info after change arrival cover Test Report

## Functional Testing

### 1.Change Arrival Cover Operation

#### 1.1.Change arrival cover on a host-arrived reservation

**Prerequisite(s):**
1. TMS Operation is accessible and a reservation exists with a table assigned (e.g. Table 5)
2. The reservation is marked Host Arrived with Arrival Cover set to 2

**Step(s):**
1. Open TMS Operation and locate the reservation
2. Click Arrival again on the same reservation to open Change Arrival Cover
3. Change Arrival Cover from 2 to 4 and click Confirm
4. Open the Change Log tab of that reservation

**Reproduction Result(s):**
1. Arrival Cover is updated to 4, but the change log wrongly records Table# : from 5 to [Empty]
2. The Arrival Cover from/to entry is not written to the change log

**Fix Result(s):**
1. Arrival Cover is updated successfully on the reservation
2. The change log records the cover change, e.g. Arrival Cover : From 2 To 4
3. The table number is not logged as changed

#### 1.2.Change arrival cover without changing the value

**Prerequisite(s):**
1. TMS Operation is accessible and a reservation exists with a table assigned
2. The reservation is marked Host Arrived with Arrival Cover set to 4

**Step(s):**
1. Open TMS Operation and click Arrival on the reservation to open Change Arrival Cover
2. Keep Arrival Cover as 4 and click Confirm
3. Open the Change Log tab of that reservation

**Reproduction Result(s):**
1. The change log wrongly records Table# : from 5 to [Empty] even though the cover value was not changed

**Fix Result(s):**
1. No "Arrival Cover : From 4 To 4" entry is shown in the Change Log because the cover number is the same as the old one
2. No Table# entry is written to the change log

### 2.Change Log Recording Accuracy

#### 2.1.Change log records correct from and to values for arrival cover

**Prerequisite(s):**
1. TMS Operation is accessible and a reservation exists with a table assigned
2. The reservation is marked Host Arrived with Arrival Cover set to 2

**Step(s):**
1. Change Arrival Cover from 2 to 6 via the Arrival function and confirm
2. Open the Change Log tab of the reservation
3. Verify the field name and the from/to values of the new log entry

**Reproduction Result(s):**
1. The original cover value is overwritten before the log write, so no correct from/to pair is available and the Arrival Cover entry is missing

**Fix Result(s):**
1. The change log entry shows Arrival Cover : From 2 To 6 with correct field name and values
2. No unrelated fields (such as Table#) appear in the log entry for this action

### 3.Regression Scope

The following related modules and scenarios must be retested after the fix:

- TMS Operation Change Arrival Cover flow (single reservation, cover increased and decreased)
- Mark Host Arrived flow with initial Arrival Cover input
- Reservation Change Log recording for other reservation updates (edit booking, table change, reservation field updates)
- New Reservation creation with cover input
- Verification that table assignment and table change logging still works as before
