# TMS 2.0 - Authority Ask Password field not masked Test Report

## Functional Testing

### 1.Password Masking in Authority Dialog

#### 1.1.Password input is masked while typing
**Prerequisite(s):**
1. TMS 2.0 with the fix deployed is accessible
2. A function whose outlet function authority is Ask Password (e.g. confirm or void) is configured for the logged-in user

**Step(s):**
1. Log in to TMS 2.0
2. Run the function with Ask Password authority so the dialog Authority : Ask Password opens
3. Focus the Password field and type the password characters

**Reproduction Result(s):**
1. Before the fix, the password was displayed as plaintext (e.g. 51 was visible while typing) and the input stayed as type text after the first keystroke, because the React 19.2.5 upgrade (HERO-65836) rewrote the field type from props on every commit

**Fix Result(s):**
1. The password is masked as dots or asterisks during the whole input
2. The field remains masked after multiple characters are typed and no plaintext is exposed at any time

#### 1.2.Password remains masked after re-focus and re-entry
**Prerequisite(s):**
1. The Authority : Ask Password dialog is accessible

**Step(s):**
1. Open the dialog, type a partial password, then move focus to another field
2. Focus the Password field again and continue typing
3. Submit an incorrect password so the dialog stays open, then retype the password

**Reproduction Result(s):**
1. Before the fix, the field content stayed visible as plaintext after re-focus and re-entry

**Fix Result(s):**
1. The password stays masked across focus changes, re-entry and resubmission attempts

#### 1.3.Cancel and reopen keeps the password masked
**Prerequisite(s):**
1. The Authority : Ask Password dialog can be cancelled and reopened

**Step(s):**
1. Open the dialog, type some characters, then cancel the dialog
2. Trigger the function again and type in the reopened dialog

**Reproduction Result(s):**
1. Before the fix, each newly opened dialog showed the typed password as plaintext

**Fix Result(s):**
1. The reopened dialog masks the password input from the first keystroke

### 2.Function Authority Flow

#### 2.1.Correct password proceeds with the function
**Prerequisite(s):**
1. A function with Ask Password authority is available and the correct password is known

**Step(s):**
1. Open the Authority : Ask Password dialog
2. Enter the correct password and confirm

**Reproduction Result(s):**
1. Before the fix, the authority validation itself passed with the correct password and only the masking was broken

**Fix Result(s):**
1. The function proceeds successfully after the correct password is entered
2. The authority validation behavior is unchanged compared with before the fix

#### 2.2.Wrong password is rejected
**Prerequisite(s):**
1. A function with Ask Password authority is available and an intentionally wrong password is prepared

**Step(s):**
1. Open the Authority : Ask Password dialog
2. Enter the wrong password and confirm

**Reproduction Result(s):**
1. Before the fix, the wrong password was rejected as expected

**Fix Result(s):**
1. The wrong password is still rejected with the appropriate message and no bypass of the authority check occurs

### 3.Autofill Behavior Regression

#### 3.1.Browser autofill does not expose or fill the password
**Prerequisite(s):**
1. Chrome with a saved password for the TMS 2.0 site is used for testing

**Step(s):**
1. Open the Authority : Ask Password dialog in Chrome
2. Observe whether the browser offers or autofills the saved password
3. Type the password manually and submit

**Reproduction Result(s):**
1. The original implementation rendered type text and switched the type on focus as an autofill workaround, which stopped working after the React upgrade

**Fix Result(s):**
1. The field keeps type password with autocomplete set to new-password, so Chrome does not autofill the saved password
2. Manual input works normally and remains masked

### 4.Regression Scope
1. TMS 2.0 Authority : Ask Password dialog for all functions using Ask Password authority (confirm, void and similar)
2. TMS 2.0 login page password field (UserLogin.tsx) which already uses a React-controlled type
3. Other password inputs in TMS 2.0 after the React 19.2.5 upgrade
4. Authority validation logic for correct and wrong passwords
