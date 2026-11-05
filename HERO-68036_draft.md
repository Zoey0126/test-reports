# HERO-68036 Test Report

This enhancement enables KDS 2.0 to notify Infrasys POS when ticket items or courses are marked as Delivered. When an item or a course is updated from Completed to Delivered on the Expo screen or the Station screen, KDS sends a Mark Delivery request to POS through POSiApi so that POS knows which exact item(s) are marked as Delivered. The KDS interface settings are maintained in the KDS module of Infrasys Cloud, synchronized from Infrasys Cloud to the KDS 2.0 client, and the KDS 2.0 client notifies POS through the POSiApi mark delivery API once the item is delivered.

## Functional Testing

### 1. KDS Module Interface Configuration

#### 1.1. Endpoint Is Selected and Saved in the KDS Module

**Prerequisite(s):**
1. The Infrasys Cloud admin page is accessible with permission to System Management > KDS Module.
2. An endpoint is available for selection in the KDS module interface setup.
3. A KDS 2.0 device is registered and connected.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit" to open the KDS module settings.
3. Select the endpoint in the interface setup.
4. Click "Save".
5. Reopen the KDS module edit page.

**Test Result(s):**
1. The selected endpoint is saved and displayed when the KDS module edit page is reopened.
2. The configuration is saved locally and synchronized to KDS.
3. Mark Delivery requests are sent to the selected endpoint when items or courses are marked as Delivered in KDS.

#### 1.2. API Path Is Filled and Saved in the KDS Module

**Prerequisite(s):**
1. The Infrasys Cloud admin page is accessible with permission to System Management > KDS Module.
2. An endpoint is selected in the KDS module interface setup.
3. The API path value provided for the endpoint is available.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit".
3. Fill in the API path in the interface setup.
4. Click "Save".
5. Reopen the KDS module edit page.

**Test Result(s):**
1. The API path is saved and displayed correctly when the KDS module edit page is reopened.
2. The configuration is saved locally and synchronized to KDS.
3. The Mark Delivery request triggered from KDS is sent through the configured API path.

#### 1.3. Secret Key Is Filled and Saved in the KDS Module

**Prerequisite(s):**
1. The Infrasys Cloud admin page is accessible with permission to System Management > KDS Module.
2. The Secret Key is obtained from API Store.
3. An endpoint and API path are configured in the KDS module interface setup.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit".
3. Fill in the Secret Key from API Store.
4. Click "Save".
5. Reopen the KDS module edit page.

**Test Result(s):**
1. The Secret Key is saved and retained in the interface setup.
2. The configuration is saved locally and synchronized to KDS.
3. The saved Secret Key is used by the KDS Mark Delivery request and the request is processed by POS.

#### 1.4. Consumer Key Is Filled and Saved in the KDS Module

**Prerequisite(s):**
1. The Infrasys Cloud admin page is accessible with permission to System Management > KDS Module.
2. The Consumer Key is obtained from API Store.
3. An endpoint and API path are configured in the KDS module interface setup.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit".
3. Fill in the Consumer Key from API Store.
4. Click "Save".
5. Reopen the KDS module edit page.

**Test Result(s):**
1. The Consumer Key is saved and retained in the interface setup.
2. The configuration is saved locally and synchronized to KDS.
3. The saved Consumer Key is used by the KDS Mark Delivery request and the request is processed by POS.

#### 1.5. Consumer Secret Is Filled and Saved in the KDS Module

**Prerequisite(s):**
1. The Infrasys Cloud admin page is accessible with permission to System Management > KDS Module.
2. The Consumer Secret is obtained from API Store.
3. An endpoint and API path are configured in the KDS module interface setup.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit".
3. Fill in the Consumer Secret from API Store.
4. Click "Save".
5. Reopen the KDS module edit page.

**Test Result(s):**
1. The Consumer Secret is saved and retained in the interface setup.
2. The configuration is saved locally and synchronized to KDS.
3. The saved Consumer Secret is used by the KDS Mark Delivery request and the request is processed by POS.

#### 1.6. KDS Module Configuration Is Saved and Synchronized to KDS

**Prerequisite(s):**
1. The endpoint is selected and the API path, Secret Key, Consumer Key and Consumer Secret are filled in System Management > KDS Module.
2. The KDS 2.0 device is registered, powered on and reachable.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit" and confirm all interface settings.
3. Click "Save".
4. Check the KDS 2.0 client for the synchronized interface settings.

**Test Result(s):**
1. The configuration is saved locally in Infrasys Cloud.
2. The configuration is synchronized to KDS 2.0 successfully.
3. The KDS 2.0 client holds the synchronized interface settings and uses them for Mark Delivery requests.

#### 1.7. Synchronization Failure Displays an Error Message

**Prerequisite(s):**
1. The endpoint, API path, Secret Key, Consumer Key and Consumer Secret are filled in System Management > KDS Module.
2. The KDS 2.0 device is powered off or disconnected from the network so that synchronization cannot complete.

**Step(s):**
1. Go to the Infrasys Admin page, System Management > KDS Module.
2. Click "Edit" and confirm all interface settings.
3. Click "Save".
4. Observe the message displayed on the admin page.

**Test Result(s):**
1. The configuration is saved locally in Infrasys Cloud.
2. An error message is displayed because the synchronization to KDS failed.
3. The KDS 2.0 client does not receive the interface settings.

#### 1.8. Device Is Re-Synced from Kitchen Display System Module

**Prerequisite(s):**
1. The previous save of the KDS module configuration ended with a synchronization failure.
2. The KDS 2.0 device is powered on and connected to the network.

**Step(s):**
1. Go to Kitchen Display System > Device in the Infrasys Cloud admin page.
2. Select the affected KDS device.
3. Trigger the device re-sync.
4. Check the interface settings in the KDS 2.0 client.

**Test Result(s):**
1. The device re-sync is completed successfully.
2. The KDS module interface settings are synchronized to the KDS 2.0 client.
3. Mark Delivery requests from the KDS 2.0 client are processed by POS after the re-sync.

### 2. Mark Delivery Request Triggered from KDS 2.0

#### 2.1. Mark Finish Button on the Ticket Marks Items as Delivered in POS

**Prerequisite(s):**
1. The KDS module interface settings are configured, saved and synchronized to KDS 2.0.
2. A check with several items is sent from POS to the KDS 2.0 client.
3. The items are displayed in the KDS 2.0 client with status Completed.

**Step(s):**
1. Open the ticket in the KDS 2.0 client.
2. Use the mark finish button on the ticket.
3. Observe the Mark Delivery request sent to POS.
4. Check the corresponding check in the POS client.

**Test Result(s):**
1. KDS sends a Mark Delivery request to POS for the delivered item(s) of the ticket.
2. The processing flag of the ticket is updated to Delivered in the KDS 2.0 client.
3. The corresponding item(s) are marked as "Delivered" in the POS client.

#### 2.2. Single Item Updated to Delivered Is Marked Delivered in POS

**Prerequisite(s):**
1. The KDS module interface settings are configured, saved and synchronized to KDS 2.0.
2. A check with more than one item is sent from POS to the KDS 2.0 client.
3. All items are displayed in the KDS 2.0 client with status Completed.

**Step(s):**
1. Update one single item to Delivered in the KDS 2.0 client.
2. Observe the Mark Delivery request sent to POS.
3. Check the corresponding check in the POS client.

**Test Result(s):**
1. KDS sends a Mark Delivery request to POS for the item updated to Delivered.
2. Only the updated item is marked as "Delivered" in the POS client.
3. The remaining items of the check keep their previous status.

#### 2.3. Course Updated to Delivered Marks Its Items Delivered in POS

**Prerequisite(s):**
1. The KDS module interface settings are configured, saved and synchronized to KDS 2.0.
2. A check containing at least two courses is sent from POS to the KDS 2.0 client.
3. The courses are displayed in the KDS 2.0 client with status Completed.

**Step(s):**
1. Update one course to Delivered in the KDS 2.0 client.
2. Observe the Mark Delivery request sent to POS.
3. Check the corresponding check in the POS client.

**Test Result(s):**
1. KDS sends a Mark Delivery request to POS for the items of the course updated to Delivered.
2. The items of the updated course are marked as "Delivered" in the POS client.
3. The items of the other courses keep their previous status.

### 3. Mark Delivery Failure Notification Is Displayed in KDS Client

**Prerequisite(s):**
1. The KDS module interface settings are configured and synchronized to KDS 2.0.
2. The POS endpoint is unreachable from the KDS 2.0 client so that the Mark Delivery request fails.
3. A check with items in status Completed is displayed in the KDS 2.0 client.

**Step(s):**
1. Update the items or the course to Delivered in the KDS 2.0 client.
2. Observe the KDS 2.0 client after the Mark Delivery request fails.
3. Check the corresponding check in the POS client.

**Test Result(s):**
1. The Mark Delivery request to POS fails.
2. A system notification appears in the KDS 2.0 client for the failed Mark Delivery.
3. The item(s) are not marked as "Delivered" in the POS client.

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-68036 |
| Summary | KDS2.0 Printing - KDS trigger POS mark delivery API to print runner chit - Dev |
| Issue Type | New Feature |
| Test Target | KDS module interface setup in Infrasys Cloud, synchronization to KDS 2.0, Mark Delivery request from KDS 2.0 to Infrasys POS |
| Infrasys Cloud | Admin page with System Management > KDS Module and Kitchen Display System > Device |
| KDS | KDS 2.0 client with Expo screen and Station screen |
| Infrasys POS | POS client with the mark delivery function and item Delivered status |
| API Store | Endpoint, API path, Secret Key, Consumer Key and Consumer Secret for the KDS interface |
| New Setup | KDS module interface setup: endpoint, API path, Secret Key, Consumer Key and Consumer Secret |
| Verification Points | Configuration save and synchronization; synchronization failure and device re-sync; mark delivery by ticket, single item and course; mark delivery failure notification |
