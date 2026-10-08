# HERO-68555 [Tech] Production diagnostic logger for TMS Operation Test Report

## Functional Testing

### 1.Diagnostic Logger Core - Initialization and Storage

#### 1.1.Logger initializes with IndexedDB store and queue on TMS Operation startup

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. TMS Operation module is accessible.
3. The browser supports IndexedDB.

**Step(s):**
1. Open the browser developer tools (Application > IndexedDB) before loading TMS Operation.
2. Load TMS Operation and log in.
3. Verify the IndexedDB store for the diagnostic logger is created.
4. Verify the logger queue is initialized.

**Test Result(s):**
1. The diagnostic logger initializes on TMS Operation startup.
2. An IndexedDB store is created for the diagnostic logger.
3. The logger queue is ready to accept events.
4. No errors are thrown during initialization.

#### 1.2.Logger does not interfere with the existing ActionLogger

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. The existing ActionLogger is functional.

**Step(s):**
1. Load TMS Operation and perform actions that trigger the existing ActionLogger.
2. Verify the ActionLogger records actions as expected.
3. Verify the diagnostic logger operates independently without affecting ActionLogger behavior.

**Test Result(s):**
1. The existing ActionLogger continues to record actions as before.
2. The diagnostic logger does not interfere with or duplicate ActionLogger behavior.
3. Both loggers operate independently.

### 2.Consent and Kill Switch

#### 2.1.User consent is required before the diagnostic logger collects data

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. A fresh user session with no prior consent given.

**Step(s):**
1. Load TMS Operation with a fresh session.
2. Verify the diagnostic logger does not collect data before consent is given.
3. Open the Preference UI and grant consent.
4. Perform user actions (clicks, navigation) and verify the logger starts collecting data.

**Test Result(s):**
1. Before consent is granted, the diagnostic logger does not collect or store user behavior data.
2. After consent is granted, the logger begins collecting clicks, form submits, and navigation events.
3. Consent state is persisted and cached on the logger.

#### 2.2.Kill switch disables the diagnostic logger entirely

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. The diagnostic logger has been enabled and consent has been granted.
3. Access to the kill switch configuration is available.

**Step(s):**
1. Enable the kill switch (e.g., via a configuration or feature flag).
2. Load TMS Operation and perform user actions.
3. Verify the diagnostic logger does not collect any data while the kill switch is active.
4. Disable the kill switch and verify the logger resumes collecting data.

**Test Result(s):**
1. When the kill switch is enabled, the diagnostic logger is completely disabled.
2. No data is collected or stored while the kill switch is active.
3. After the kill switch is disabled, the logger resumes collecting data (if consent was previously granted).

### 3.Collectors - Click, Form Submit, and Navigation

#### 3.1.Click collector records user click events

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.

**Step(s):**
1. Load TMS Operation and navigate to a page with clickable elements.
2. Click several buttons, links, and interactive elements.
3. Open the Preference UI and trigger a log upload or inspect the IndexedDB store.
4. Verify the click events are recorded with relevant context (element, timestamp).

**Test Result(s):**
1. Click events are recorded by the click collector.
2. Each recorded click includes the relevant context (e.g., element identifier or label) and timestamp.
3. Clicks on non-interactive elements are not recorded or are filtered appropriately.

#### 3.2.Form submit collector records form submission events

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. A form (e.g., reservation creation, settings form) is available.

**Step(s):**
1. Load TMS Operation and navigate to a form (e.g., New Reservation).
2. Fill in the form fields and submit the form.
3. Inspect the IndexedDB store or trigger a log upload.
4. Verify the form submission event is recorded.

**Test Result(s):**
1. Form submission events are recorded by the form submit collector.
2. The recorded event includes the form context and timestamp.
3. Sensitive form field values (e.g., passwords, tokens) are sanitized and not stored.

#### 3.3.Navigation collector records page navigation events

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.

**Step(s):**
1. Load TMS Operation and navigate between different pages/modules.
2. Inspect the IndexedDB store or trigger a log upload.
3. Verify the navigation events are recorded.

**Test Result(s):**
1. Navigation events are recorded by the navigation collector.
2. Each recorded navigation includes the source and destination page/module and timestamp.
3. Navigation events are captured for both initial loads and in-app routing.

### 4.API Collector - Late-Bound Axios Hook

#### 4.1.API errors are always logged

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. An API endpoint that returns an error is accessible.

**Step(s):**
1. Load TMS Operation and trigger an API call that returns an error (e.g., a 4xx or 5xx response).
2. Inspect the IndexedDB store or trigger a log upload.
3. Verify the API error event is recorded.

**Test Result(s):**
1. API errors are always logged by the API collector, regardless of response time.
2. The recorded event includes the API endpoint, error status, and timestamp.
3. The request/response body is not stored by default (privacy sanitization).

#### 4.2.Slow API responses (>= 2 seconds) are logged

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. An API endpoint with a response time of 2 seconds or more is accessible (or simulate latency).

**Step(s):**
1. Load TMS Operation and trigger an API call that takes 2 or more seconds to respond.
2. Inspect the IndexedDB store or trigger a log upload.
3. Verify the slow API response event is recorded.

**Test Result(s):**
1. API responses with a duration of 2 seconds or more are logged.
2. The recorded event includes the API endpoint, response duration, and timestamp.
3. The response body is not stored by default.

#### 4.3.Fast successful API responses are not logged and the tms/logs endpoint is ignored

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. A fast-responding API endpoint (under 2 seconds, success) and the /rest_api/tms/logs endpoint are accessible.

**Step(s):**
1. Load TMS Operation and trigger a fast successful API call (response time under 2 seconds).
2. Trigger a call to the /rest_api/tms/logs endpoint.
3. Inspect the IndexedDB store or trigger a log upload.
4. Verify neither the fast success nor the tms/logs call is logged.

**Test Result(s):**
1. Fast successful API responses (under 2 seconds) are not logged.
2. Calls to the /rest_api/tms/logs endpoint are ignored and not logged.
3. The API collector only logs errors and slow responses, reducing noise and storage usage.

### 5.Error and React Boundary Collector

#### 5.1.Uncaught errors are captured by the error collector

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. A scenario that triggers an uncaught JavaScript error is reproducible.

**Step(s):**
1. Load TMS Operation and trigger an uncaught JavaScript error (e.g., via a faulty interaction or test injection).
2. Inspect the IndexedDB store or trigger a log upload.
3. Verify the uncaught error is recorded.

**Test Result(s):**
1. Uncaught errors are captured by the error collector.
2. The recorded event includes the error message, stack trace (if available), and timestamp.
3. The error is recorded without crashing the application.

#### 5.2.React boundary errors are captured

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. A React component that throws an error within an error boundary is reproducible.

**Step(s):**
1. Load TMS Operation and trigger a React component error that is caught by an error boundary.
2. Inspect the IndexedDB store or trigger a log upload.
3. Verify the React boundary error is recorded.

**Test Result(s):**
1. React boundary errors are captured by the error collector.
2. The recorded event includes the error details and timestamp.
3. The error boundary fallback UI is displayed while the error is logged.

### 6.Privacy and Sanitization

#### 6.1.Sensitive keys are sanitized and passwords/tokens/bodies are not stored by default

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. A form or API call involving sensitive data (passwords, tokens, request bodies) is available.

**Step(s):**
1. Load TMS Operation and submit a form containing password or token fields.
2. Trigger an API call that includes a request body with sensitive data.
3. Inspect the IndexedDB store or trigger a log upload.
4. Verify the sensitive data is sanitized or excluded.

**Test Result(s):**
1. Sensitive keys (passwords, tokens) are sanitized in the recorded events.
2. Request and response bodies are not stored by default.
3. The sanitize key checks use exact match against a cached list (Set) for performance.
4. No raw passwords or tokens appear in the stored logs.

### 7.Preference UI - Consent, Status, Upload, Clear, Enable/Disable

#### 7.1.Preference UI allows granting and revoking consent

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. The Preference UI is accessible.

**Step(s):**
1. Open the Preference UI for the diagnostic logger.
2. Grant consent and verify the logger starts collecting data.
3. Revoke consent and verify the logger stops collecting data.
4. Verify the consent state is persisted across sessions.

**Test Result(s):**
1. The Preference UI allows the user to grant and revoke consent.
2. Granting consent enables data collection.
3. Revoking consent stops data collection.
4. The consent state persists across browser sessions.

#### 7.2.Preference UI shows logger status and allows enabling/disabling

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. The Preference UI is accessible and consent has been granted.

**Step(s):**
1. Open the Preference UI.
2. Verify the logger status is displayed (e.g., enabled, collecting, storage size).
3. Disable the logger and verify data collection stops.
4. Enable the logger and verify data collection resumes.

**Test Result(s):**
1. The Preference UI displays the current logger status (enabled/disabled, storage usage).
2. Disabling the logger stops data collection.
3. Enabling the logger resumes data collection.
4. The enabled/disabled state is persisted.

#### 7.3.Preference UI allows manual log upload

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger has collected some data.
3. The existing logs endpoint is reachable.

**Step(s):**
1. Open the Preference UI.
2. Click the "Upload" button to manually upload the collected logs.
3. Verify the upload request is sent to the existing logs endpoint.
4. Verify the logs are uploaded successfully.

**Test Result(s):**
1. The Preference UI allows manual log upload via the "Upload" button.
2. The upload reuses the existing logs endpoint (no new gzip endpoint).
3. The logs are uploaded successfully.
4. A confirmation or status message is shown after upload.

#### 7.4.Preference UI allows clearing stored logs

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger has collected some data in IndexedDB.

**Step(s):**
1. Open the Preference UI.
2. Click the "Clear" button to clear all stored logs.
3. Verify the IndexedDB store is emptied.

**Test Result(s):**
1. The Preference UI allows clearing all stored logs via the "Clear" button.
2. The IndexedDB store is emptied after clearing.
3. New events continue to be collected after clearing if the logger is still enabled.

### 8.Performance Hardening - Eviction Strategy

#### 8.1.Eviction does not run on every flush

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.

**Step(s):**
1. Load TMS Operation and perform multiple user actions that trigger log flushes.
2. Monitor the eviction behavior during each flush.
3. Verify the full-store eviction does not run on every flush.

**Test Result(s):**
1. The full-store eviction does not run on every flush.
2. Eviction only runs on init, at a 5-minute interval, pre-upload, or on throttled overflow.
3. Performance is not degraded by frequent evictions during normal usage.

#### 8.2.Eviction runs on init, 5-minute interval, pre-upload, and throttled overflow

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.
3. The store has reached a size that triggers eviction.

**Step(s):**
1. Load TMS Operation (init) and verify eviction runs on startup.
2. Wait 5 minutes and verify eviction runs at the interval.
3. Trigger a manual log upload and verify eviction runs pre-upload.
4. Fill the store to overflow and verify throttled eviction runs.

**Test Result(s):**
1. Eviction runs on logger initialization.
2. Eviction runs at the 5-minute interval.
3. Eviction runs before a manual upload.
4. Throttled eviction runs when the store overflows, preventing unbounded growth.

#### 8.3.Retention and size limits are enforced

**Prerequisite(s):**
1. The build containing the HERO-68555 diagnostic logger is deployed to the TMS environment.
2. Consent has been granted and the logger is enabled.

**Step(s):**
1. Generate a large volume of log events exceeding the retention/size limits.
2. Verify the oldest events are evicted when limits are exceeded.
3. Verify the store size stays within the configured limits.

**Test Result(s):**
1. Retention and size limits are enforced by the eviction strategy.
2. The oldest events are evicted first when limits are exceeded.
3. The store size remains within the configured bounds.
