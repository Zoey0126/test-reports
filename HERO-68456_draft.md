# [Tech] Impact of Changing RabbitMQ createQueue durable from false to true on TMS sendMessageToMQ - Dev Test Report

## Functional Testing

### 1.Table Lock Synchronization via WebSocket

#### 1.1.Table lock syncs from new booking page to current view in TMS environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the TMS environment.
2. WebSocket is connected on both browsers.
3. Browser A is on the New Booking page and Browser B is on the Current View page for the same outlet.

**Step(s):**
1. In Browser A on the New Booking page, select the period and table.
2. Observe Browser B on the Current View page for the same period, outlet, and date.

**Test Result(s):**
1. Browser B locks the selected table with the same period, outlet, and date.
2. The table lock icon is displayed in Browser B without delay or error.

#### 1.2.Table unlock syncs when the table selection is cancelled

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the TMS environment.
2. WebSocket is connected on both browsers.
3. A table is currently locked and the lock icon is shown in Browser B.

**Step(s):**
1. In Browser A on the New Booking page, unselect the previously selected table.
2. Observe Browser B on the Current View page.

**Test Result(s):**
1. The table lock icon disappears in Browser B.
2. The table becomes selectable again in the Current View.

#### 1.3.Table lock and unlock sync in HKJC environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the HKJC environment.
2. WebSocket is connected on both browsers.
3. Browser A is on the New Booking page and Browser B is on the Current View page.

**Step(s):**
1. In Browser A, select the period and a table on the New Booking page and check the lock in Browser B.
2. Unselect the table in Browser A and check Browser B again.

**Test Result(s):**
1. The table lock is synced to Browser B when the table is selected.
2. The table lock is removed in Browser B when the table is unselected.

### 2.Reservation Data Synchronization After Booking

#### 2.1.New booking is updated in current view after successful booking in TMS environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the TMS environment.
2. WebSocket is connected on both browsers.
3. Browser A is on the New Booking page and Browser B is on the Current View page.

**Step(s):**
1. In Browser A, complete the booking process and make the booking successfully.
2. Observe Browser B on the Current View page.

**Test Result(s):**
1. Browser B updates and shows the new reservation in the current view after the booking is made.

#### 2.2.Quick table change updates table number across browsers in TMS environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the TMS environment.
2. WebSocket is connected on both browsers.
3. An existing reservation is available in the current view.

**Step(s):**
1. In Browser A, open the reservation details in the current view and perform a quick table change.
2. Observe the table number of the reservation in Browser B.

**Test Result(s):**
1. After the table quick change, Browser B updates and displays the new table number.

#### 2.3.Booking creation and quick table change sync in HKJC environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the HKJC environment.
2. WebSocket is connected on the involved browsers.

**Step(s):**
1. Using two browsers with New Booking and Current View, make a booking and check if the reservation is updated in the current view.
2. Using two browsers both on the Current View, test quick table change in the reservation details.
3. Observe the reservation data in the second browser after each operation.

**Test Result(s):**
1. The reservation made in Browser A appears in the Current View of Browser B.
2. After the quick table change, the table number is updated in the second browser without error.

### 3.Mark Arrival with POS

#### 3.1.Mark arrival changes table color in Operation and POS in TMS environment

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the TMS environment.
2. WebSocket is connected between Operation and POS.
3. A seated reservation is available on a table.

**Step(s):**
1. Click the mark arrival icon on the reservation in Operation.
2. Observe the table color in Operation and on POS.

**Test Result(s):**
1. The mark arrival operation succeeds.
2. Both the Operation view and the POS view change the table color accordingly.

#### 3.2.Mark arrival changes table color in HKJC environment with Browser and POS

**Prerequisite(s):**
1. The build with the durable queue change is deployed to the HKJC environment.
2. WebSocket is connected between the browser and POS.
3. A seated reservation is available on a table.

**Step(s):**
1. In the browser, click the mark arrival icon to perform mark arrival for the reservation.
2. Observe the table color on the POS side.

**Test Result(s):**
1. The mark arrival operation succeeds without error.
2. The color of the table is changed on the POS side.

### 4.Queue Durability Behavior

#### 4.1.Durable queue and persistent messages survive broker restart

**Prerequisite(s):**
1. The build with the durable queue change is deployed and TMS sendMessageToMQ declares the queue as durable.
2. Access to the RabbitMQ management console or CLI is available.
3. Permission to restart the RabbitMQ broker in the test environment.

**Step(s):**
1. Trigger TMS operations that publish messages through sendMessageToMQ, such as creating a booking, so that messages are sent to the durable queue.
2. Check in the RabbitMQ management console that the queue is created with the durable flag set to true.
3. Restart the RabbitMQ broker.
4. After the broker is back, check whether the queue is recreated and whether previously unconsumed persistent messages are restored.
5. Reconnect the WebSocket client and observe the message delivery behavior.

**Test Result(s):**
1. The queue is declared as durable and is automatically recreated after the broker restart.
2. Previously unconsumed persistent messages are restored after the restart.
3. The WebSocket client reconnects normally and there is no connection behavior regression compared with the non-durable setting for real-time operations such as table lock, booking sync, and mark arrival.
