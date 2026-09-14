# TMS 2.0 - Error occurs when clicking reservation in Operation after creating or editing reservation online Test Report

## Functional Testing

### 1.Viewing Online Booking Details in Operation

#### 1.1.View newly created online booking details after WebSocket push

**Prerequisite(s):**
1. WebSocket is enabled between the online booking portal and TMS Operation.
2. Operation is logged in and stays on the Current View page with the booking outlet and period selected.
3. Access to the online booking portal is available for creating a booking.

**Step(s):**
1. In Operation, enter the Current View and stay on the booking outlet and period selection screen.
2. Create an online booking in the online portal.
3. After the WebSocket pushes the booking record to Operation, click the booking to view its details.

**Reproduction Result(s):**
1. An error prompt is shown: "Something went wrong. Error: Cannot read properties of undefined (reading 'length')".
2. The booking details page cannot be opened.

**Fix Result(s):**
1. The page does not return an error and the booking details can be viewed normally after the WebSocket push.

#### 1.2.View edited online booking details after WebSocket push

**Prerequisite(s):**
1. WebSocket is enabled between the online booking portal and TMS Operation.
2. The outlet has an existing reservation, for example Confirmation No. E0007.
3. Access to the online booking portal retrieve and edit functions is available.

**Step(s):**
1. Stay on the TMS Operation Current View page for the booking outlet.
2. In the online portal, click "Retrieve Booking Record", input the reservation information, and click the "Search" button.
3. Click the "Edit" button, change the number of people, and click the "Submit" button.
4. Immediately go back to TMS Operation and click the reservation with Confirmation No. E0007 to view its details.

**Reproduction Result(s):**
1. An error prompt is shown: "Something went wrong. Error: Cannot read properties of undefined (reading 'length')" when clicking the reservation.

**Fix Result(s):**
1. The reservation record page is entered normally and the updated number of people is displayed correctly.

#### 1.3.View booking details with payment after online payment completes

**Prerequisite(s):**
1. WebSocket is enabled between the online booking portal and TMS Operation.
2. Period configuration payment is enabled for the outlet.
3. The online portal payment flow is available.

**Step(s):**
1. Create an online booking and complete the payment in the online portal.
2. After the WebSocket pushes the data to Operation, click the booking to view its details.

**Reproduction Result(s):**
1. Clicking the pushed booking shows the error prompt "Something went wrong. Error: Cannot read properties of undefined (reading 'length')" instead of the details page.

**Fix Result(s):**
1. The booking details are displayed normally and the payment amount can be seen in the details.

### 2.Edge and Timing Scenarios

#### 2.1.Click reservation immediately and after a delay following WebSocket push

**Prerequisite(s):**
1. WebSocket is enabled between the online booking portal and TMS Operation.
2. Operation is logged in and stays on the Current View page.

**Step(s):**
1. Create an online booking and, immediately after the WebSocket push arrives, click the booking in Operation to view the details.
2. Repeat the flow, but wait for several seconds after the push before clicking the booking.
3. Compare the behavior of the two attempts.

**Reproduction Result(s):**
1. Clicking immediately after the push triggered the error prompt "Something went wrong. Error: Cannot read properties of undefined (reading 'length')".

**Fix Result(s):**
1. Both the immediate click and the delayed click open the booking details normally without any error prompt.

#### 2.2.Open booking details when WebSocket connection is interrupted and then restored

**Prerequisite(s):**
1. WebSocket is enabled and currently connected between the online booking portal and TMS Operation.
2. Operation is logged in and stays on the Current View page.

**Step(s):**
1. Interrupt the WebSocket connection on the Operation side.
2. Create an online booking in the portal, then restore the WebSocket connection so the booking data is synced to Operation.
3. Click the pushed booking to view its details.

**Reproduction Result(s):**
1. Opening the details of the synced booking could trigger the error prompt "Something went wrong. Error: Cannot read properties of undefined (reading 'length')" because the pushed data was incomplete.

**Fix Result(s):**
1. After the connection is restored and the data is synced, the booking details open normally without any error prompt.

### 3.Regression Scope

The following related modules and scenarios should be retested after the fix:

1. Reservation details viewing in Operation Current View for both newly created and edited online bookings.
2. WebSocket push of booking records from the online portal to Operation, including the outlet and period selection flow.
3. Period configuration payment display in the booking details page.
4. Other detail pages in Operation that read the same booking data structure pushed by WebSocket.
