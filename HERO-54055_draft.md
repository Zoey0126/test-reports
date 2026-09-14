# Language is incorrect if there is language override Test Report

## Report Information

| Field | Value |
| --- | --- |
| Report Name | Report List / Favourite Reports Display Language |
| Report Code | N/A (applies to the report list and the Favourite Reports section) |
| JIRA Issue | HERO-54055 |
| Component | REPORTS |
| Issue Type | Improvement |
| Related Ticket | Ticket #1466393 (report names under Favourite Reports in Chinese language) |

## Report Display Verification

### 1.Favourite Reports Language Display

#### 1.1.Report Names Display in Overridden Language

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. The system default language is English and several reports have been added to Favourite Reports.
  3. Report name translations exist for the override language (for example Simplified Chinese).
**Step(s):**
  1. Log in with a user whose language override is set to Simplified Chinese while the system default language stays English.
  2. Open Reports.
  3. View the report names shown under Favourite Reports.
  4. Verify the report names display in the overridden language.
**Test Result(s):**
  1. Report names under Favourite Reports display in the overridden language instead of the system default language.
  2. No mixed language appears in the Favourite Reports section.
  3. No errors displayed.

#### 1.2.Traditional Chinese Font Rendering in Favourite Reports

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. The user language override is set to Traditional Chinese.
  3. Reports with Traditional Chinese translations are added to Favourite Reports.
**Step(s):**
  1. Log in with the Traditional Chinese override user.
  2. Open Reports and view the Favourite Reports section.
  3. Verify the rendering of the Traditional Chinese report names.
**Reproduction Result(s):**
  1. Font rendering issue observed in the Traditional Chinese translations within the Favourite Reports section (garbled or incorrect glyphs).
**Fix Result(s):**
  1. Traditional Chinese report names render with correct glyphs and readable font.
  2. No garbled characters, boxes or fallback Latin characters appear.
  3. No errors displayed.

#### 1.3.Language Override Applies Per User Only

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. User A has a language override; User B uses the system default language.
  3. Both users have the same reports in Favourite Reports.
**Step(s):**
  1. Log in as User A and check the Favourite Reports language.
  2. Log out and log in as User B on the same environment.
  3. Check the Favourite Reports language for User B.
**Test Result(s):**
  1. User A sees the Favourite Reports names in the override language.
  2. User B still sees the Favourite Reports names in the system default language.
  3. The override of one user does not affect other users.
  4. No errors displayed.

#### 1.4.Remove Language Override and Revert to Default Language

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. The user currently has a language override set and the Favourite Reports names show in the override language.
**Step(s):**
  1. Remove the language override of the user (revert to following the system default language).
  2. Log out and log in again.
  3. Open Reports and view the Favourite Reports section.
**Test Result(s):**
  1. Favourite Reports names revert to the system default language.
  2. No cached override-language names remain after the revert.
  3. No errors displayed.

### 2.Report List Consistency Between Favourite Reports and Full Report List

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. The user has a language override and the same reports exist both in Favourite Reports and in the full report list.
**Step(s):**
  1. Open Reports with the language override applied.
  2. Compare each report name under Favourite Reports with the same report name in the full report list.
**Test Result(s):**
  1. Report names in Favourite Reports and in the full report list are consistent in the override language.
  2. No name mismatch between the two locations.
  3. No errors displayed.

## Report Functional Verification

### 1.Open Favourite Report After Language Override

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. The user has a language override and a favourite report with available data.
**Step(s):**
  1. Open Reports and select a report from Favourite Reports.
  2. Run the report with default parameters.
  3. Verify the report opens and displays data.
**Test Result(s):**
  1. The favourite report opens and runs successfully from the Favourite Reports section.
  2. Report parameters, headers and data display correctly in the overridden language.
  3. No errors displayed.

### 2.Multi-Language Switching

**Prerequisite(s):**
  1. A build with the HERO-54055 fix is deployed.
  2. Reports are added to Favourite Reports.
**Step(s):**
  1. Switch the user language override among English, Simplified Chinese and Traditional Chinese.
  2. After each switch, open Reports and view the Favourite Reports section.
**Test Result(s):**
  1. Favourite Reports names update correctly after every language switch.
  2. Each language shows correct translations and font rendering.
  3. No errors displayed.

## Test Environment

- **Version**: Sprint build containing the HERO-54055 fix
- **Browser**: Chrome 114.0.0.0, Firefox 114.0.0.0, Edge 114.0.0.0
- **Environment**: Local Test Environment, HQ Test Environment, ER Test Environment
