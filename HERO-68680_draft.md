# POS Client (Windows) - Update pos version desc for reference Test Report

## Functional Testing

### 1.POS Server Version Display
**Prerequisite(s):**
1. POS Client (Windows) is installed with the current build
2. POS local server version 1.2.115.0 is installed and running
3. A user account with permission to access the Setting page is available

**Step(s):**
1. Login to the POS Client (Windows)
2. Go to Setting -> Other Information
3. Check the value shown for Best POS Server version

**Test Result(s):**
1. The Best POS Server version shows 1.2.115.0
2. The version information is displayed completely without truncation or layout issue

### 2.Version Information Accuracy And Consistency

#### 2.1.Verify Displayed Version Matches Installed Local Server Version
**Prerequisite(s):**
1. POS Client (Windows) is installed
2. The actual installed POS local server version is known

**Step(s):**
1. Check the Best POS Server version in Setting -> Other Information
2. Compare the displayed version with the installed local server version

**Test Result(s):**
1. The displayed version matches the installed local server version 1.2.115.0
2. No outdated version description is shown

#### 2.2.Verify Version Display Persists After Client Restart
**Prerequisite(s):**
1. POS Client (Windows) is installed and the Best POS Server version is displayed correctly

**Step(s):**
1. Restart the POS Client (Windows)
2. Login again and go to Setting -> Other Information
3. Check the Best POS Server version

**Test Result(s):**
1. The Best POS Server version still shows 1.2.115.0 after restart
2. No version information is lost or reset

#### 2.3.Verify Version Display On Multiple Stations
**Prerequisite(s):**
1. At least two POS stations are available with POS Client (Windows) installed
2. All stations are connected to the same POS local server 1.2.115.0

**Step(s):**
1. Login to the POS Client on each station
2. Go to Setting -> Other Information on each station
3. Check the Best POS Server version on each station

**Test Result(s):**
1. Every station shows the Best POS Server version as 1.2.115.0
2. The displayed version is consistent across all stations

### 3.Version Display With Older Local Server Version
**Prerequisite(s):**
1. POS Client (Windows) current build is installed
2. A POS local server with an older version than 1.2.115.0 is installed

**Step(s):**
1. Login to the POS Client (Windows)
2. Go to Setting -> Other Information
3. Check the Best POS Server version

**Test Result(s):**
1. The Best POS Server version shows the actual installed older version without error
2. The client operates normally and no abnormal message is prompted
