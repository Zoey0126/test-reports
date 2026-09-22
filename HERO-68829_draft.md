# HERO-68829 TMS 2.0 - Payment method export/edit leaks password, client_secret and keys Test Report

## Functional Testing

### F1. Payment Method Export - Credential Stripping

#### F1.1. Export JSON must not contain credentials
**Prerequisite(s):**
  1. Payment method configured with Shiji password, Payby client_secret, SilverStone key, or Astral key
  2. Admin user with payment-method read ACL
**Step(s):**
  1. Log in to TMS 2.0 admin as a user with Payment Method read access
  2. Navigate to Payment Methods admin page
  3. Click "Export" to download the payment methods JSON
  4. Open the exported JSON file in a text editor
  5. Search for `password`, `client_secret`, and any key-related fields
**Reproduction Result(s):**
  1. Exported JSON contained full `paym_settings` JSON including Shiji password, Payby `client_secret`, and SilverStone/Astral keys in plaintext
**Fix Result(s):**
  1. Exported JSON does NOT contain Shiji password
  2. Exported JSON does NOT contain Payby `client_secret`
  3. Exported JSON does NOT contain SilverStone key
  4. Exported JSON does NOT contain Astral key
  5. All other non-sensitive payment method fields are still exported correctly

#### F1.2. Export respects payment-method read ACL
**Prerequisite(s):**
  1. Admin user WITHOUT payment-method read ACL
**Step(s):**
  1. Log in as a user without Payment Method read ACL
  2. Attempt to access Payment Methods admin export endpoint (PaymentMethodsController::admin_export)
**Reproduction Result(s):**
  1. Export was performed without checking the payment-method read access ACL
**Fix Result(s):**
  1. Export is blocked for users without payment-method read ACL
  2. Access denied / 403 is returned

### F2. Payment Method Edit - No Secrets Sent to Browser

#### F2.1. Edit page does not pre-fill stored secrets
**Prerequisite(s):**
  1. An existing payment method has stored password / client_secret / keys
  2. Admin user with write access to that payment method
**Step(s):**
  1. Open the existing payment method's Edit page in the browser
  2. Inspect the HTML form fields via browser DevTools (or view-source)
  3. Check the value attribute of password, client_secret, and key input fields
**Reproduction Result(s):**
  1. Edit form put `client_secret` into HTML as a plaintext text field — the secret was visible in the DOM
  2. Stored secrets were sent from server to browser in the initial form render
**Fix Result(s):**
  1. Password input field value is empty on page load (no pre-fill from DB)
  2. Payby `client_secret` input is rendered as a password-type field and stays empty
  3. SilverStone key and Astral key inputs stay empty
  4. Secrets are NOT present anywhere in the HTML source / DOM for the edit page

#### F2.2. Saving with blank secrets preserves existing DB values
**Prerequisite(s):**
  1. Existing payment method with stored credentials in DB
**Step(s):**
  1. Open the payment method Edit page
  2. Change a non-sensitive field (e.g., method name or description)
  3. Leave all password / client_secret / key fields blank (do not enter anything)
  4. Click "Save"
  5. Re-open the Edit page or view the payment method details
  6. Verify the original credentials are still present in the system
**Reproduction Result(s):**
  1. Edit was sending secrets to browser; behaviour when blank fields were submitted was not secure by design
**Fix Result(s):**
  1. DB-stored password is preserved — blank field does not overwrite it
  2. DB-stored Payby `client_secret` is preserved
  3. DB-stored SilverStone key and Astral key are preserved
  4. Only the modified non-sensitive field is updated

#### F2.3. Saving with newly entered secrets updates DB values
**Prerequisite(s):**
  1. Existing payment method
**Step(s):**
  1. Open the payment method Edit page
  2. Enter a new value in the password field (and/or `client_secret`, key fields)
  3. Click "Save"
  4. Re-open the Edit page and verify by checking that the masked view changes (or via DB introspection)
**Fix Result(s):**
  1. Entered secrets correctly overwrite the previous values in DB
  2. Non-entered secret fields keep their previous values

### F3. Payment Method View - Secrets Masked

#### F3.1. View page masks Payby client_secret
**Prerequisite(s):**
  1. Payment method with Payby integration has a real `client_secret` stored
**Step(s):**
  1. Open the payment method View page (not Edit)
  2. Locate the Payby `client_secret` display
**Reproduction Result(s):**
  1. Payby `client_secret` was shown in plaintext on the View page
**Fix Result(s):**
  1. Payby `client_secret` is masked (e.g., `************` or similar), never shown in plaintext

#### F3.2. View page masks SilverStone key
**Prerequisite(s):**
  1. Payment method with SilverStone integration has a real key stored
**Step(s):**
  1. Open the payment method View page
  2. Locate the SilverStone key display
**Reproduction Result(s):**
  1. SilverStone key was shown in plaintext on the View page
**Fix Result(s):**
  1. SilverStone key is masked, never shown in plaintext

#### F3.3. View page masks Shiji password and Astral key (if applicable)
**Prerequisite(s):**
  1. Payment methods configured with Shiji password or Astral key
**Step(s):**
  1. Open each payment method's View page
**Fix Result(s):**
  1. Shiji password is masked
  2. Astral key is masked

### F4. Payment Method Import - Secrets Handling

#### F4.1. Import onto existing method preserves stored secrets when fields are blank
**Prerequisite(s):**
  1. Existing payment method has credentials in DB
  2. Exported JSON (post-fix version) does not contain the credentials
**Step(s):**
  1. Use the post-fix export JSON as an import source
  2. Import it to update the SAME existing payment method
  3. Inspect the resulting payment method via View or DB
**Reproduction Result(s):**
  1. Pre-fix export contained secrets; post-fix export does not — import behaviour must handle the missing keys safely
**Fix Result(s):**
  1. The existing DB credentials are preserved because the imported JSON has blank / missing credential fields
  2. No credential is wiped by an import that does not provide those values

#### F4.2. Import for a new method still requires secrets to be entered
**Prerequisite(s):**
  1. A new payment method (not yet saved) is being imported / created
**Step(s):**
  1. Import a JSON that does not include credentials for a brand new payment method
  2. Attempt to save
**Fix Result(s):**
  1. Import rejects or warns that required credential fields (password / client_secret / key) are missing
  2. A new payment method cannot be created without its required secrets

## Compatibility Testing

### TMS Browser Compatibility

#### 1. Payment Method List and Edit in Chrome
**Step(s):**
  1. Open Payment Methods admin page in Chrome
  2. Click Edit on a method with stored credentials
  3. Verify credential inputs are empty password-type fields
  4. View the method and verify masking works
**Test Result(s):**
  1. All credential handling works as expected in Chrome

#### 2. Payment Method List and Edit in Safari
**Step(s):**
  1. Repeat the same flow in Safari
**Test Result(s):**
  1. All credential handling works as expected in Safari

#### 3. Payment Method List and Edit in Edge / Firefox
**Step(s):**
  1. Repeat the same flow in Edge and Firefox
**Test Result(s):**
  1. All credential handling works as expected

### POS Device Compatibility

#### 1. POS devices using payment methods still process payments correctly
**Prerequisite(s):**
  1. A POS device is configured to use one of the payment methods that had its export/edit security hardened
**Step(s):**
  1. Take a payment on the POS using that method
  2. Confirm the payment is processed successfully via the payment gateway
**Test Result(s):**
  1. Payments succeed — the server-side credentials are still intact and used correctly for gateway communication
  2. No regression in day-to-day payment operations

## Test Environment Information

- **Version**: TMS 2.0 build containing the fix
- **Environment**: Local Test Environment, HKJC Test Environment
- **Browsers**: Chrome, Safari, Edge, Firefox (latest stable)

## Appendix

### Affected Code
- `PaymentMethodsController::admin_export` — access check + credential stripping
- Payment method Edit page — secrets not sent to browser; `client_secret` rendered as password field
- Payment method View page — `client_secret` and SilverStone key masking

### Release Notes Summary
- Fixed: Export leaked Shiji password, Payby `client_secret`, SilverStone/Astral keys
- Fixed: Edit form exposed stored `client_secret` in plaintext HTML
- Fixed: View page exposed `client_secret` and SilverStone key in plaintext
- Fixed: Export now checks payment-method read ACL
- Improved: Import preserves existing DB secrets when incoming fields are blank
