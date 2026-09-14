# TMS2.0 - Searching data trigger WAF interception, and the popped prompt message has no translation. Test Report

## Functional Testing

### 1.Search View WAF Interception Prompt

#### 1.1.Search with special characters shows a translated error message

**Prerequisite(s):**
1. Tester account has access to the TMS Operation portal on environment HQ1.
2. A WAF rule is active on the test environment that intercepts requests containing special characters.
3. The UI language of the portal is set to a supported non-English language (e.g., Simplified Chinese).

**Step(s):**
1. Go to TMS Operation > Search View.
2. Fill in "'!@#" in a search field.
3. Click the [Search] button.

**Reproduction Result(s):**
1. A dialog pops up showing the raw text "access_denied_request_violates_security_policy" and the error message is not translated.

**Fix Result(s):**
1. The pop-up dialog shows the error message for WAF interception translated into the selected UI language.
2. No raw translation key such as "access_denied_request_violates_security_policy" is displayed to the user.

#### 1.2.Search with special characters under English language shows the English message

**Prerequisite(s):**
1. Tester account has access to the TMS Operation portal on environment HQ1.
2. A WAF rule is active on the test environment that intercepts requests containing special characters.
3. The UI language of the portal is set to English.

**Step(s):**
1. Go to TMS Operation > Search View.
2. Fill in "'!@#" in a search field.
3. Click the [Search] button.

**Reproduction Result(s):**
1. The pop-up dialog shows the untranslated raw text "access_denied_request_violates_security_policy".

**Fix Result(s):**
1. The pop-up dialog shows the English error message for WAF interception.
2. The message text is readable and consistent with the English translation resource.

#### 1.3.Search with normal keywords does not trigger WAF interception

**Prerequisite(s):**
1. Tester account has access to the TMS Operation portal on environment HQ1.
2. Searchable data (e.g., member or reservation records) exists in the environment.

**Step(s):**
1. Go to TMS Operation > Search View.
2. Fill in a normal keyword (e.g., an existing member name) in a search field.
3. Click the [Search] button.

**Reproduction Result(s):**
1. Not applicable - the normal search flow was not affected by the reported issue.

**Fix Result(s):**
1. The search executes normally and returns matching results.
2. No WAF interception dialog or HTTP 403 error appears.

### 2.Member Auto-Search WAF Interception Prompt

#### 2.1.Walk In member auto-search with special characters pops up an error message

**Prerequisite(s):**
1. Tester account has access to TMS Operation on environment HQ1.
2. A WAF rule is active that intercepts requests containing special characters.
3. The browser Developer Tools are available.

**Step(s):**
1. Go to TMS Operation > Current View > Walk In.
2. Right-click the page to open Developer Tools and switch to the Network panel.
3. Fill in "'!@#" in the member info field to trigger the auto-search of the member.

**Reproduction Result(s):**
1. An HTTP 403 error is observed in the browser network requests, but no pop-up message is displayed on the page.

**Fix Result(s):**
1. After WAF interception is triggered, an error message pops up on the page.
2. The pop-up message is translated according to the selected UI language.

#### 2.2.New reservation view member auto-search pops up an error message

**Prerequisite(s):**
1. Tester account has access to TMS Operation on environment HQ1.
2. A WAF rule is active that intercepts requests containing special characters.

**Step(s):**
1. Go to TMS Operation > New reservation view.
2. Fill in "'!@#" in the member info field to trigger the auto-search of the member.

**Reproduction Result(s):**
1. An HTTP 403 error occurs on the auto-search request, but no error message is displayed on the page.

**Fix Result(s):**
1. After WAF interception is triggered, an error message pops up on the page.
2. The pop-up message content is translated and no raw error key is shown.

#### 2.3.Member View search with special characters pops up an error message

**Prerequisite(s):**
1. Tester account has access to TMS Operation on environment HQ1.
2. A WAF rule is active that intercepts requests containing special characters.

**Step(s):**
1. Go to TMS Operation > Member View.
2. Fill in "'!@#" in the member search field.

**Reproduction Result(s):**
1. The search request is intercepted (HTTP 403) and no pop-up message is displayed.

**Fix Result(s):**
1. After WAF interception is triggered, an error message pops up on the page with translated content.

#### 2.4.Normal member auto-search works without WAF pop-up

**Prerequisite(s):**
1. Tester account has access to TMS Operation on environment HQ1.
2. Member records exist in the environment.

**Step(s):**
1. Go to TMS Operation > Current View > Walk In.
2. Fill in a normal member name or phone number in the member info field to trigger the auto-search.

**Reproduction Result(s):**
1. Not applicable - the normal member auto-search flow was not affected by the reported issue.

**Fix Result(s):**
1. The member auto-search executes normally and matching member results are displayed.
2. No WAF interception dialog or HTTP 403 error appears.

### 3.Regression Scope

1. TMS Operation > Search View: searching with normal keywords on all search fields.
2. TMS Operation > Current View > Walk In: member info auto-search with normal input.
3. TMS Operation > New reservation view: member info auto-search with normal input.
4. TMS Operation > Member View: member search with normal input.
5. Search fields on other pages (e.g., New Page) that were reported to share the same issue.
6. UI language switching and translation display of other system error dialogs.
