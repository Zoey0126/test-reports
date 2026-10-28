# HERO-69782 TMS2.0 - Online Themes V2 - Fix footer copyright year to display current year automatically - Test Report

## Background

The page footer of Online Reservation V2 (Online Themes V2) displayed a **hardcoded copyright year** that did not reflect the current calendar year. This misled guests and required manual theme updates every year.

**User Story / Use Case**

As a venue using Online Themes V2 for online reservations, I want the footer copyright year to automatically reflect the current calendar year so that guests always see an up-to-date year without manual intervention.

**Expected Behavior**

Footer copyright year automatically displays the current calendar year based on system/client date (e.g., "© 2026 Shiji Information Technology (Hong Kong) Limited. All Rights Reserved." when viewed in 2026). The change applies consistently across all Online Themes V2 pages that share the same footer.

**Supported Languages**

English, Traditional Chinese, Simplified Chinese, Japanese, Korean.

## Functional Testing

### 1. Bug Reproduction - Verify Fixed Copyright Year Issue

#### 1.1 Reproduce the Original Bug with Hardcoded Year

**Prerequisite(s):**
1. Access to an environment running Online Themes V2 with the **unfixed** version (before this fix)
2. Browser / client system date set to a year later than the hardcoded footer year
3. A valid Online Reservation V2 booking URL accessible

**Step(s):**
1. Open the Online Reservation V2 booking page in a browser
2. Scroll to the page footer and locate the copyright line
3. Compare the displayed year in the footer against the current calendar year (per system/client date)

**Reproduction Result(s):**
1. Footer displays a **fixed / hardcoded year** (e.g., "© 2024 Shiji Information Technology (Hong Kong) Limited. All Rights Reserved.")
2. The fixed year does **not match** the current calendar year
3. Venue admin cannot change this year through theme settings; requires manual code update

**Fix Result(s):**
1. After deploying the fix, re-opening the same booking page shows the year matching the current calendar year
2. No manual theme update or code change was required

---

### 2. Automatic Year Display - Core Behavior After Fix

#### 2.1 Verify Footer Shows Correct Current Year (Normal Date)

**Prerequisite(s):**
1. Environment running Online Themes V2 **with the fix** deployed
2. Browser / client system date set to today's actual date (e.g., 2026-10-27)
3. Valid Online Reservation V2 booking URL accessible

**Step(s):**
1. Open the Online Reservation V2 booking page in a browser
2. Scroll to the page footer
3. Observe the copyright line year

**Fix Result(s):**
1. Footer copyright year matches the **current calendar year** (e.g., 2026 when system date is 2026)
2. Footer format matches the venue's selected language correctly
3. No manual theme configuration was needed

#### 2.2 Verify Footer Updates When System Date Changes Forward

**Prerequisite(s):**
1. Same environment as 2.1
2. Ability to change browser / client system date

**Step(s):**
1. Confirm footer year = current year YYYY
2. Change system / browser date one year forward (YYYY+1)
3. Refresh the booking page
4. Observe footer year again

**Fix Result(s):**
1. Footer year automatically advances to **YYYY+1** after refresh
2. No cache or stale year is shown after page reload

#### 2.3 Verify Footer Updates When System Date Changes Backward

**Prerequisite(s):**
1. Same environment as 2.1
2. System / browser date changed to next year (YYYY+1) — year shows correctly

**Step(s):**
1. Change system / browser date back to current year (YYYY)
2. Refresh the booking page
3. Observe footer year

**Fix Result(s):**
1. Footer year automatically reverts to **YYYY**
2. Year is always driven by live date, not cached

---

### 3. Multi-Language Footer Verification

#### 3.1 English Footer — Automatic Year

**Prerequisite(s):**
1. Fixed environment from 2.1
2. Online Themes V2 set to English language

**Step(s):**
1. Open booking page with English language selected
2. Scroll to footer
3. Confirm copyright line format and year

**Fix Result(s):**
1. Footer shows: "© 2026 Shiji Information Technology (Hong Kong) Limited. All Rights Reserved." (with 2026 being the current year)
2. Year updates automatically per current date

#### 3.2 Traditional Chinese Footer — Automatic Year

**Prerequisite(s):**
1. Same environment
2. Online Themes V2 set to Traditional Chinese language

**Step(s):**
1. Switch booking page language to Traditional Chinese
2. Scroll to footer
3. Observe copyright year

**Fix Result(s):**
1. Traditional Chinese version of the footer displays correctly with the **current year**
2. Year updates automatically; no manual translation or text change needed

#### 3.3 Simplified Chinese Footer — Automatic Year

**Prerequisite(s):**
1. Same environment
2. Online Themes V2 set to Simplified Chinese language

**Step(s):**
1. Switch booking page language to Simplified Chinese
2. Scroll to footer
3. Observe copyright year

**Fix Result(s):**
1. Simplified Chinese footer displays correctly with the **current year**
2. Year updates automatically

#### 3.4 Japanese Footer — Automatic Year

**Prerequisite(s):**
1. Same environment
2. Online Themes V2 set to Japanese language

**Step(s):**
1. Switch booking page language to Japanese
2. Scroll to footer
3. Observe copyright year

**Fix Result(s):**
1. Japanese footer displays correctly with the **current year**
2. Year updates automatically

#### 3.5 Korean Footer — Automatic Year

**Prerequisite(s):**
1. Same environment
2. Online Themes V2 set to Korean language

**Step(s):**
1. Switch booking page language to Korean
2. Scroll to footer
3. Observe copyright year

**Fix Result(s):**
1. Korean footer displays correctly with the **current year**
2. Year updates automatically

---

### 4. Online Themes V2 Page Coverage — Consistent Footer

#### 4.1 Reservation Booking Page Footer

**Prerequisite(s):**
1. Fixed environment, English language selected

**Step(s):**
1. Open the main Online Reservation V2 booking page
2. Scroll to footer

**Fix Result(s):**
1. Footer shows correct current year, format correct

#### 4.2 Reservation Confirmation Page Footer

**Prerequisite(s):**
1. Fixed environment, English language selected
2. Ability to complete a mock reservation flow

**Step(s):**
1. Complete the booking flow (or reach the confirmation page via test data)
2. On the confirmation page, scroll to footer

**Fix Result(s):**
1. Footer year on confirmation page matches the current calendar year
2. Year is consistent with the booking page footer

#### 4.3 Other Online Themes V2 Pages Sharing the Footer

**Prerequisite(s):**
1. Fixed environment

**Step(s):**
1. Navigate through other Online Themes V2 pages that share the same footer template (e.g., login/register page, profile page, terms page)
2. Check footer on each page

**Fix Result(s):**
1. Every page using the shared footer template shows the **correct current year**
2. No page still shows the old hardcoded year

---

### 5. Cross-Browser / Cross-Device Footer Consistency

#### 5.1 Desktop Chrome

**Prerequisite(s):**
1. Fixed environment
2. Desktop browser Chrome latest

**Step(s):**
1. Open booking page in Chrome desktop
2. Check footer year

**Fix Result(s):**
1. Correct current year displayed

#### 5.2 Desktop Safari

**Prerequisite(s):**
1. Fixed environment
2. Desktop browser Safari latest

**Step(s):**
1. Open booking page in Safari desktop
2. Check footer year

**Fix Result(s):**
1. Correct current year displayed

#### 5.3 Desktop Edge

**Prerequisite(s):**
1. Fixed environment
2. Desktop browser Edge latest

**Step(s):**
1. Open booking page in Edge desktop
2. Check footer year

**Fix Result(s):**
1. Correct current year displayed

#### 5.4 Mobile Safari (iOS)

**Prerequisite(s):**
1. Fixed environment
2. iPhone with Safari

**Step(s):**
1. Open booking page in mobile Safari
2. Check footer year

**Fix Result(s):**
1. Correct current year displayed on mobile viewport

#### 5.5 Mobile Chrome (Android)

**Prerequisite(s):**
1. Fixed environment
2. Android device with Chrome

**Step(s):**
1. Open booking page in mobile Chrome
2. Check footer year

**Fix Result(s):**
1. Correct current year displayed on mobile viewport

---

### 6. Year Transition — Boundary Edge Cases

#### 6.1 Year Rollover on December 31 → January 1

**Prerequisite(s):**
1. Fixed environment
2. System / browser date can be set to 2026-12-31 then 2027-01-01

**Step(s):**
1. Set date to Dec 31, 2026 → footer should show **2026**
2. Set date to Jan 1, 2027 → footer should show **2027**
3. Refresh between steps

**Fix Result(s):**
1. Dec 31 shows 2026; Jan 1 (midnight rollover) shows 2027 correctly
2. No off-by-one error in year boundary

---

### 7. Regression Test — Related Footer & Theme Elements

#### 7.1 Footer Link & Branding — Unchanged

**Prerequisite(s):**
1. Fixed environment

**Step(s):**
1. Open booking page, check all footer elements except the year:
   - "Shiji Information Technology (Hong Kong) Limited" brand name
   - "All Rights Reserved." trailing text
   - Any footer links (e.g., Terms, Privacy)

**Fix Result(s):**
1. Brand name, trailing text, and footer links are **unchanged and functional**
2. Only the year portion changed from hardcoded → dynamic

#### 7.2 Theme Other Dynamic Date Fields — Not Affected

**Prerequisite(s):**
1. Fixed environment

**Step(s):**
1. Check if Online Themes V2 has other dynamic date displays (e.g., promotional banners, copyright lines in other modules)
2. Verify those are unaffected

**Fix Result(s):**
1. Other dynamic date fields behave exactly as before
2. Fix is scoped to the footer copyright year only

#### 7.3 Language-Specific Footer Content — Intact

**Prerequisite(s):**
1. Fixed environment
2. Switch through all 5 supported languages

**Step(s):**
1. For each of the 5 supported languages, review the full footer line for correct localised wording (except year)

**Fix Result(s):**
1. Each language's localised footer text is grammatically correct and complete
2. Only the year value is dynamic; translations remain intact

---

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-69782 |
| Sprint | TMS - 20261019-20261030 |
| Template | infrasys |
| Issue Type | Bug |
| Test Target | Online Themes V2 (Online Reservation V2) footer copyright year |
| Test Objective | Verify automatic current-year display after fix, multi-language support, cross-browser/device consistency, and no regression |
| Supported Languages | English, Traditional Chinese, Simplified Chinese, Japanese, Korean |
| Browsers to Validate | Desktop Chrome / Safari / Edge; Mobile Safari (iOS); Mobile Chrome (Android) |
