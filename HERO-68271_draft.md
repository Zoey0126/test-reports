# HERO-68271 Vulnerability Issues - JWT Token Contains Sensitive Internal Infrastructure Information and Is Transmitted via URL - Test Report

## Report Information

| Field | Value |
| --- | --- |
| Issue Key | HERO-68271 |
| Issue Type | Bug |
| Component | REPORTS |
| Template | report |
| QC Tester | Zephyr Ji |
| QC Reviewer | Kathy Kuang |
| Test Date | 2026-09-21 |
| Version | v1.0.0 |

## Bug Summary

When a user opens a report in Infrasys Cloud, the report session token and callback address were passed in the browser URL. This exposed internal server hostnames in the browser address bar or Network panel, creating a security vulnerability.

After this fix, when both Infrasys Cloud Report package and BIRT report viewer package are upgraded together, the first open of a report no longer puts the session token or callback address in the URL. The report still opens and works as before (preview, favorite, preset, client-side open, print view, export, and page flip).

---

## Environment Preparation

### Environment Matrix

| Environment | PHP Report Plugin | BIRT Version | Test Type |
| --- | --- | --- | --- |
| A (Main) | New | 1.1.11 Rev#290 or higher | Full validation |
| B | New | Old (below 1.1.11 Rev#290) | Quick compatibility check |
| C | Old | 1.1.11 Rev#290 or higher | Quick compatibility check |

### Pass Criteria (Environment A)

- Browser Network shows no internal hostname and no plaintext JWT on first open
- First request is POST with enc1. token
- Cookies work for follow-up requests
- Favorite / Preset / Client / Print-view / Export checks pass

---

## Test Cases

### Test Case 1: Standard HQ Report Preview - First Request Security (Environment A)

**Prerequisite(s):**
1. Environment A: Both PHP Report Plugin and BIRT package upgraded (BIRT version.txt shows 1.1.11 Rev#290 or higher)
2. Browser Developer Tools → Network panel is open
3. Logged into Infrasys Cloud

**Step(s):**
1. Navigate to Reports module
2. Select any standard HQ report from the listing
3. Click "View" to open the report
4. Check the first request to the BIRT report viewer in Network panel

**Reproduction Result(s) - Before Fix:**
1. The first request URL contains `jwt` and/or `callbackUrl` query parameters
2. The URL or token content shows an internal server hostname
3. The report can still open, but sensitive information is visible in the browser

**Fix Result(s) - After Fix:**
1. ✅ The first request is a POST method
2. ✅ Request body contains `rptSession` starting with `enc1.` (encrypted token)
3. ✅ Request body contains `rptOrigin`
4. ✅ URL does NOT contain plaintext `jwt=` or `callbackUrl=` query parameters
5. ✅ No internal server hostname is visible on the report open URL
6. ✅ Report renders normally after the POST request

---

### Test Case 2: Favorite Reports - Security Validation (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Network panel is open
3. At least one favorite report exists in the system

**Step(s):**
1. Navigate to the Favorite Reports section
2. Click to open a favorite report
3. Check the first BIRT request in Network panel

**Reproduction Result(s) - Before Fix:**
1. URL contains `jwt` and/or `callbackUrl` parameters
2. Internal hostname exposed in URL

**Fix Result(s) - After Fix:**
1. ✅ First request is POST with `rptSession` and `rptOrigin` in body
2. ✅ No `jwt=` or `callbackUrl=` in URL
3. ✅ No internal hostname in URL
4. ✅ Favorite report renders normally

---

### Test Case 3: Preset Parameter Reports (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Network panel is open
3. A report with preset parameters available

**Step(s):**
1. Open a report with existing preset parameters
2. Select a preset parameter from BIRT toolbar
3. Run the report via the preset
4. Check Network panel

**Reproduction Result(s) - Before Fix:**
1. URL exposes JWT token and callback address
2. Internal hostname visible

**Fix Result(s) - After Fix:**
1. ✅ Preset parameter runs without `wrong_param_format` error
2. ✅ First request is POST with encrypted session token
3. ✅ No plaintext JWT or callback address in URL
4. ✅ Report renders correctly with preset parameters

---

### Test Case 4: Client-Side Reports - Security Validation (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Network panel is open
3. POS client connected to HQ

**Step(s):**
1. Open a report from POS/client side
2. Monitor the Network panel for the BIRT request

**Reproduction Result(s) - Before Fix:**
1. Client-side report open exposes internal hostname via URL parameters

**Fix Result(s) - After Fix:**
1. ✅ First BIRT request is POST with encrypted session
2. ✅ No internal hostname or plaintext JWT in URL
3. ✅ Client-side report renders successfully

---

### Test Case 5: View Reports After Printing (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Network panel is open

**Step(s):**
1. Open a report
2. Click Print or add to Print Queue
3. Navigate to Print Queue and view the printed result

**Reproduction Result(s) - Before Fix:**
1. Viewing printed report result exposes JWT in URL

**Fix Result(s) - After Fix:**
1. ✅ Print function works normally
2. ✅ Viewing print queue result uses POST with encrypted token
3. ✅ No sensitive parameters in URL
4. ✅ Report/print path functions correctly

---

### Test Case 6: Report Export - URL Sanitation (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Network panel is open

**Step(s):**
1. Open a report
2. Export to Excel format
3. Export to PDF format
4. Export to CSV format (legacy)
5. Check the export request URLs in Network panel

**Reproduction Result(s) - Before Fix:**
1. Export URLs contain duplicated frameset paths with JWT token
2. Legacy CSV export may show "The report file : does not exist or contains errors"

**Fix Result(s) - After Fix:**
1. ✅ Excel export succeeds
2. ✅ PDF export succeeds
3. ✅ Legacy CSV export succeeds without "report file does not exist" error
4. ✅ Network URL does NOT contain duplicated `frameset?__report=&https://.../frameset?__report=` pattern
5. ✅ No plaintext JWT token visible in export URLs

---

### Test Case 7: Pagination / Second Request - Cookie-Based Session (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Browser Developer Tools → Application → Cookies panel is open
3. A report already opened successfully via POST

**Step(s):**
1. After the report opens, check cookies under `/birt` path
2. Flip to the next page of the report
3. Trigger a re-run of the report
4. Verify subsequent requests still work

**Reproduction Result(s) - Before Fix:**
1. Page flips still exposed JWT in URL query parameters

**Fix Result(s) - After Fix:**
1. ✅ After first POST open, cookies `rptSes` and `rptOrg` exist under `/birt` path
2. ✅ Page flip uses cookie-based session, no JWT in URL
3. ✅ Re-run uses cookie-based session, no JWT in URL
4. ✅ All subsequent operations work without errors

---

### Test Case 8: Error Path - No Internal Information Leakage (Environment A)

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. Force an error condition (e.g., invalid report path, expired session)

**Step(s):**
1. Trigger an error scenario while loading a report
2. Observe the error message displayed to the user
3. Verify details are only in server logs

**Reproduction Result(s) - Before Fix:**
1. Error message could expose stack trace or internal hostname to user

**Fix Result(s) - After Fix:**
1. ✅ Screen shows a short generic message with a Ref code (e.g., "Ref: xxxxxxxx")
2. ✅ No internal hostname displayed in UI
3. ✅ No technical stack or path information shown to user
4. ✅ Detailed error stays in server logs (`birt_security` / `birt_api`)

---

### Test Case 9: Compatibility - Mixed Package Environment B (New PHP + Old BIRT)

**Prerequisite(s):**
1. Environment B: PHP upgraded, but BIRT version.txt is below 1.1.11 Rev#290
2. Browser Developer Tools → Network panel is open

**Step(s):**
1. Open any one standard report
2. Confirm the report still opens successfully

**Reproduction Result(s) - Before Fix:**
1. N/A (compatibility check, not a bug scenario)

**Fix Result(s) - After Fix:**
1. ✅ Report still opens successfully
2. ✅ Mixed package does not break report open functionality
3. Note: Full security improvement (no JWT in URL) requires both packages to be upgraded

---

### Test Case 10: Compatibility - Mixed Package Environment C (Old PHP + New BIRT)

**Prerequisite(s):**
1. Environment C: PHP not yet upgraded, BIRT already new (version.txt ≥ 1.1.11 Rev#290)
2. Browser Developer Tools → Network panel is open

**Step(s):**
1. Open any one standard report
2. Confirm the report still opens successfully

**Reproduction Result(s) - Before Fix:**
1. N/A (compatibility check, not a bug scenario)

**Fix Result(s) - After Fix:**
1. ✅ Report still opens successfully
2. ✅ Mixed package does not break report open functionality
3. Note: Full security improvement (no JWT in URL) requires both packages to be upgraded

---

### Test Case 11: Auto Reports - Scheduled Run Verification

**Prerequisite(s):**
1. Environment A: Both packages upgraded
2. An auto report preset configured (scheduled or manual)

**Step(s):**
1. Trigger an auto report run (scheduled or manual)
2. Verify the auto run completes successfully

**Reproduction Result(s) - Before Fix:**
1. N/A (auto report does not involve browser URL exposure)

**Fix Result(s) - After Fix:**
1. ✅ Scheduled auto report run succeeds
2. ✅ Manual auto report run succeeds
3. ✅ Report output is delivered correctly (Email/FTP/SFTP as configured)

---

### Test Case 12: Regression - Report Functional Scope

**Prerequisite(s):**
1. Environment A: Both packages upgraded

**Step(s):**
1. Verify the following report-related functions still work correctly:
   1. Drill-down reports open and navigate correctly
   2. English (EN) and Chinese (ZH) language reports open correctly
   3. Report preview works from all entry points
   4. Multi-page reports render and paginate correctly

**Reproduction Result(s) - Before Fix:**
1. N/A (regression check)

**Fix Result(s) - After Fix:**
1. ✅ Drill-down reports function correctly
2. ✅ EN language reports open correctly
3. ✅ ZH language reports open correctly
4. ✅ All report entry points work without JWT exposure
5. ✅ Multi-page pagination uses cookie-based session

---

## Test Environment

| Item | Details |
| --- | --- |
| **Environment** | HQ Test Environment |
| **PHP Report Plugin** | Latest version (with security fix) |
| **BIRT Viewer** | 1.1.11 Rev#290 (new) |
| **Browser** | Chrome (with DevTools Network panel) |
| **Test Date** | 2026-09-21 |

## Regression Scope Analysis

This Bug fix changes how report session tokens are passed (URL → POST body + cookies). The following related modules require regression verification:

| Related Module | Regression Items |
| --- | --- |
| **Auto Reports** | Scheduled/manual runs still succeed |
| **Favorite Reports** | Open flow uses POST, no JWT in URL |
| **Preset Parameter Reports** | Creation, edit, run all work with POST-based session |
| **Client-side Reports** | POS-to-HQ report viewing uses POST security |
| **Export Functions** | Excel/PDF/CSV export URLs sanitized |
| **Print Queue** | Viewing printed reports uses cookie-based session |
| **Multi-language** | EN/ZH reports open with new security model |
| **Mixed Package Compatibility** | Environments B and C still open reports |

## Conclusion

- **Bug Severity**: P0 (Security Vulnerability - internal infrastructure info exposure)
- **Fix Scope**: Session token transmission mechanism (URL → POST + Cookies)
- **Test Coverage**: 12 test cases covering Environment A (main validation), Environment B/C (compatibility), error paths, and regression scope
- **Expected Pass Rate**: All test cases should pass when both packages are upgraded (Environment A)
- **Compatibility**: Mixed package environments (B/C) remain functional but do not achieve full security improvement until both are upgraded
