# HERO-68250 TMS2.0 - "No Notification Deployment Applies" is obscured Test Report

## Functional Testing

### 1.Reproduction - Obscured Notification Deployment Prompt

#### 1.1."No Notification Deployment Applies" prompt is obscured when no notifications are deployed

**Prerequisite(s):**
1. TMS 2.0 is deployed and accessible.
2. Go to TMS > Basic Setup > Notification Deployments and ensure no notifications are deployed for the outlet.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Click the SMS icon.

**Reproduction Result(s):**
1. The "No Notification Deployment Applies" and "No Message Interface Applies" prompts repeatedly appear.
2. One of the prompts is obscured by the other.
3. The user cannot clearly see which message applies, causing confusion about the notification deployment status.

### 2.Fix Verification - Notification Deployment Prompt Display

#### 2.1.Hovering over SMS icon shows "No Notification Deployment Applies" when no notifications are deployed

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. Go to TMS > Basic Setup > Notification Deployments and ensure no notifications are deployed for the outlet.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Hover the mouse over the SMS icon.
4. Verify the prompt that appears.

**Fix Result(s):**
1. The message "No Notification Deployment Applies" is displayed when hovering over the SMS icon.
2. Only one prompt is shown at a time (no obscuring or overlapping).
3. The prompt is clearly visible and readable.

#### 2.2.Hovering over Email icon shows "No Notification Deployment Applies" when no notifications are deployed

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. Go to TMS > Basic Setup > Notification Deployments and ensure no notifications are deployed for the outlet.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Hover the mouse over the Email icon.
4. Verify the prompt that appears.

**Fix Result(s):**
1. The message "No Notification Deployment Applies" is displayed when hovering over the Email icon.
2. Only one prompt is shown at a time.
3. The prompt is clearly visible and not obscured.

### 3.Message Interface Prompt Scenarios

#### 3.1."No Message Interface Applies" shown when no message interface is configured

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. The outlet has no message interface configured.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Hover the mouse over the SMS or Email icon.
4. Verify the prompt that appears.

**Fix Result(s):**
1. The message "No Message Interface Applies" is displayed when no message interface is configured for the outlet.
2. Only one prompt is shown at a time.
3. The prompt is clearly visible and not obscured.

#### 3.2."No Active Message Interface Applies" shown when the message interface is disabled

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. The outlet has a message interface configured but it has been disabled.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Hover the mouse over the SMS or Email icon.
4. Verify the prompt that appears.

**Fix Result(s):**
1. The message "No Active Message Interface Applies" is displayed when the configured message interface is disabled.
2. Only one prompt is shown at a time.
3. The prompt is clearly visible and not obscured.

### 4.Message Prompt Priority Verification

#### 4.1.Priority order: No Message Interface Applies > No Active Message Interface Applies > No Notification Deployment Applies

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. Multiple notification conditions can be simulated for the same outlet (no message interface, disabled message interface, no notification deployment).

**Step(s):**
1. Configure the outlet so that no message interface is configured and no notification deployment exists.
2. Go to TMS Operation > New Reservation page, select the outlet, date, and period, and hover over the SMS icon.
3. Verify the highest-priority prompt ("No Message Interface Applies") is shown.
4. Configure the outlet so that a message interface is configured but disabled, and no notification deployment exists.
5. Hover over the SMS icon again and verify the prompt ("No Active Message Interface Applies") is shown.
6. Configure the outlet so that a message interface is active but no notification deployment exists.
7. Hover over the SMS icon again and verify the prompt ("No Notification Deployment Applies") is shown.

**Fix Result(s):**
1. When no message interface is configured, "No Message Interface Applies" is shown (highest priority).
2. When the message interface is disabled, "No Active Message Interface Applies" is shown (second priority).
3. When the message interface is active but no notification deployment exists, "No Notification Deployment Applies" is shown (lowest priority).
4. Only the highest-priority applicable prompt is shown at any time.
5. No prompts are obscured or overlapped.

### 5.Regression - Normal Notification Deployment

#### 5.1.Normal notification deployment does not show any error prompt

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. The outlet has a valid notification deployment configured with an active message interface.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Hover the mouse over the SMS or Email icon.
4. Verify no error prompt appears.

**Fix Result(s):**
1. No "No Notification Deployment Applies", "No Message Interface Applies", or "No Active Message Interface Applies" prompt is shown when a valid notification deployment is configured.
2. The SMS or Email icon functions normally (e.g., opens the notification compose dialog or sends the notification).
3. No regression is introduced for outlets with valid notification deployments.

#### 5.2.SMS and Email notification sending works correctly with valid deployment

**Prerequisite(s):**
1. The build containing the HERO-68250 fix is deployed to the TMS environment.
2. The outlet has a valid notification deployment configured with an active message interface.

**Step(s):**
1. Go to TMS Operation > New Reservation page.
2. Select the outlet, date, and period.
3. Create a reservation.
4. Click the SMS icon and send an SMS notification.
5. Click the Email icon and send an Email notification.

**Fix Result(s):**
1. The SMS notification is sent successfully without any prompt error.
2. The Email notification is sent successfully without any prompt error.
3. The notification sending flow works as expected with a valid deployment.
