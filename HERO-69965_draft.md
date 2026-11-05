# HERO-69965 Test Report - License Admin Security Enhancement on Cloud HQ

## Background

Security testing of cloud HQ (hq.hero-cloud.com) found two high-risk issues in license management. Anyone can open the offline certificate pages and the certificate update page without logging in, while the main license page correctly redirects the user to the login page. The certificate update page also accepts a proxy address from the request and uses it for the server's outbound connection, and the returned error text reveals whether that address can be reached, which can be used to probe the internal network.

The affected pages are `.../licenses/verify_offline_cert`, `.../licenses/update_offline_cert` and `.../licenses/update_cert`. This enhancement applies the security changes only on the cloud HQ domains; license update on on-premises sites, including a customer proxy, stays as it is.

## Functional Testing

### 1.Manual Update HERO Cert. Function Availability

#### 1.1.Verify Manual Update HERO Cert. Function Is Not Available on Cloud HQ
**Prerequisite(s):**
1. A cloud HQ environment (hq.hero-cloud.com) is deployed with the new version
2. A valid admin account exists on the cloud HQ environment

**Step(s):**
1. Log in to the cloud HQ admin portal with a valid admin account
2. Navigate to the license management area
3. Look for the function "Manual Update HERO Cert." in the license pages and function menus
4. Try to access the offline certificate pages used by the "Manual Update HERO Cert." function directly

**Test Result(s):**
1. The "Manual Update HERO Cert." function is not available on the cloud HQ environment
2. The function entry is not displayed or cannot be launched from the license management pages
3. The offline certificate pages used by this function are not accessible without login

#### 1.2.Verify Manual Update HERO Cert. Function Remains Available on Non-Cloud HQ Environment
**Prerequisite(s):**
1. An on-premises (non-cloud HQ) environment is deployed with the new version
2. A valid admin account exists on the on-premises environment

**Step(s):**
1. Log in to the on-premises admin portal with a valid admin account
2. Navigate to the license management area
3. Locate the function "Manual Update HERO Cert."
4. Launch the function and open the offline certificate pages

**Test Result(s):**
1. The "Manual Update HERO Cert." function is still available on the non-cloud HQ environment
2. The offline certificate pages can be opened as before
3. Existing license management behavior on-premises is unchanged

### 2.Login Requirement for Offline Certificate Endpoints

#### 2.1.Verify Offline Certificate Endpoint Requires Login on Cloud HQ
**Prerequisite(s):**
1. A cloud HQ environment (hq.hero-cloud.com) is deployed with the new version
2. The browser session is logged out (no active admin login)

**Step(s):**
1. Without logging in, open the endpoint `.../licenses/verify_offline_cert` on the cloud HQ environment
2. Observe the system response
3. Check whether any certificate verification processing is executed

**Test Result(s):**
1. The system detects the call is in the cloud HQ environment and requires login
2. The user is redirected to the admin login page, the same as the main license page
3. No certificate verification processing runs before login

#### 2.2.Verify Update Offline Certificate Endpoint Requires Login on Cloud HQ
**Prerequisite(s):**
1. A cloud HQ environment (hq.hero-cloud.com) is deployed with the new version
2. The browser session is logged out (no active admin login)

**Step(s):**
1. Without logging in, open the endpoint `.../licenses/update_offline_cert` on the cloud HQ environment
2. Observe the system response
3. Check whether any certificate update processing is executed and whether the stored license certificate changes

**Test Result(s):**
1. The system detects the call is in the cloud HQ environment and requires login
2. The user is redirected to the admin login page, the same as the main license page
3. No certificate update processing runs before login and the stored license certificate is not changed

#### 2.3.Verify Logged-In Admin Can Access Offline Certificate Endpoints on Cloud HQ
**Prerequisite(s):**
1. A cloud HQ environment (hq.hero-cloud.com) is deployed with the new version
2. A valid admin account exists on the cloud HQ environment

**Step(s):**
1. Log in to the cloud HQ admin portal with a valid admin account
2. Open the endpoint `.../licenses/verify_offline_cert` in the same session
3. Open the endpoint `.../licenses/update_offline_cert` in the same session

**Test Result(s):**
1. No redirect to the admin login page occurs for a logged-in admin
2. The offline certificate endpoints respond to the logged-in admin according to the existing certificate logic
3. No unexpected error is displayed after login

#### 2.4.Verify Offline Certificate Endpoints Do Not Require Login on Non-Cloud HQ Environment
**Prerequisite(s):**
1. An on-premises (non-cloud HQ) environment is deployed with the new version
2. The browser session is logged out (no active admin login)

**Step(s):**
1. Without logging in, open the endpoint `.../licenses/verify_offline_cert` on the on-premises environment
2. Without logging in, open the endpoint `.../licenses/update_offline_cert` on the on-premises environment
3. Observe the system response

**Test Result(s):**
1. The system detects the call is not in the cloud HQ environment and does not add a login requirement
2. The offline certificate endpoints behave exactly as before the change
3. No redirect to the admin login page is introduced on-premises

### 3.Proxy URL Handling on Update Certificate Endpoint

#### 3.1.Verify Update Certificate Endpoint Ignores proxyUrl POST Parameter on Cloud HQ
**Prerequisite(s):**
1. A cloud HQ environment (hq.hero-cloud.com) is deployed with the new version
2. A reachable proxy address and an internal network address are prepared as test proxyUrl values

**Step(s):**
1. Send a POST request to the endpoint `.../licenses/update_cert` on the cloud HQ environment with the `proxyUrl` parameter set to the prepared reachable proxy address
2. Observe whether the server's outbound connection uses the supplied proxy address
3. Send the request again with `proxyUrl` set to an internal network address
4. Compare the returned error text between a reachable and an unreachable address

**Test Result(s):**
1. The endpoint does not account `proxyUrl` as a POST parameter on cloud HQ
2. The server's outbound connection is not made through the proxy address supplied in the request
3. The response does not reveal whether the supplied address can be reached, so it cannot be used to probe the internal network

#### 3.2.Verify Update Certificate Endpoint Keeps Existing proxyUrl Behavior on Non-Cloud HQ Environment
**Prerequisite(s):**
1. An on-premises (non-cloud HQ) environment is deployed with the new version
2. A reachable customer proxy address is prepared

**Step(s):**
1. Send a POST request to the endpoint `.../licenses/update_cert` on the on-premises environment with the `proxyUrl` parameter set to the customer proxy address
2. Observe whether the server's outbound connection uses the supplied proxy address
3. Complete a certificate update through the customer proxy

**Test Result(s):**
1. The endpoint still accounts `proxyUrl` as a POST parameter on the non-cloud HQ environment
2. The server's outbound connection goes through the customer proxy as before
3. On-premises license update, including a customer proxy, is unchanged

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-69965 |
| Sprint | Platform - 20261019-20261030 |
| Template | infrasys |
| Component | CORE |
| Issue Type | Improvement |
| Module | License Admin (Cloud HQ) |
| Test Target | `.../licenses/verify_offline_cert`, `.../licenses/update_offline_cert`, `.../licenses/update_cert` |
| Environment | Cloud HQ environment (hq.hero-cloud.com) and on-premises (non-cloud HQ) environment |
| QC Tester | To be assigned |
| QC Reviewer | To be assigned |
