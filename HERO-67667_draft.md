# Android POS App - Apply Goggle Play policy for camera permission, feature declaration, and compliance Test Report

## Functional Testing

### 1.Runtime Camera Permission Handling

#### 1.1.Camera Permission Is Prompted on First Camera Invocation

**Prerequisite(s):**
1. The updated Android POS app is installed on a device running Android 6.0 (API level 23) or higher
2. The camera permission has not been granted to the app before

**Step(s):**
1. Launch the POS app and invoke a camera function (for example barcode scanning)
2. Observe the system permission dialog

**Test Result(s):**
1. The app validates camera access upon invocation and prompts the user for runtime camera permission if not previously granted
2. No SecurityException or crash occurs while the permission dialog is shown

#### 1.2.Camera Functions After Permission Is Granted

**Prerequisite(s):**
1. The camera permission prompt is currently displayed

**Step(s):**
1. Tap Allow on the camera permission dialog
2. Use the camera function for barcode scanning

**Test Result(s):**
1. The camera opens and the camera function works normally
2. Barcode scanning completes and the parsed value is processed by the app

#### 1.3.Graceful Behaviour When Permission Is Denied

**Prerequisite(s):**
1. The camera permission prompt is currently displayed

**Step(s):**
1. Tap Do not allow on the camera permission dialog
2. Observe the app behaviour after the denial
3. Invoke the camera function again in the same session

**Test Result(s):**
1. The app resumes gracefully without crashing and without an unhandled SecurityException
2. The app does not prompt for the camera permission again within the session after the refusal
3. Other non-camera POS functions keep working normally

#### 1.4.Permission Asked Only Once Across App Launches

**Prerequisite(s):**
1. The user has previously refused the camera permission

**Step(s):**
1. Close and relaunch the POS app
2. Invoke the camera function again

**Test Result(s):**
1. The app does not prompt for the camera permission again, including on the next app launch
2. The app handles the unavailable camera gracefully without crashing

#### 1.5.Permission Revoked from System Settings During a Session

**Prerequisite(s):**
1. The camera permission has been granted and the camera function works

**Step(s):**
1. Revoke the camera permission from the system app settings while the POS app is in the background
2. Return to the POS app and invoke the camera function
3. Grant the permission again from the system settings and invoke the camera function once more

**Test Result(s):**
1. The app detects the missing permission and handles the invocation gracefully without a SecurityException crash
2. The camera function recovers normally once the permission is granted again

### 2.Camera Feature Declaration and Device Compatibility

**Prerequisite(s):**
1. The updated app package is available for inspection
2. A test device without a built-in camera is available

**Step(s):**
1. Inspect the AndroidManifest.xml for the camera permission and the explicit uses-feature declaration for camera
2. Install the app on the device without a built-in camera
3. Launch the app on that device and use non-camera functions

**Test Result(s):**
1. The manifest explicitly declares the camera hardware requirement instead of relying on the implicit required=true behaviour
2. The app is installable on devices without a built-in camera according to the declared hardware feature
3. Non-camera POS functions work normally on the device without a camera

### 3.Camera Frame Data Handling

**Prerequisite(s):**
1. The updated POS app is installed and the camera permission is granted
2. Network capture tooling is available to observe outbound traffic

**Step(s):**
1. Use the camera for live barcode scanning several times
2. Check the device storage for any persisted camera frames or photos
3. Inspect the outbound traffic during scanning

**Test Result(s):**
1. Camera frames are processed ephemerally on-device for live barcode scanning and are not persistently stored
2. Only the parsed string value is sent to the backend and no raw image or video files are transmitted

### 4.Google Play Data Safety Declaration

**Prerequisite(s):**
1. Access to the Google Play Console for the POS app
2. The Data Safety form is open for review

**Step(s):**
1. Review the App Activity section of the Data Safety form
2. Check the declaration for App Interactions
3. Check the Photos and videos category status against the on-device processing behaviour

**Test Result(s):**
1. App Interactions is declared as Collected under the App Activity section to support app functionality
2. The Photos and videos category is not declared because no raw image or video files are collected or transmitted, which matches the on-device ephemeral processing
