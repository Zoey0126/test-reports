# HERO-69808 Bug Fix Test Report - Incorrect Table Status Shown in Ask Table Number Screen

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Table Status in Ask Table Number Screen

#### F1.1.Verify Table Status Displayed for Merge Check From Target Outlet
**Prerequisite(s):**
1. HKJC POS Operation is available and the user can log in to the current outlet
2. A target outlet is configured with at least one table whose table status differs from the current outlet, such as an occupied table
3. An old check exists in the current outlet

**Step(s):**
1. Open the old check in the current outlet
2. Click the function "Merge Check From Target Outlet"
3. Select the target outlet
4. Open the "Ask Table Number" screen and review the table status of each table
5. Compare the displayed status with the table status of the target outlet

**Reproduction Result(s):**
1. The "Ask Table Number" screen shows the table status of the current outlet
2. The table status does not match the table status of the selected target outlet
3. Tables that are occupied in the target outlet may be shown as available

**Fix Result(s):**
1. The "Ask Table Number" screen shows the table status of the selected target outlet
2. Tables occupied in the target outlet are displayed as occupied
3. Tables available in the target outlet are displayed as available
4. The displayed status is identical to the table status maintained in the target outlet

#### F1.2.Verify Table Status Displayed for Transfer Check To Target Outlet
**Prerequisite(s):**
1. HKJC POS Operation is available and the user can log in to the current outlet
2. A target outlet is configured with at least one table whose table status differs from the current outlet
3. An old check exists in the current outlet

**Step(s):**
1. Open the old check in the current outlet
2. Click the function "Transfer Check To Target Outlet"
3. Select the target outlet
4. Open the "Ask Table Number" screen and review the table status of each table
5. Compare the displayed status with the table status of the target outlet

**Reproduction Result(s):**
1. The "Ask Table Number" screen shows the table status of the current outlet
2. The table status does not match the table status of the selected target outlet
3. The guest may be assigned to a table that is occupied in the target outlet

**Fix Result(s):**
1. The "Ask Table Number" screen shows the table status of the selected target outlet
2. The occupied tables of the target outlet are not selectable
3. The table status is refreshed according to the target outlet each time the target outlet is selected

#### F1.3.Verify Table Status When Switching Between Target Outlets
**Prerequisite(s):**
1. At least two target outlets are configured with different table statuses
2. An old check exists in the current outlet

**Step(s):**
1. Open the old check and click the function "Merge Check From Target Outlet"
2. Select the first target outlet and open the "Ask Table Number" screen
3. Go back and select the second target outlet
4. Open the "Ask Table Number" screen again and review the table status

**Reproduction Result(s):**
1. The table status shown belongs to the current outlet and does not change with the selected target outlet
2. The table status of the previously selected outlet may remain displayed

**Fix Result(s):**
1. The table status is refreshed and matches the currently selected target outlet
2. The table status of the previously selected outlet is not displayed
3. No stale table status is shown after switching the target outlet

#### F1.4.Verify Table Status of the Current Outlet Is Not Displayed
**Prerequisite(s):**
1. The current outlet and the target outlet have clearly different table status configurations, such as one outlet fully occupied and the other mostly available
2. An old check exists in the current outlet

**Step(s):**
1. Open the old check and click the function "Transfer Check To Target Outlet"
2. Select the target outlet that is fully occupied
3. Open the "Ask Table Number" screen
4. Review the status of all tables

**Reproduction Result(s):**
1. Available tables of the current outlet are displayed, even though the target outlet has no available table
2. The table status conflicts with the actual status of the target outlet

**Fix Result(s):**
1. All tables of the target outlet are displayed as occupied
2. No available table of the current outlet is displayed
3. The table status is consistent with the target outlet only

#### F1.5.Verify Merge Check and Transfer Check Complete With the Correct Table
**Prerequisite(s):**
1. The target outlet has an available table with a known table number
2. An old check exists in the current outlet

**Step(s):**
1. Open the old check and click the function "Merge Check From Target Outlet"
2. Select the target outlet
3. In the "Ask Table Number" screen, select the available table
4. Confirm the operation
5. Repeat the same steps with the function "Transfer Check To Target Outlet"

**Reproduction Result(s):**
1. The table status shown in the "Ask Table Number" screen is misleading
2. The selected table may already be occupied in the target outlet

**Fix Result(s):**
1. Only tables shown as available can be selected
2. The merge check and the transfer check complete successfully with the selected table
3. The check is displayed on the selected table of the target outlet after the operation
4. No error message is displayed during the operation

#### F1.6.Verify Behavior When No Target Outlet Is Selected
**Prerequisite(s):**
1. An old check exists in the current outlet
2. The "Ask Table Number" screen can be triggered

**Step(s):**
1. Open the old check and click the function "Merge Check From Target Outlet"
2. Close the outlet selection without choosing a target outlet
3. Review the screen state and the check status

**Reproduction Result(s):**
1. The behavior of the screen is not affected by the fix
2. No unexpected table status is displayed

**Fix Result(s):**
1. The operation is cancelled and the check remains unchanged
2. No "Ask Table Number" screen with incorrect table status is displayed
3. No error message is displayed

### F2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Merge Check From Target Outlet with target outlet selection and table status display
- Transfer Check To Target Outlet with target outlet selection and table status display
- Ask Table Number screen table status for the current outlet operations
- Table status refresh when the target outlet is changed
- Check merge and check transfer completion on the selected target table
- Table status maintenance in the target outlet after merge check and transfer check
- Permission and authority check for the merge check and transfer check functions
