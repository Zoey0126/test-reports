# HERO-64069 Test Report - PF Housekeeping Migrate Hot Data to Cold (Historical) Tables Phase 1 (PoC)

## Background

HERO-64069 is part of the Infrasys POS Housekeeping Master Plan. Phase 1 focuses on migrating historical data from operational (hot) tables to corresponding historical (cold) database tables with identical structures. The goal is to reduce active table sizes, improve system performance, and prepare the data foundation for long-term archival in Phase 2. This phase is performance-critical as it directly impacts operational POS and admin functions. Data integrity, correctness, and minimal operational disruption are mandatory. Housekeeping of hot to cold data is performed within Amazon RDS, with daily housekeeping execution, transaction-safe migration with restart/resume capability, and data verification with controlled purge of hot data.

## Functional Testing

### F1.SysHousekeepingShell Scheduled Task Configuration

#### F1.1.Verify Scheduled Task Creation With the New SysHousekeepingShell Shell Option
**Prerequisite(s):**
1. Infrasys POS Backend version 1.2.117.0 is deployed
2. The user has access to System Management > Scheduled Task
3. The housekeeping configuration is maintained by the L3 support team on the Supreme config zone

**Step(s):**
1. Go to System Management > Scheduled Task
2. Click "Add New"
3. Under the "setting" section, open the option list of "Shell"
4. Select the new option "SysHousekeepingShell"
5. Maintain the schedule information and click "Save"
6. Reopen the scheduled task list and review the created record

**Test Result(s):**
1. The new option "SysHousekeepingShell" is available for "Shell" under the "setting" section
2. The scheduled task record is saved successfully without any error message
3. The created scheduled task is displayed in the scheduled task list with the selected shell and schedule information

#### F1.2.Verify Housekeeping Configuration Is Only Available on the Supreme Config Zone
**Prerequisite(s):**
1. Infrasys POS Backend version 1.2.117.0 is deployed
2. The user has access to the Supreme config zone and at least one non-Supreme config zone
3. The housekeeping configuration is maintained by the L3 support team

**Step(s):**
1. Log in to the Supreme config zone and open the housekeeping configuration
2. Verify that the housekeeping configuration can be maintained
3. Switch to a non-Supreme config zone and look for the same housekeeping configuration
4. Compare the availability of the housekeeping configuration between the zones

**Test Result(s):**
1. The housekeeping configuration is available and configurable on the Supreme config zone
2. The housekeeping configuration is not available or not configurable on non-Supreme config zones
3. The SysHousekeepingShell scheduled task applies the configuration maintained on the Supreme config zone

### F2.Housekeeping Execution Conditions

#### F2.1.Verify Cold Data Migration and Purge When Housekeeping Is Enabled With Hot Retention Months Equal to or Greater Than 36
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" on the Supreme config zone
2. The housekeeping hot retention months is configured to a value equal to or greater than 36
3. The SysHousekeepingShell scheduled task is created
4. The hot tables contain records older than the configured retention period and records within the retention period

**Step(s):**
1. Record the current data volume of the hot tables before execution
2. Trigger the SysHousekeepingShell scheduled task
3. Wait for the task to complete
4. Review the data of the hot tables and the corresponding historical (cold) tables

**Test Result(s):**
1. The SysHousekeepingShell scheduled task executes the housekeeping process
2. The specific cold data beyond the decided retention period is migrated to the corresponding historical (cold) tables
3. The migrated cold data is purged from the hot tables
4. The records within the retention period remain in the hot tables and are not migrated or purged

#### F2.2.Verify No Process Is Executed When the Housekeeping Config Is Disabled
**Prerequisite(s):**
1. The housekeeping config is set to "disabled" on the Supreme config zone
2. The SysHousekeepingShell scheduled task is created
3. The hot tables contain records older than the retention period

**Step(s):**
1. Record the current data volume of the hot tables before execution
2. Trigger the SysHousekeepingShell scheduled task
3. Wait for the task run to finish
4. Review the data of the hot tables and the historical (cold) tables

**Test Result(s):**
1. The SysHousekeepingShell scheduled task does not execute any process
2. No data is migrated to the historical (cold) tables
3. No data is purged from the hot tables
4. The data volume of the hot tables remains unchanged

#### F2.3.Verify No Process Is Executed When Hot Retention Months Is Less Than 36
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" on the Supreme config zone
2. The housekeeping hot retention months is configured to a value less than 36
3. The SysHousekeepingShell scheduled task is created
4. The hot tables contain historical records

**Step(s):**
1. Record the current data volume of the hot tables before execution
2. Trigger the SysHousekeepingShell scheduled task
3. Wait for the task run to finish
4. Review the data of the hot tables and the historical (cold) tables

**Test Result(s):**
1. The SysHousekeepingShell scheduled task does not execute any process
2. No data is migrated to the historical (cold) tables
3. No data is purged from the hot tables
4. The data volume of the hot tables remains unchanged

### F3.SysHousekeepingShell Scheduled Task Parameters

#### F3.1.Verify Debug Log File Output Parameter (-l)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created

**Step(s):**
1. Add the parameter "-l" to the SysHousekeepingShell scheduled task
2. Trigger the scheduled task
3. Wait for the task run to finish
4. Check the debug log file output of the task

**Test Result(s):**
1. The debug log file output is turned on by the parameter "-l"
2. The debug log file is generated with the execution detail of the housekeeping process

#### F3.2.Verify Debug Log Verbosity Level Parameter (-v)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created with the parameter "-l" to output the debug log file

**Step(s):**
1. Add the parameter "-v" with level 0 to the scheduled task and trigger the task
2. Review the detail level of the debug log file
3. Repeat the execution with the parameter "-v" level 1 and review the debug log file
4. Repeat the execution with the parameter "-v" level 2 and review the debug log file

**Test Result(s):**
1. The debug log verbosity levels 0 to 2 are supported by the parameter "-v"
2. The debug log file content matches the configured verbosity level
3. A higher verbosity level outputs more detailed debug information in the log file

#### F3.3.Verify Optimize Table After Purge Parameter (-o)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created
3. The hot tables contain records eligible for migration and purge

**Step(s):**
1. Add the parameter "-o" to the SysHousekeepingShell scheduled task
2. Trigger the scheduled task and wait for the migration and purge to complete
3. Review the execution of the operational tables after the purge

**Test Result(s):**
1. The housekeeping process completes the migration and purge
2. OPTIMIZE TABLE is run on the operational tables after the purge when the parameter "-o" is used

#### F3.4.Verify Default Date Batch Limit of Parameter (-d)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created without the parameter "-d"
3. The hot tables contain eligible cold data spanning more than 30 date batches

**Step(s):**
1. Trigger the SysHousekeepingShell scheduled task without the parameter "-d"
2. Wait for the task run to finish
3. Review the number of date batches processed in the run

**Test Result(s):**
1. The scheduled task processes a maximum of 30 date batches by default before terminating
2. The remaining eligible date batches are not processed in the same run

#### F3.5.Verify Explicit and Unlimited Date Batch Limit of Parameter (-d)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created
3. The hot tables contain eligible cold data spanning multiple date batches

**Step(s):**
1. Add the parameter "-d" with a specific value (for example 5) to the scheduled task and trigger the task
2. Review the number of date batches processed in the run
3. Change the parameter "-d" to 0 and trigger the task again
4. Review the number of date batches processed in the run

**Test Result(s):**
1. With a specific value, the scheduled task terminates after processing the configured maximum number of date batches
2. With the value 0, the scheduled task processes the eligible date batches without a batch limit

#### F3.6.Verify Running Only the Specified Plugin Module With Parameter (-m)
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created
3. More than one plugin module with eligible cold data is discovered

**Step(s):**
1. Add the parameter "-m" with a specified plugin module (for example AuditLog) to the scheduled task
2. Trigger the scheduled task and wait for the run to finish
3. Review which plugin modules are processed in the run

**Test Result(s):**
1. Only the specified plugin module (for example AuditLog) is executed by the scheduled task
2. The other discovered plugin modules are not processed in the same run

#### F3.7.Verify Running All Discovered Plugin Modules When Parameter (-m) Is Empty
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created
3. More than one plugin module with eligible cold data is discovered

**Step(s):**
1. Leave the parameter "-m" empty in the scheduled task
2. Trigger the scheduled task and wait for the run to finish
3. Review which plugin modules are processed in the run

**Test Result(s):**
1. All discovered plugin modules are executed by the scheduled task
2. The cold data of every discovered plugin module is migrated and purged according to the retention period

### F4.Cold Data Storage and Search

#### F4.1.Verify Three Cold Tables Are Created on the First Trigger
**Prerequisite(s):**
1. The housekeeping config is set to "enabled" and the hot retention months is equal to or greater than 36
2. The SysHousekeepingShell scheduled task is created
3. The SysHousekeepingShell scheduled task has never been triggered and the cold tables do not exist yet

**Step(s):**
1. Confirm in the database that the cold tables for the housekeeping data do not exist
2. Trigger the SysHousekeepingShell scheduled task for the first time
3. Wait for the task run to finish
4. Review the database tables created by the task

**Test Result(s):**
1. Three tables are created to store all the cold data after the first trigger
2. The migrated cold data is stored in the created historical (cold) tables

#### F4.2.Verify Cold Data Search on the Admin Page With Search Cold Data Set to Yes
**Prerequisite(s):**
1. The SysHousekeepingShell scheduled task has run successfully and cold data exists in the historical (cold) tables
2. The user has access to the admin page

**Step(s):**
1. Set the option "Search Cold Data" to "Yes"
2. Open the admin page and search for the historical data that has been migrated to the cold tables
3. Review the search result returned on the admin page

**Test Result(s):**
1. The option "Search Cold Data" is saved successfully with the value "Yes"
2. The cold data can be searched on the admin page after the successful housekeeping execution
3. The search result includes the records migrated to the historical (cold) tables

## Test Environment

| Item | Details |
| --- | --- |
| QC Tester | Mila Cao (mila.cao@shijigroup.com) |
| QC Reviewer | Briana Li (briana.li@shijigroup.com) |
| Backend Version | Infrasys POS Backend 1.2.117.0 |
| Database | Amazon RDS (operational hot tables and historical cold tables) |
| Configuration | Supreme config zone; housekeeping configuration maintained by the L3 support team |
| Test Scope | System Management > Scheduled Task (SysHousekeepingShell), housekeeping execution conditions and parameters, cold table creation, admin page cold data search |
