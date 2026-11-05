# HERO-70187 Test Report

The member profile listing required Profile Management login, but the export member endpoint did not. Unauthenticated requests could download a ZIP export of member PII through a direct URL. The finding was raised in the penetration testing report dated 2026-11-02 for Infrasys POS HQ, where a cookie-less client obtained a ZIP archive containing member.csv with the full member records, and the headerList path segment allowed the requester to choose the exported columns.

## Functional Testing

### 1. Unauthenticated Export URL Returns a ZIP Archive of Member Data

**Prerequisite(s):**
1. Profile Management is installed and the member profile listing is available.
2. A user account with read permission for Import and Export is available.
3. Member profiles exist in the member profile listing.
4. An incognito browser window without any session is available.

**Step(s):**
1. Log in as the user with read permission for Import and Export and go to Profile Management > Profile Listing.
2. Click the "Export Profile" button and capture the export member request URL from the network tab.
3. Open an incognito browser window and enter the captured export member URL directly, without any cookie of any kind.
4. Observe the response status, the response headers and the returned content.
5. Extract the downloaded archive and open member.csv.

**Reproduction Result(s):**
1. The request is answered with HTTP 200 OK and Content-Type text/html; charset=UTF-8.
2. The response carries Content-Disposition attachment;filename=member_<timestamp>.zip and a ZIP archive is downloaded.
3. The ZIP archive contains member.csv with a header row and one data row per member, including Account No., Salutation, Last Name, First Name, Alt. Name, Contact (Mobile), Contact (Office), Contact (Home), Email (Personal), Organization, Expiry Date and Profile Status.
4. No login page and no permission error is returned to the unauthenticated client.

**Fix Result(s):**
1. The export member request without a valid authenticated session is redirected to the login page.
2. No ZIP archive and no member data are returned.
3. The response is the same redirect that the member profile listing returns to an unauthenticated client.

### 2. Unauthenticated Access to Member Data

#### 2.1. Export Member URL Requested Without a Session Is Redirected to the Login Page

**Prerequisite(s):**
1. Profile Management is installed and the member profile listing is available.
2. The export member request URL is captured from the network tab after clicking the "Export Profile" button in Profile Listing.
3. An incognito browser window without any session is available.

**Step(s):**
1. Open the incognito browser window and confirm that no session exists.
2. Enter the export member request URL directly.
3. Observe the response and the page displayed.

**Reproduction Result(s):**
1. Before the fix, the request returns HTTP 200 OK with a ZIP attachment of member records.
2. The unauthenticated client receives the exported personal data of all members.

**Fix Result(s):**
1. The request is redirected to the login page.
2. The login page is displayed and no attachment is downloaded.
3. No member record is disclosed to the unauthenticated client.

#### 2.2. Export Member Request with Attacker-Selected Header List Is Rejected Without a Session

**Prerequisite(s):**
1. Profile Management is installed and the member profile listing is available.
2. An incognito browser window without any session is available.
3. The export member request URL including the headerList path segment is prepared.

**Step(s):**
1. Open the incognito browser window without any session.
2. Modify the headerList path segment of the export member URL to a self-chosen set of member fields.
3. Send the modified request directly.
4. Observe the response and any returned content.

**Reproduction Result(s):**
1. Before the fix, the request returns HTTP 200 OK with a ZIP attachment and the header row of member.csv follows the self-chosen headerList fields.
2. The requester controls which member fields are extracted without any authenticated session.

**Fix Result(s):**
1. The request is redirected to the login page before any data is processed.
2. No archive is produced and the self-chosen field list has no effect.
3. No member record or member field is disclosed.

#### 2.3. Member Profile Listing Remains Redirected to the Login Page Without a Session

**Prerequisite(s):**
1. Profile Management is installed and the member profile listing is available.
2. An incognito browser window without any session is available.

**Step(s):**
1. Open the incognito browser window without any session.
2. Enter the URL of the member profile listing under Profile Management.
3. Observe the response and the page displayed.

**Reproduction Result(s):**
1. Before the fix, the member profile listing is already protected and redirects the cookie-less client to the gate or login page.
2. Only the export member endpoint is reachable without authentication.

**Fix Result(s):**
1. The member profile listing continues to redirect the unauthenticated client to the login page.
2. The protection of the listing page is unchanged by the fix.

### 3. Export Permission for Logged-In Users

#### 3.1. User Without Import and Export Read Permission Is Redirected to the List Page

**Prerequisite(s):**
1. A user account without read permission for Import and Export exists.
2. The export member request URL is captured from the network tab after clicking the "Export Profile" button in Profile Listing.
3. The user without Import and Export read permission is able to log in to Profile Management.

**Step(s):**
1. Log in as the user without read permission for Import and Export.
2. Enter the export member request URL directly.
3. Observe the response and the page displayed.
4. Repeat the request after logging out and logging in again.

**Reproduction Result(s):**
1. Before the fix, the authenticated user without Import and Export read permission is able to download the ZIP export of member records through the direct URL.
2. The export is not restricted by the Import and Export permission of the user.

**Fix Result(s):**
1. After login as the user without Import and Export read permission, the direct export member URL redirects to the list page.
2. No ZIP archive and no member record is returned.
3. The same result is observed on each repeated attempt.

#### 3.2. User With Import and Export Read Permission Exports Member Profiles Successfully

**Prerequisite(s):**
1. A user account with read permission for Import and Export exists.
2. Member profiles exist in the member profile listing.
3. The user is able to log in to Profile Management.

**Step(s):**
1. Log in as the user with read permission for Import and Export.
2. Go to Profile Management > Profile Listing.
3. Select the members to be exported and click the "Export Profile" button.
4. Observe the response and open the downloaded archive.

**Reproduction Result(s):**
1. Before the fix, the authorized export succeeds and returns the ZIP archive of member records.
2. The export is performed without any additional permission prompt.

**Fix Result(s):**
1. The authorized user still exports the selected member profiles successfully.
2. The response is a ZIP archive with Content-Disposition attachment.
3. No login redirect and no permission error is returned to the authorized user.

### 4. Member Export Function Regression

#### 4.1. Exported Archive Contains member.csv with the Selected Header Fields

**Prerequisite(s):**
1. A user with read permission for Import and Export is logged in.
2. Member profiles exist in the member profile listing.
3. The member fields to be exported are selected in the export header list.

**Step(s):**
1. Go to Profile Management > Profile Listing.
2. Select the member fields for the export header list and click the "Export Profile" button.
3. Open the downloaded ZIP archive and open member.csv.
4. Compare the header row and the data rows with the selected fields.

**Reproduction Result(s):**
1. Before the fix, the archive still contains member.csv with the header row followed by one data row per member.
2. The exported columns follow the selected header list.

**Fix Result(s):**
1. The ZIP archive still contains member.csv.
2. The header row matches the selected header fields and the data rows follow the same column order.
3. One data row is present for each exported member.

#### 4.2. Export Profile Button Still Generates the Export Request in Profile Listing

**Prerequisite(s):**
1. A user with read permission for Import and Export is logged in.
2. Member profiles exist in the member profile listing.
3. The browser network tab is open during the test.

**Step(s):**
1. Go to Profile Management > Profile Listing.
2. Click the "Export Profile" button.
3. Observe the request recorded in the network tab and the downloaded file.
4. Compare the export request URL with the URL used for the unauthenticated test cases.

**Reproduction Result(s):**
1. Before the fix, the "Export Profile" button sends the export member request and the archive is downloaded.
2. The export request URL is the address that can also be entered directly.

**Fix Result(s):**
1. The "Export Profile" button still generates the export member request for the authorized user.
2. The archive is still downloaded by the button.
3. The export request URL is unchanged, and it is now protected by the authenticated session and the Import and Export permission.

## Test Environment

Testing is performed on Infrasys POS HQ with Profile Management installed and the member profile listing available. The environment includes a user account with read permission for Import and Export, a user account without read permission for Import and Export, member profiles seeded for the export, and an incognito browser window used as a cookie-less client. Export member URLs are captured from the browser network tab after clicking the "Export Profile" button in Profile Listing.
