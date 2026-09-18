# KDS2.0 - Support to view ticket when disconnect with internet Test Report

## Functional Testing

### 1.Offline Ticket Viewing And PWA Caching

#### 1.1.View Existing Tickets After Internet Disconnects
**Prerequisite(s):**
1. The KDS client is signed in and the tickets page is open with internet access
2. At least one active ticket is displayed on the screen
3. The service worker is registered and the page data is cached in local storage

**Step(s):**
1. Disconnect the network on the KDS client station
2. Observe the cloud and signal icons on the page
3. Refresh the page or navigate between pages while offline
4. Verify that the existing tickets are still visible

**Test Result(s):**
1. The cloud icon and signal icon turn red when the KDS backend and MQTT disconnect
2. The page loads from the cached data and the existing tickets are displayed normally while offline
3. Users can switch pages, select or unselect processing statuses and change display options while offline

#### 1.2.Service Worker Registration And Cache Management
**Prerequisite(s):**
1. A fresh browser session is available on the KDS client station
2. Network access to the KDS backend is available

**Step(s):**
1. Open the KDS client URL and sign in
2. Open the browser developer tools and inspect the registered service workers
3. Check the cached page data in local storage and IndexedDB

**Test Result(s):**
1. A service worker is automatically registered when opening the KDS client
2. The page data is cached in local storage and IndexedDB is used as the fallback data source for failed HTTP API calls
3. The cached data includes display list and device info used by the KDS client

### 2.Offline Sign In And Page Reload Behavior

#### 2.1.Sign In Requires Online Access
**Prerequisite(s):**
1. The KDS client station is fully offline
2. No previous signed-in session is active on the station

**Step(s):**
1. Open the KDS client URL while offline
2. Attempt to sign in with valid credentials

**Test Result(s):**
1. The KDS client cannot acquire an access token from the KDS backend while offline
2. Sign in is blocked and an appropriate message is shown indicating that sign in requires internet access

#### 2.2.Page Reload Of Signed In Client While Offline
**Prerequisite(s):**
1. The KDS client is signed in with internet access
2. The tickets page is open

**Step(s):**
1. Disconnect the network on the station
2. Perform a full page reload in the browser
3. Observe the page behavior after reload

**Test Result(s):**
1. The KDS client automatically skips the sign in page and loads the cached tickets page
2. The page displays the same data as before the reload and shows the page normally even while offline

### 3.New Version Notification And Upgrade Flow

#### 3.1.New Version Notification On Tickets Page
**Prerequisite(s):**
1. A new version of the KDS Client has been published
2. The user is signed in and has not performed any page refresh since the new version was published

**Step(s):**
1. Open the KDS client and navigate to the tickets page
2. Observe the bottom right area of the page
3. Click the upgrade button in the notification box

**Test Result(s):**
1. A notification box pops up in the bottom right of the tickets page prompting that a new version is available
2. Clicking the upgrade button triggers a page reload to the latest version

#### 3.2.Upgrade Button In System Information Menu
**Prerequisite(s):**
1. A new version of the KDS Client has been published
2. The user is signed in on the tickets page

**Step(s):**
1. Open the settings menu on the tickets page
2. Navigate to System Information
3. Locate the upgrade button
4. Click the upgrade button

**Test Result(s):**
1. The upgrade button is available in settings -> System Information menu on the tickets page
2. Clicking the upgrade button triggers a page reload to the latest version

### 4.Connection Recovery And Data Synchronization

#### 4.1.Update Tickets After Connection Resumes
**Prerequisite(s):**
1. The KDS client is offline and displaying cached tickets
2. At least one new ticket status change exists on the AWS cloud that has not been synchronized

**Step(s):**
1. Reconnect the network on the KDS client station
2. Wait for the KDS client to detect the restored connection
3. Observe the tickets list after the connection is restored

**Test Result(s):**
1. The cloud icon and signal icon return to normal after internet access is restored
2. The KDS client calls the backend API to load the latest tickets
3. All tickets are updated to the latest status from the AWS cloud after the connection returns to normal

### 5.Cross Screen Offline Alerts

#### 5.1.Alert When Current Screen Disconnects From KDS Cloud
**Prerequisite(s):**
1. Multiple KDS screens are running and signed in to the KDS cloud
2. At least two screens are operating on different stations

**Step(s):**
1. Disconnect one screen from the KDS cloud by removing its network access
2. Observe the disconnected screen
3. Observe the other connected screens

**Test Result(s):**
1. The disconnected screen displays a prompt alert indicating it has disconnected from the KDS cloud
2. The connected screens receive a notification or alert that another screen is offline or disconnected from the KDS cloud
3. All screens can still view existing tickets while any screen is offline
