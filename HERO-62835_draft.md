# TMS 2.0 - Incorrect table used to mark arrival when table changed & WS disconnected Test Report

## Functional Testing

### 1.Mark Arrival After Quick Table Change

#### 1.1.Mark arrival uses the updated table when WebSocket is disconnected

**Prerequisite(s):**
1. A reservation is booked with table 1A and is displayed in the TMS 2.0 reservation list.
2. The WebSocket (WS) connection between the frontend and the server is disconnected (e.g., simulate a network interruption).

**Step(s):**
1. While the WS is disconnected, change the table via quick table change in the reservation list from 1A to 2A.
2. Mark arrival for the reservation.
3. Check the arrival status on the POS side.

**Reproduction Result(s):**
1. The old table 1A is used to mark arrival, and the POS side shows the arrival marked on the outdated table.

**Fix Result(s):**
1. The most updated table 2A is used to mark arrival.
2. The POS side shows the arrival marked on table 2A.

#### 1.2.Mark arrival uses the updated table when WebSocket is connected

**Prerequisite(s):**
1. A reservation is booked with table 1A and is displayed in the TMS 2.0 reservation list.
2. The WebSocket (WS) connection between the frontend and the server is connected.

**Step(s):**
1. Change the table via quick table change in the reservation list from 1A to 2A.
2. Mark arrival for the reservation.
3. Check the arrival status on the POS side.

**Reproduction Result(s):**
1. Not applicable - the WS-connected flow was not affected by the reported issue.

**Fix Result(s):**
1. The most updated table 2A is used to mark arrival and the POS side displays the arrival on table 2A.

#### 1.3.Mark arrival dialog retrieves reservation tables from the server

**Prerequisite(s):**
1. A reservation is booked with table 1A.
2. The WebSocket (WS) connection between the frontend and the server is disconnected.

**Step(s):**
1. While the WS is disconnected, change the table via quick table change in the reservation list from 1A to 2A.
2. Open the mark arrival dialog for the reservation.

**Reproduction Result(s):**
1. The dialog uses the record stored in the frontend and displays the outdated table 1A.

**Fix Result(s):**
1. The dialog retrieves the corresponding reservation tables by reservation ID via the internal API and displays the most updated table 2A.
2. No loader is shown before the dialog and the dialog opens without a noticeable delay.

#### 1.4.Mark arrival without table change uses the original booked table

**Prerequisite(s):**
1. A reservation is booked with table 1A and no table change is performed.
2. The WebSocket (WS) connection between the frontend and the server is disconnected.

**Step(s):**
1. Mark arrival for the reservation without changing the table.
2. Check the arrival status on the POS side.

**Reproduction Result(s):**
1. Not applicable - no table change was involved in the reported issue.

**Fix Result(s):**
1. The original booked table 1A is used to mark arrival and the POS side displays the arrival on table 1A.

### 2.Table Change Synchronization Scenarios

#### 2.1.Multiple successive table changes then mark arrival

**Prerequisite(s):**
1. A reservation is booked with table 1A.
2. The WebSocket (WS) connection between the frontend and the server is disconnected.

**Step(s):**
1. Change the table from 1A to 2A, then change it again from 2A to 3A via quick table change in the reservation list.
2. Mark arrival for the reservation.

**Reproduction Result(s):**
1. An outdated table is used to mark arrival.

**Fix Result(s):**
1. The latest table 3A is used to mark arrival and the POS side displays the arrival on table 3A.

#### 2.2.WebSocket reconnected after disconnection then mark arrival

**Prerequisite(s):**
1. A reservation is booked with table 1A.
2. The WebSocket (WS) connection between the frontend and the server is disconnected, then reconnected.

**Step(s):**
1. While the WS is disconnected, change the table from 1A to 2A via quick table change in the reservation list.
2. Restore the WS connection.
3. Mark arrival for the reservation.

**Reproduction Result(s):**
1. An outdated table may be used to mark arrival depending on the data cached in the frontend.

**Fix Result(s):**
1. The most updated table 2A is used to mark arrival.

#### 2.3.Table changed by another terminal while WebSocket is disconnected

**Prerequisite(s):**
1. A reservation is booked with table 1A and opened on two terminals (A and B).
2. The WebSocket (WS) connection of terminal A is disconnected.

**Step(s):**
1. From terminal B, change the table of the reservation from 1A to 2A.
2. From terminal A (WS still disconnected), mark arrival for the reservation.

**Reproduction Result(s):**
1. Terminal A uses the old table 1A to mark arrival.

**Fix Result(s):**
1. Terminal A uses the most updated table 2A to mark arrival and the POS side shows the arrival on table 2A.

### 3.Regression Scope

1. Normal mark arrival flow without any table change.
2. Quick table change in the reservation list with the WS connected.
3. POS side table status and arrival display after marking arrival.
4. WebSocket disconnection and reconnection handling for other reservation actions.
5. Reservation list display of the table assignment after a table change.
