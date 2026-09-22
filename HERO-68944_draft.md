# HERO-68944 TMS2.0 - Online booking language switcher missing when the site has no config zone Test Report

## Functional Testing

### F1. Language Switcher Display Verification

#### F1.1. Language switcher icon and dropdown visibility
**Prerequisite(s):**
  1. Outlet's shop has no config zone assigned
  2. TMS 2.0 online booking page accessible
  3. System default languages are configured
**Step(s):**
  1. Open TMS 2.0 online booking page for an outlet without config zone
  2. Check if language switcher icon is visible on the page
  3. Click the language switcher icon
  4. Verify the language dropdown list appears with available system languages
**Reproduction Result(s):**
  1. Language switcher icon is NOT displayed on the page (classic theme shows broken icon)
  2. Shared media URL is empty on new theme
  3. No language dropdown is available
**Fix Result(s):**
  1. Language switcher icon is displayed correctly
  2. Clicking the icon shows the system language list
  3. Each language option can be selected

### F2. Language Switching Functionality

#### F2.1. Switch language and verify page reload
**Prerequisite(s):**
  1. Outlet's shop has no config zone assigned
  2. Language switcher is visible and functional
**Step(s):**
  1. Open TMS 2.0 online booking page
  2. Open language switcher dropdown
  3. Select a different language from the default
  4. Wait for page reload
  5. Verify the page content is displayed in the selected language
**Reproduction Result(s):**
  1. Language switcher has no language list, cannot select any language
**Fix Result(s):**
  1. Page reloads successfully after language selection
  2. Page content displays in the selected language
  3. No JavaScript errors in browser console

### F3. Shared Media URL Verification

#### F3.1. Shared media path loads correctly without config zone
**Prerequisite(s):**
  1. Outlet's shop has no config zone assigned
**Step(s):**
  1. Open TMS 2.0 online booking page (new theme)
  2. Check shared media URL is not empty
  3. Verify media assets load correctly
**Reproduction Result(s):**
  1. Shared media URL is empty on new theme
  2. Media assets (images, shared resources) fail to load
**Fix Result(s):**
  1. Shared media URL follows system shared data URL
  2. All media assets load correctly

### F4. Regression Testing - Config Zone Assigned Outlets

#### F4.1. Outlets WITH config zone still work correctly
**Prerequisite(s):**
  1. Outlet's shop HAS a config zone assigned
**Step(s):**
  1. Open TMS 2.0 online booking page for an outlet with config zone
  2. Verify language switcher works as expected
  3. Switch language and confirm page reloads correctly
**Fix Result(s):**
  1. Language switcher works normally
  2. Config-zone-specific languages are shown when configured
  3. Page reloads correctly after language switch

## Compatibility Testing

### POS Device Compatibility

#### 1. Chrome Browser - Windows & macOS
**Prerequisite(s):**
  1. Outlet's shop has no config zone
  2. Chrome browser installed
**Step(s):**
  1. Open TMS 2.0 online booking page in Chrome
  2. Verify language switcher displays and works
**Test Result(s):**
  1. Language switcher is visible and functional

#### 2. Safari Browser - macOS & iOS
**Step(s):**
  1. Open TMS 2.0 online booking page in Safari
  2. Verify language switcher displays and works
**Test Result(s):**
  1. Language switcher is visible and functional

#### 3. Mobile Devices (iPhone, iPad)
**Step(s):**
  1. Open TMS 2.0 online booking page on iPhone and iPad
  2. Verify language switcher displays and works
**Test Result(s):**
  1. Language switcher is visible and functional on all mobile devices

### TMS Browser Compatibility

#### 1. Classic Theme vs New Theme
**Prerequisite(s):**
  1. Outlet's shop has no config zone
**Step(s):**
  1. Open booking page using classic theme
  2. Verify language switcher works
  3. Open booking page using new theme
  4. Verify language switcher and shared media work
**Test Result(s):**
  1. Classic theme: language switcher icon visible and functional
  2. New theme: language switcher and shared media URL both work correctly

## Test Environment Information

- Local Test Environment
- HKJC Test Environment
- TMS 2.0 Version 1.2.115.0

## Appendix

### Affected Flow
TMS 2.0 online booking pages that include a language switcher in the theme layout.

### Release Notes Summary
- Fixed: Language switcher icon not displayed when shop has no config zone
- Fixed: System now loads booking language settings without requiring config zone
- Fallback: Default booking languages when no config-zone-specific languages configured
