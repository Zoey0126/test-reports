# HERO-67615 KDS2.0 - Create new API to show ticket history in client Test Report

## Functional Testing

### 1.Ticket History Dialog and API Display

#### 1.1.Open ticket history dialog via hamburger icon button

**Prerequisite(s):**
1. KDS client is running and at least one ticket exists on the KDS screen.
2. The user has access to the KDS client.

**Step(s):**
1. Open the KDS client and locate an existing ticket on the screen.
2. On the top right corner of the ticket, verify the hamburger icon button is displayed.
3. Click the hamburger icon button.
4. Observe the ticket history dialog opening.

**Test Result(s):**
1. The hamburger icon button is visible on the top right corner of the ticket.
2. Clicking the button opens the ticket history dialog.
3. The dialog shows the ticket history content for the selected ticket.

#### 1.2.Loading skeleton shown while calling the ticket history API

**Prerequisite(s):**
1. KDS client is running and at least one ticket exists on the KDS screen.
2. Network or backend latency is observable (or use a ticket with a large history).

**Step(s):**
1. Open the KDS client and select a ticket.
2. Click the hamburger icon button to open the ticket history dialog.
3. Observe the dialog content immediately after clicking and while waiting for the API response.
4. Wait for the API response to complete.

**Test Result(s):**
1. A loading skeleton is shown in the dialog while the API is being called and the full response has not yet returned.
2. After the full response is received, the loading skeleton is replaced by the ticket history content.
3. The dialog does not show an empty or broken layout during loading.

#### 1.3.Ticket history timeline is shown in chronological order with latest on top

**Prerequisite(s):**
1. KDS client is running and a ticket with multiple history events exists.
2. The ticket history API is reachable.

**Step(s):**
1. Open the KDS client and select a ticket that has multiple recorded history events.
2. Click the hamburger icon button to open the ticket history dialog.
3. Verify the order of the displayed history entries.
4. Compare the timestamps of the entries shown in the dialog.

**Test Result(s):**
1. The ticket history entries are displayed in a chronological timeline.
2. The latest event is shown on top of the timeline.
3. Earlier events are displayed below in descending order of time.

### 2.Ticket and Item Creation Events Logging

#### 2.1.Ticket Created event is logged in ticket history

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A new ticket can be created from POS.

**Step(s):**
1. At the POS, create a new ticket/order sent to the kitchen.
2. On the KDS client, open the ticket and click the hamburger icon button to open the ticket history dialog.
3. Verify the first event shown in the history.

**Test Result(s):**
1. A "Ticket Created" event is logged in the ticket history.
2. The event shows the timestamp when the ticket was created.
3. The event records the station or system component responsible for the action.

#### 2.2.Item created event is logged in ticket history

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A ticket has been created on the KDS.

**Step(s):**
1. At the POS, add a new item to an existing ticket and send it to the kitchen.
2. On the KDS client, open the ticket history dialog.
3. Verify the event related to the new item.

**Test Result(s):**
1. An "Item created" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the entry/course/menu related to the item.
3. The station or system component responsible is recorded.

#### 2.3.Item course created and Item menu created events are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A menu/course structure is configured so items are sent with course or menu information.

**Step(s):**
1. At the POS, create an item that is assigned to a course and/or menu, then send it to the kitchen.
2. On the KDS client, open the ticket history dialog.
3. Verify the events for course creation and menu creation.

**Test Result(s):**
1. An "Item course created" event is logged when the item is associated with a course.
2. An "Item menu created" event is logged when the item is associated with a menu.
3. Both events show the timestamp, event type name, and the relevant entry/course/menu information.

### 3.Item Update Events from POS

#### 3.1.Item quantity updated at POS is logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item has already been sent to the kitchen (fired) on an existing ticket.

**Step(s):**
1. At the POS, update the quantity of the fired item (increase or decrease).
2. On the KDS client, open the ticket history dialog.
3. Verify the event related to the quantity update.

**Test Result(s):**
1. An "Item quantity updated at POS" event is logged in the ticket history.
2. The event shows the timestamp and the affected entry.
3. The station or system component responsible for the update is recorded.

#### 3.2.Item updated, item voided, and item modifier updated at POS are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item with modifiers exists on a ticket sent to the kitchen.

**Step(s):**
1. At the POS, update the item (e.g., change item details), then void the item, and update a modifier on another item.
2. On the KDS client, open the ticket history dialog.
3. Verify the events for the item update, item void, and modifier update.

**Test Result(s):**
1. An "Item updated at POS" event is logged when the item is updated.
2. An "Item voided at POS" event is logged when the item is voided.
3. An "Item modifier updated at POS" event is logged when the modifier is updated.
4. Each event shows the timestamp, event type name, and the relevant entry.

#### 3.3.Item menu updated and item course updated at POS are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item assigned to a course and/or menu exists on a ticket sent to the kitchen.

**Step(s):**
1. At the POS, update the menu and the course of the item.
2. On the KDS client, open the ticket history dialog.
3. Verify the events for the menu update and course update.

**Test Result(s):**
1. An "Item menu updated at POS" event is logged when the menu is changed.
2. An "Item course updated at POS" event is logged when the course is changed.
3. Both events show the timestamp, event type name, and the relevant entry/course/menu information.

### 4.Addon Item Events at KDS

#### 4.1.Addon Item created at KDS when item quantity is increased at POS

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item has already been sent to the kitchen (fired) on a ticket.

**Step(s):**
1. At the POS, increase the quantity of the fired item.
2. On the KDS client, observe that an addon item is automatically generated for the additional quantity.
3. Open the ticket history dialog and verify the event.

**Test Result(s):**
1. An "Addon Item created at KDS" event is logged when the item quantity is increased at POS.
2. The KDS automatically generates an addon item for the additional quantity.
3. The event shows the timestamp, event type name, and the relevant entry.

#### 4.2.Addon Item void at KDS when item quantity is reduced to zero at POS

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item with an associated addon item exists on a fired ticket.

**Step(s):**
1. At the POS, reduce the item quantity to zero.
2. On the KDS client, observe that the associated addon items are automatically voided.
3. Open the ticket history dialog and verify the event.

**Test Result(s):**
1. An "Addon Item void at KDS" event is logged when the item quantity is reduced to zero at POS.
2. The KDS automatically voids the associated addon items.
3. The event shows the timestamp, event type name, and the relevant entry.

### 5.Ticket-level Events from POS

#### 5.1.Ticket updated, ticket voided, and ticket merged at POS are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. At least two tickets exist on the KDS.

**Step(s):**
1. At the POS, update the ticket information on one ticket, void another ticket, and merge two tickets.
2. On the KDS client, open the ticket history dialog for the affected tickets.
3. Verify the events for the ticket update, ticket void, and ticket merge.

**Test Result(s):**
1. A "Ticket updated (Ticket Information updated) at POS" event is logged when ticket information is changed.
2. A "Ticket voided at POS" event is logged when the ticket is voided.
3. A "Ticket merged at POS" event is logged when tickets are merged.
4. Each event shows the timestamp and the station or system component responsible.

#### 5.2.Table changed at POS is logged in ticket history

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A ticket assigned to a table exists on the KDS.

**Step(s):**
1. At the POS, change the table of the ticket.
2. On the KDS client, open the ticket history dialog.
3. Verify the event related to the table change.

**Test Result(s):**
1. A "Table changed at POS" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry.
3. The station or system component responsible for the table change is recorded.

#### 5.3.Ticket marked as finished at KDS is logged

**Prerequisite(s):**
1. KDS client is running and a ticket with items exists on the KDS.
2. The user has permission to mark a ticket as finished on the KDS.

**Step(s):**
1. On the KDS client, mark the ticket as finished.
2. Open the ticket history dialog for the finished ticket.
3. Verify the event related to finishing the ticket.

**Test Result(s):**
1. A "Ticket marked as finished at KDS" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry.
3. The station or system component responsible for the action is recorded.

### 6.Rush, Pantry Message, and Course/Menu Update Events

#### 6.1.Rush item at POS is logged in ticket history

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. An item exists on a ticket sent to the kitchen.

**Step(s):**
1. At the POS, mark the item as a rush order.
2. On the KDS client, open the ticket history dialog.
3. Verify the event related to the rush item.

**Test Result(s):**
1. A "Rush item at POS" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry.
3. The station or system component responsible for the rush action is recorded.

#### 6.2.Pantry message created to item and to ticket at POS are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A ticket with at least one item exists on the KDS.

**Step(s):**
1. At the POS, create a pantry message attached to an item on the ticket.
2. At the POS, create a pantry message attached to the ticket itself.
3. On the KDS client, open the ticket history dialog.
4. Verify the events for both pantry messages.

**Test Result(s):**
1. A "Pantry message created to item at POS" event is logged when a pantry message is added to an item.
2. A "Pantry message created to the ticket at POS" event is logged when a pantry message is added to the ticket.
3. Both events show the timestamp, event type name, and the relevant entry.

#### 6.3.Update course and update menu at POS are logged

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. A ticket with course and menu structure exists on the KDS.

**Step(s):**
1. At the POS, update the course and update the menu on the ticket.
2. On the KDS client, open the ticket history dialog.
3. Verify the events for the course update and menu update.

**Test Result(s):**
1. An "Update course at POS" event is logged when the course is updated.
2. An "Update menu at POS" event is logged when the menu is updated.
3. Both events show the timestamp, event type name, and the relevant entry/course/menu information.

### 7.Item Processing Status Events at KDS

#### 7.1.Item processing status updated at KDS is logged

**Prerequisite(s):**
1. KDS client is running and a ticket with items exists on the KDS.
2. The user has permission to update the item processing status on the KDS.

**Step(s):**
1. On the KDS client, update the processing status of an item (e.g., start cooking, mark as done).
2. Open the ticket history dialog.
3. Verify the event related to the item processing status update.

**Test Result(s):**
1. An "Item processing status updated at KDS" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry.
3. The station or system component responsible for the status update is recorded.

#### 7.2.Addon item processing status updated at KDS is logged

**Prerequisite(s):**
1. KDS client is running and a ticket with an addon item exists on the KDS.
2. The user has permission to update the addon item processing status on the KDS.

**Step(s):**
1. On the KDS client, update the processing status of the addon item.
2. Open the ticket history dialog.
3. Verify the event related to the addon item processing status update.

**Test Result(s):**
1. An "Addon item processing status updated at KDS" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry.
3. The station or system component responsible for the status update is recorded.

#### 7.3.Item processing status updated by course is logged

**Prerequisite(s):**
1. KDS client is running and a ticket with items grouped by course exists on the KDS.
2. The user has permission to update the processing status by course on the KDS.

**Step(s):**
1. On the KDS client, update the processing status of items grouped by course.
2. Open the ticket history dialog.
3. Verify the event related to the item processing status update by course.

**Test Result(s):**
1. An "Item processing status updated by course" event is logged in the ticket history.
2. The event shows the timestamp, event type name, and the relevant entry/course information.
3. The station or system component responsible for the status update is recorded.

### 8.Ticket History Event Format Verification

#### 8.1.Event rows show timestamp, event type name, and entry/course/menu

**Prerequisite(s):**
1. KDS client is running and a ticket with multiple types of history events exists.

**Step(s):**
1. Open the KDS client and select a ticket that has multiple recorded history events of different types.
2. Click the hamburger icon button to open the ticket history dialog.
3. For several events of different types, verify the three rows of each event entry.

**Test Result(s):**
1. The first row of each event entry is the timestamp that the action was performed.
2. The second row of each event entry is the event type name.
3. The third row of each event entry is the entry/course/menu related to the event.
4. The timestamp, event type, and entry/course/menu are displayed correctly for each event type.

#### 8.2.Entry and course/menu combined display when both are involved

**Prerequisite(s):**
1. KDS client is running and a ticket with events involving both an entry and a course/menu exists.

**Step(s):**
1. Open the KDS client and select a ticket that has events where both an entry and a course/menu are involved.
2. Click the hamburger icon button to open the ticket history dialog.
3. Verify the third row display format for those events.

**Test Result(s):**
1. When an event involves both an entry and a course/menu, the third row is shown as "{entry name} · {course/menu name}".
2. The combined display correctly shows both the entry name and the course/menu name separated by the "·" character.
3. Events involving only an entry or only a course/menu show the corresponding single value.

#### 8.3.Station or system component responsible is recorded for each action

**Prerequisite(s):**
1. POS client and KDS client are running and connected.
2. Multiple actions have been performed on a ticket from different stations/components.

**Step(s):**
1. Perform several actions on a ticket from different stations (e.g., POS station and KDS station).
2. On the KDS client, open the ticket history dialog.
3. For each event, verify the station or system component responsible for the action is recorded.

**Test Result(s):**
1. Each event entry records the station or system component responsible for the action.
2. Events triggered at the POS show the POS station as the responsible component.
3. Events triggered at the KDS show the KDS station as the responsible component.
4. The station/component information is accurate and consistent with where the action was performed.
