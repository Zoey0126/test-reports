# TMS 2.0 - Revert the durable to false when send message to RabbitMQ Test Report

## Functional Testing

### 1.Websocket And Environment Setup Verification

#### 1.1.Websocket connection and TMS environment readiness

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to both the TMS environment and the HKJC environment
2. Two browsers are available for testing
3. The websocket service is reachable before testing starts

**Step(s):**
1. Open browser A and browser B and log in to the TMS environment
2. Open the network tab in the browser developer tools and verify the websocket connection is established
3. Repeat steps 1 and 2 in the HKJC environment

**Reproduction Result(s):**
1. With the previous change that set durable to true, the websocket message could not be sent because the queue behavior was different and the RabbitMQ broker rejected the message for the not-yet-upgraded MQ version

**Fix Result(s):**
1. The websocket connection is established successfully in both TMS and HKJC environments
2. The websocket stays connected during the test session

### 2.New Booking And Table Lock In TMS Environment

#### 2.1.Table lock synchronization across browsers in TMS environment

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to the TMS environment
2. Websocket is connected in both browsers as verified in scenario 1.1
3. Two browsers are logged in to the TMS environment

**Step(s):**
1. In browser A, navigate to the new booking page
2. In browser B, navigate to the current page
3. In browser A, select a period and a table for a new booking
4. In browser B, observe the current page and verify the selected table is locked with the same period, outlet and date
5. In browser A, unselect the table
6. In browser B, verify the table lock icon disappears

**Reproduction Result(s):**
1. With durable set to true, the table lock message could not be delivered through RabbitMQ because the queue rejected the message
2. The table lock icon would not appear in browser B when the table was selected in browser A

**Fix Result(s):**
1. When the table is selected in browser A, the table lock icon appears in browser B for the same period, outlet and date
2. When the table is unselected in browser A, the table lock icon disappears in browser B
3. The websocket message is delivered successfully with durable reverted to false

#### 2.2.Booking creation synchronization across browsers in TMS environment

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to the TMS environment
2. Websocket is connected in both browsers
3. Scenario 2.1 has been completed successfully

**Step(s):**
1. In browser A, on the new booking page, complete the new booking for the selected period and table
2. In browser B, on the current page, observe the reservation list

**Reproduction Result(s):**
1. With durable set to true, the booking creation message could not be delivered through RabbitMQ
2. The reservation would not appear in browser B until a manual refresh

**Fix Result(s):**
1. When the booking is successfully made in browser A, the reservation appears in browser B on the current page without manual refresh
2. The websocket message for booking creation is delivered successfully with durable reverted to false

### 3.Quick Table Change In TMS Environment

#### 3.1.Quick table change synchronization across browsers

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to the TMS environment
2. Websocket is connected in both browsers
3. A reservation exists that can be moved through quick table change

**Step(s):**
1. In browser A, perform a quick table change for an existing reservation
2. In browser B, observe the current page and verify the table number is updated

**Reproduction Result(s):**
1. With durable set to true, the quick table change message could not be delivered through RabbitMQ
2. The table number would not be updated in browser B until a manual refresh

**Fix Result(s):**
1. After the quick table change in browser A, the table number is updated in browser B without manual refresh
2. The websocket message for quick table change is delivered successfully with durable reverted to false

### 4.Mark Arrival With POS In TMS Environment

#### 4.1.Mark arrival synchronization between TMS and POS

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to the TMS environment
2. Websocket is connected in both browsers
3. A reservation exists that can be marked as arrived
4. The POS workstation is online and connected

**Step(s):**
1. In browser A (or browser B), click the mark arrival icon to perform the mark arrival action
2. Verify the operation in TMS changes color to indicate the guest has arrived
3. Verify the table in POS also changes color to indicate the guest has arrived

**Reproduction Result(s):**
1. With durable set to true, the mark arrival message could not be delivered through RabbitMQ
2. Either TMS or POS would not update the color to reflect the arrival status

**Fix Result(s):**
1. The mark arrival action succeeds
2. Both the TMS operation and the POS table change color to indicate the guest has arrived
3. The websocket message for mark arrival is delivered successfully with durable reverted to false

### 5.Cross-Environment Verification In HKJC

#### 5.1.New booking, quick table change and mark arrival in HKJC environment

**Prerequisite(s):**
1. The build containing the HERO-68901 fix is deployed to the HKJC environment
2. Websocket is connected in both browsers as verified in scenario 1.1
3. Two browsers are logged in to the HKJC environment

**Step(s):**
1. In browser A, navigate to the new booking page and select a period and a table
2. In browser B, navigate to the current page and verify the table is locked with the same period, outlet and date
3. In browser A, complete the new booking and verify the reservation appears in browser B without manual refresh
4. In browser A, perform a quick table change and verify the table number is updated in browser B
5. Click the mark arrival icon to perform the mark arrival action
6. Verify both the HKJC operation and the POS table change color to indicate the guest has arrived

**Reproduction Result(s):**
1. With durable set to true, all websocket messages in the HKJC environment could not be delivered through RabbitMQ
2. None of the table lock, booking creation, quick table change or mark arrival actions would sync across browsers or between HKJC and POS

**Fix Result(s):**
1. The table lock, booking creation, quick table change and mark arrival actions all sync correctly across browsers in the HKJC environment
2. The mark arrival action syncs correctly between HKJC and POS
3. The websocket messages are delivered successfully with durable reverted to false in the HKJC environment

### 6.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Websocket connection stability in both TMS and HKJC environments over an extended test session
- Table lock, unlock and re-lock across more than two browsers
- Booking creation, edit and cancellation synchronization
- Quick table change for reservations in different statuses
- Mark arrival and mark no-show actions between TMS/HKJC and POS
- Any other RabbitMQ-driven real-time updates in TMS and HKJC environments
- Verification that no regressions are introduced for the original HERO-67901 use cases that required durable to be true once the RabbitMQ version is upgraded
