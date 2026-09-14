# TMS 2.0 - Overnight period saving fails in Config by Date with error "overlapped_period_is_not_allowed" Test Report

## Functional Testing

### 1.Period and Period Set Preparation

#### 1.1.Create overnight period and add it to a period set

**Prerequisite(s):**
1. User has access to Backend Outlet Management and TMS Basic Setup
2. The target outlets support both regular period and overnight period types

**Step(s):**
1. Go to Backend > Outlet Management > Date and Period > Period and create a regular period E-Breakfast with Time Range of Period 05:00:00-10:59:59 and Time Range of Reservation 05:00:00-10:55:59
2. Create an overnight period E-Overnight period with Time Range of Period 18:00:00-05:59:59 (overnight) and Time Range of Reservation 18:00:00-21:55:59 (non-overnight)
3. Go to Backend > TMS > Basic Setup > Period Sets, create a new period set "Elvar test" and add periods E-Breakfast and E-Overnight period

**Reproduction Result(s):**
1. The overnight period and the period set are saved normally in Backend and TMS Basic Setup, no error is prompted in this preparation step

**Fix Result(s):**
1. The overnight period and the period set are saved normally and can be selected in Config by Date

### 2.Overnight Period Saving in Config by Date

#### 2.1.Replace period set and edit overnight period on an operating date

**Prerequisite(s):**
1. Period set "Elvar test" contains the overnight period E-Overnight period
2. The default period set of the outlet uses a period that will overlap with the overnight period
3. An operating date is selected for outlet Elvar-Hong Kong (E0414) that does not have period settings configured for that day

**Step(s):**
1. Open Table Management System > Config by Date and select outlet Elvar-Hong Kong (E0414)
2. Select the operating date that has no period settings configured and replace the Period Set by "Elvar test", then save
3. Edit the overnight period E-Overnight period, then save

**Reproduction Result(s):**
1. Saving the valid overnight period prompts error "overlapped_period_is_not_allowed" even though the overnight period does not actually overlap other periods on that date

**Fix Result(s):**
1. The overnight period is saved successfully and no "Overlapped period is not allowed" error appears
2. The period settings of the selected date reflect the edited overnight period correctly

#### 2.2.Edit regular period reservation interval on the configured date

**Prerequisite(s):**
1. Config by Date for outlet Elvar-Hong Kong (E0414) has date 2026-08-29 configured with Period Set Replaced By "Elvar test"
2. The period set contains E-Breakfast and E-Overnight period

**Step(s):**
1. Go to Backend > TMS > Basic Setup > Config by Date > Elvar-Hong Kong (E0414) and select date 2026-08-29
2. Click "Find" and then click "Edit" for E-Breakfast
3. Change Time Interval of Reservation from 5 to 10 and click "Save"

**Reproduction Result(s):**
1. Error "overlapped_period_is_not_allowed" is prompted when saving, although the periods on that date do not genuinely overlap

**Fix Result(s):**
1. The period change is saved successfully without the overlapped period error
2. The updated Time Interval of Reservation is displayed correctly for that date

### 3.Period Overlap Validation

#### 3.1.Genuinely overlapping period on the same date is still blocked

**Prerequisite(s):**
1. An operating date in Config by Date has at least one period configured
2. A period can be edited so that its time range genuinely overlaps another period already configured on the same date

**Step(s):**
1. Open Config by Date and select the operating date
2. Edit a period so that its reservation time range truly overlaps another configured period
3. Click "Save"

**Reproduction Result(s):**
1. Error "overlapped_period_is_not_allowed" is prompted when saving the overlapping period

**Fix Result(s):**
1. The genuine overlap is still rejected and error "Overlapped period is not allowed" is prompted as before
2. The invalid period is not saved

#### 3.2.Non-operating day overlap validation still uses deployed period set periods

**Prerequisite(s):**
1. A non-operating date is selected in Config by Date for the target outlet
2. A period set with periods is deployed for that date

**Step(s):**
1. Open Config by Date and select the non-operating date
2. Edit or add a period so that it overlaps a period from the deployed period set
3. Click "Save"
4. Repeat with a non-overlapping period and save

**Reproduction Result(s):**
1. The overlap validation on the non-operating date is based on the deployed period set periods, and the genuine overlap is rejected with "overlapped_period_is_not_allowed"

**Fix Result(s):**
1. The behavior on non-operating days is unchanged: overlap validation continues to use the deployed period set periods
2. The non-overlapping period is saved successfully on the non-operating date

### 4.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Period overlap validation for operating days and non-operating days
- Config by Date period set replacement (Period Set Replaced By)
- Overnight period configuration in Backend Outlet Management (Time Range of Period and Time Range of Reservation)
- Period Sets basic setup (add, edit periods)
- Normal period saving without overlap remains unaffected
