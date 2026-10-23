# Audit Log - Support Search for Cold Data for Audit Log Admin Page Test Report

## Functional Testing

### 1.Search From Historical Data Option

#### 1.1.Verify New Option Is Shown When Historical Table Exists
**Prerequisite(s):**
1. The system has historical (cold) audit log tables created and populated by system housekeeping
2. The login account has the authority to access Audit Log Management > Audit Logs

**Step(s):**
1. Log in to Infrasys POS Backend
2. Navigate to Audit Log Management > Audit Logs
3. Locate the option "Hide Unknown Config Zone Records"
4. Observe the options displayed below it

**Test Result(s):**
1. A new option "Search From Historical Data" with values Yes / No is displayed under "Hide Unknown Config Zone Records"
2. The option is displayed only when the historical audit log table exists
3. The default value of the option is "No"

#### 1.2.Verify New Option Is Hidden When No Historical Table Exists
**Prerequisite(s):**
1. The environment has no historical (cold) audit log table created
2. The login account has the authority to access Audit Log Management > Audit Logs

**Step(s):**
1. Log in to Infrasys POS Backend
2. Navigate to Audit Log Management > Audit Logs
3. Observe the search area

**Test Result(s):**
1. The option "Search From Historical Data" is not shown when the historical table does not exist
2. The page keeps the existing behaviour and searches the live table only

#### 1.3.Verify Search With "Search From Historical Data" = No Returns Hot Data Only
**Prerequisite(s):**
1. The historical audit log table exists and contains records older than the retention period
2. The live audit log table contains records newer than the retention period
3. "Search From Historical Data" is set to "No"

**Step(s):**
1. Navigate to Audit Log Management > Audit Logs
2. Set "Search From Historical Data" to "No"
3. Enter a date range that covers both hot and cold data
4. Click "Search"

**Test Result(s):**
1. The search result lists only the hot data records whose date is greater than the retention period
2. Records belonging to the cold data (date less than the retention period) are not listed
3. The total record count matches the hot data only

#### 1.4.Verify Search With "Search From Historical Data" = Yes Returns Cold Data Only
**Prerequisite(s):**
1. The historical audit log table exists and contains records older than the retention period
2. The live audit log table contains records newer than the retention period
3. "Search From Historical Data" is set to "Yes"

**Step(s):**
1. Navigate to Audit Log Management > Audit Logs
2. Set "Search From Historical Data" to "Yes"
3. Enter a date range that covers both hot and cold data
4. Click "Search"

**Test Result(s):**
1. The search result lists only the cold data records whose date is less than the retention period
2. Records belonging to the hot data (date greater than the retention period) are not listed
3. The total record count matches the cold data only

#### 1.5.Verify Search From Historical Data Works With Other Search Criteria
**Prerequisite(s):**
1. Cold data records exist for multiple users, modules and actions
2. "Search From Historical Data" is set to "Yes"

**Step(s):**
1. Navigate to Audit Log Management > Audit Logs
2. Set "Search From Historical Data" to "Yes"
3. Enter a specific user, module and action as search criteria
4. Click "Search"
5. Clear all criteria and click "Search" again

**Test Result(s):**
1. The result contains only cold data records matching all entered criteria
2. The result is filtered correctly and no hot data record is mixed in
3. When no criteria are entered, all cold data records within the retention scope are returned

#### 1.6.Verify Option Is Configurable Only By L3 Support
**Prerequisite(s):**
1. A general administrator account and an L3 support account are available
2. The historical audit log table exists

**Step(s):**
1. Log in with the general administrator account and open Audit Log Management > Audit Logs
2. Attempt to change "Search From Historical Data"
3. Log in with the L3 support account and open the same page
4. Change "Search From Historical Data" to "Yes" and save

**Test Result(s):**
1. The option is not editable by the general administrator account
2. The option is editable and saved successfully by the L3 support account
3. The saved value is retained when the page is reopened

### 2.Shell Export From Cold Data

#### 2.1.Verify Shell Export Supports Exporting Cold Data
**Prerequisite(s):**
1. The audit log shell export tool is available on the backend server
2. The historical audit log table contains records older than the retention period
3. The live audit log table contains records newer than the retention period

**Step(s):**
1. Run the audit log shell export with the new flag that enables export from cold data
2. Set the export date range to cover the cold data records
3. Open the exported result file

**Test Result(s):**
1. The export completes without error
2. The exported file contains the cold data records only
3. The record content and count match the data stored in the historical tables

#### 2.2.Verify Shell Export Without New Flag Exports Hot Data Only
**Prerequisite(s):**
1. The audit log shell export tool is available on the backend server
2. Both hot and cold audit log data exist

**Step(s):**
1. Run the audit log shell export without the new cold data flag
2. Set the export date range to cover both hot and cold data records
3. Open the exported result file

**Test Result(s):**
1. The export completes without error
2. The exported file contains the records from the live table only
3. No cold data record is included in the export

### 3.Regression Scope

The following related areas must be retested after the change:

- Audit Log Management > Audit Logs existing search and export behaviours with "Search From Historical Data" = No
- "Hide Unknown Config Zone Records" option behaviour
- Existing audit log shell export parameters and output format
- System housekeeping migration and purge of audit log records
- Audit log listing pagination, sorting and detail view
- Authority control for Audit Log Management pages

## Compatibility Testing

### 1.Browser Compatibility For Audit Log Admin Page
#### 1.1.Verify Audit Log Admin Page In Supported Browsers
**Prerequisite(s):**
1. Infrasys Platform (Backend) is deployed in the local / HQ test environment
2. The historical audit log table exists

**Step(s):**
1. Open Audit Log Management > Audit Logs in Chrome, Firefox, Microsoft Edge and Safari
2. Set "Search From Historical Data" to "Yes" and search for cold data records
3. Set "Search From Historical Data" to "No" and search for hot data records
4. Open a record detail from the result list

**Test Result(s):**
1. The option "Search From Historical Data" is displayed and rendered correctly in all supported browsers
2. The search result is identical in all supported browsers
3. The record detail is displayed correctly without layout issue

## Test Environment Information

- Local test environment
- HQ test environment
- ER test environment
