# HERO-64268 [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V109 Test Report

## Report Information

| Field | Value |
| --- | --- |
| Issue Key | HERO-64268 |
| Summary | [Tech] Fix error.log with "Undefined array keys" and "Undefined variables" for Report Module - V109 |
| Type | Bug Fix |
| Component | Report Module / Messaging Plugin |
| Version | V109 |
| Status | READY FOR QA |

## Bug Reproduction Summary

| Location | Error |
| --- | --- |
| `../Plugin/Messaging/Controller/Component/MessagingEmailComponent.php`, line 236-238 | `Undefined array key "client_id"`, `"client_secret"`, `"tenant_id"` |
| `../Plugin/Messaging/Controller/Component/MessagingEmailComponent.php`, line 266 | `Trying to access array offset on value of type null` + `[TypeError] EsmtpTransport::setPassword()` |
| `../Plugin/Report/Controller/ReportsController.php`, line 811-812 | `Trying to access array offset on null` |
| Request URL `/accounts/{account}/admin/eng/report/auto_report_settings/listing/...` | `[BadRequestException] The request has been black-holed` |

**Root Cause:** OAuth2 email sending read `client_id`, `client_secret`, and `tenant_id` without guarding missing keys. A failed token response was also passed directly to `setPassword()`. Report opening read date/time/config-zone values from a `null` config array. Auto report settings listing was blocked by SecurityComponent with no blackhole callback.

## Functional Testing

### F1. Error Log Verification - Before Fix

#### F1.1. Reproduce OAuth2 email undefined keys / TypeError / blackhole
**Prerequisite(s):**
  1. Build WITHOUT the HERO-64268 fix deployed
  2. Email configured for OAuth2; or at least an email send is attempted
  3. Auto report settings listing page accessible
**Step(s):**
  1. Trigger an OAuth2 email send (use a mail template or auto report email)
  2. Navigate to auto report settings listing page (`/report/auto_report_settings/listing`)
  3. Open at least one report that reads date/time format from config
  4. Check error.log after each action
**Reproduction Result(s):**
  1. error.log records `Undefined array key "client_id"`, `"client_secret"`, `"tenant_id"` in MessagingEmailComponent.php
  2. error.log records `Trying to access array offset on value of type null` and a `TypeError` from `EsmtpTransport::setPassword()`
  3. error.log records `Trying to access array offset on null` in ReportsController.php line 811-812
  4. error.log records `[BadRequestException] The request has been black-holed` for the auto report settings listing URL

### F2. Error Log Verification - After Fix

#### F2.1. Verify no OAuth2 / blackhole warnings in error.log
**Prerequisite(s):**
  1. Build WITH the HERO-64268 fix deployed
  2. Same environment as F1; clear error.log before test
**Step(s):**
  1. Repeat exactly the same flows from F1
  2. Inspect error.log for new entries
**Fix Result(s):**
  1. error.log does NOT contain `Undefined array key "client_id"`, `"client_secret"`, `"tenant_id"`
  2. error.log does NOT contain `Trying to access array offset` from MessagingEmailComponent.php
  3. error.log does NOT contain any `TypeError` from `EsmtpTransport::setPassword()`
  4. error.log does NOT contain `Trying to access array offset on null` from ReportsController.php
  5. error.log does NOT contain any `BadRequestException The request has been black-holed`
  6. Auto report settings listing page loads normally

### F3. Regression Testing

#### F3.1. OAuth2 email sending still works
**Step(s):**
  1. Send a test email using OAuth2 configuration with valid credentials
  2. Verify email is received successfully
**Fix Result(s):**
  1. OAuth2 emails are sent and delivered correctly

#### F3.2. Non-OAuth2 email sending still works
**Step(s):**
  1. Send a test email using SMTP (non-OAuth2) credentials
**Fix Result(s):**
  1. SMTP emails are sent and delivered correctly

#### F3.3. Auto report settings listing page works
**Step(s):**
  1. Open auto report settings listing, pagination, and sort by `schd_next_run_time`
**Fix Result(s):**
  1. Page loads, pagination works, sort works
  2. No SecurityComponent blackhole / BadRequestException

#### F3.4. Report opening with/without config zones
**Step(s):**
  1. Open reports on outlets WITH config zones and outlets WITHOUT config zones
**Fix Result(s):**
  1. Both cases open normally
  2. Date format / time format fall back gracefully when config array is null

## Compatibility Testing

### Multi-Account Environment
**Step(s):**
  1. Test on multiple customer accounts (e.g. shangrilagroup, ihg)
**Test Result(s):**
  1. All accounts produce clean error.log for OAuth2, report opening, and auto report settings

## Test Environment Information

- **Version**: Build containing HERO-64268 fix (Report V109)
- **Environment**: Local Test Environment, HQ Test Environment

## Appendix

### Affected Files
- `../Plugin/Messaging/Controller/Component/MessagingEmailComponent.php`
- `../Plugin/Report/Controller/ReportsController.php`
- SecurityComponent blackhole handling for auto report settings listing

### Release Notes Summary
- Fixed: OAuth2 email sends guard `client_id` / `client_secret` / `tenant_id` before reading
- Fixed: Failed OAuth2 token responses no longer passed to `setPassword()`
- Fixed: Report opening guards null config array for date/time format lookups
- Fixed: Auto report settings listing is no longer blocked by SecurityComponent; a proper blackhole callback is in place
