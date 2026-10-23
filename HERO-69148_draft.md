# HERO-69148 Bug Fix Test Report - Unable to Apply Ask 2nd Approval for a Function

## Functional Testing

### F1.Bug Reproduction and Fix Verification - 2nd Approval for a Function

#### F1.1.Verify Function Is Executed After 2nd Approval User Login
**Prerequisite(s):**
1. The target function (e.g. "Print And Paid") is configured to support ask 2nd approval: POS System > Authority Control > Function > Ask Approval = Yes
2. Two general users are granted with the authority of the target function: POS System > Authority Control > Set Function Authorities > Allow = Yes
3. An approval user account with the authority of the target function is available

**Step(s):**
1. Log in as a general user
2. Open a check and order items
3. Click the target function (e.g. Print And Paid)
4. Click the button "OK" on the message that notifies a 2nd approval is required
5. Let the approval user log in to approve the function
6. Keep the system at the ordering panel and observe the result

**Reproduction Result(s):**
1. The system returns to the ordering basket directly after the approval user logs in
2. No response or message is shown after the approval
3. The target function (Print And Paid) is not executed and the check remains unsettled

**Fix Result(s):**
1. The approval user login is accepted as the 2nd approval
2. The target function (Print And Paid) starts to execute immediately after the approval user logs in
3. The check is printed and paid successfully without returning to the ordering basket

#### F1.2.Verify 2nd Approval With User ID and Password Input
**Prerequisite(s):**
1. The target function is configured with Ask Approval = Yes
2. The approval user has the authority of the target function

**Step(s):**
1. Log in as a general user, open a check and order items
2. Click the target function
3. Click "OK" on the 2nd approval notification
4. Input the approval user ID and password in the approval login prompt
5. Click "OK"

**Reproduction Result(s):**
1. The function is not performed after the approval login is accepted
2. The system stays at the ordering panel without any response

**Fix Result(s):**
1. The approval login is accepted successfully
2. The target function is executed after the approval
3. A failure message is shown with the correct reason when an invalid approval user ID or password is entered, and the function is not executed

#### F1.3.Verify 2nd Approval Is Still Required for Non-Authorised User
**Prerequisite(s):**
1. The target function is configured with Ask Approval = Yes
2. A general user without the authority of the target function is available
3. An approval user with the authority of the target function is available

**Step(s):**
1. Log in as a general user without the authority of the target function
2. Open a check and order items
3. Click the target function
4. Observe the 2nd approval notification
5. Let the approval user approve and execute the function

**Reproduction Result(s):**
1. The 2nd approval notification is shown for the non-authorised user
2. The function is not executed after the approval due to the same issue

**Fix Result(s):**
1. The 2nd approval notification is shown as designed
2. The function is executed successfully after the approval user approves it
3. The audit record shows the operator user and the approval user correctly

#### F1.4.Verify Function Without Ask Approval Setting Is Unaffected
**Prerequisite(s):**
1. Another function is configured with Ask Approval = No
2. A general user is granted with the authority of that function

**Step(s):**
1. Log in as a general user
2. Open a check and order items
3. Click the function with Ask Approval = No
4. Observe whether any approval notification is shown

**Reproduction Result(s):**
1. No approval notification is shown for the function with Ask Approval = No
2. The function is executed directly as before

**Fix Result(s):**
1. No approval notification is shown for the function with Ask Approval = No
2. The function is executed directly without any delay or additional prompt

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Authority Control > Function > Ask Approval setup (add, edit and clear)
- Authority Control > Set Function Authorities for general users and approval users
- 2nd approval workflow for other functions such as Void Item, Void Check, Discount and Reopen Check
- Cancelling the 2nd approval notification and returning to the ordering panel
- Invalid approval user ID, invalid password and inactive approval user scenarios
- Audit log record for the function performed with 2nd approval
- Operator session after the approval user logs in and logs out
