# TMS 2.0 - MySQL table lock race: two users can lock / assign the same table (lock API + update resv field) Test Report

## Functional Testing

### 1.Lock Table API Concurrency

#### 1.1.Two users lock the same vacant table simultaneously

**Prerequisite(s):**
1. A MySQL TMS outlet is available (e.g. Kowloon Shangri-La - Cafe Kool)
2. Two users with two login sessions operate on the same outlet, date and period
3. A vacant target table exists on the floor plan

**Step(s):**
1. Both users trigger the floor plan lock (POST table_states, action lock) on the same vacant table at the same time
2. Check the response of both requests
3. Check the tms_table_states lock records for that table

**Reproduction Result(s):**
1. Both requests can succeed and both users think they hold the lock
2. Two lock records exist for the same table

**Fix Result(s):**
1. Only one request succeeds; the second request fails with the existing locked/blocked error
2. Only one lock record exists for that table

#### 1.2.Ten concurrent lock requests on the same table via JMeter

**Prerequisite(s):**
1. JMeter is prepared with 10 threads, two or more login sessions or tokens, and Ramp-up = 0 (released simultaneously)
2. All threads target the same outlet, date, period and table

**Step(s):**
1. Run the JMeter burst so that 10 threads POST table_states action lock on the same table in the same second
2. Capture the response codes and error keys of all requests
3. Take a DB snapshot of tms_table_states for the table key after the burst

**Reproduction Result(s):**
1. Multiple requests succeed at the same time and multiple lock rows exist for the same table

**Fix Result(s):**
1. Only 1 request succeeds and the other 9 fail, returning "table_is_already_blocked"
2. tms_table_states contains exactly one lock row for that table key after the burst
3. No deadlock occurs (MySQL SHOW ENGINE INNODB STATUS and error log show no deadlock) and the API does not hang until PHP timeout

### 2.Update Reservation Field Concurrency

#### 2.1.Two reservations change table to the same target table concurrently

**Prerequisite(s):**
1. A MySQL TMS outlet is available
2. Two reservation records exist for the same period, each operated by a different user session

**Step(s):**
1. Both users change the table of their reservation to the same target table at the same time via updateResvField (Quick Edit Table / update_fields)
2. Check the result of both saves
3. Check the table assignment of both reservations

**Reproduction Result(s):**
1. Both change-table saves can succeed and both reservations are saved on the same table

**Fix Result(s):**
1. One save succeeds and the other returns the conflict error
2. Only one reservation occupies the target table

#### 2.2.Ten concurrent change-table requests via JMeter

**Prerequisite(s):**
1. Two or more reservation records exist for the same period
2. JMeter thread groups are prepared to represent two reservations changing table to the same target table with Ramp-up = 0

**Step(s):**
1. Run the JMeter test so that update_fields change-table requests hit the same target table concurrently
2. Capture the response codes and error keys
3. Verify the final table assignment of all reservations

**Reproduction Result(s):**
1. Multiple change-table requests succeed and several reservations end up on the same table

**Fix Result(s):**
1. Only one reservation modification request succeeds and the others fail with the conflict error
2. tms_table_states contains one lock row for that table key after the burst and no deadlock occurs

### 3.Lock Release and Resource Cleanup

#### 3.1.GET_LOCK is released after request completion

**Prerequisite(s):**
1. The JMeter bursts from the lock table API and update reservation field scenarios have been executed
2. Database access is available to check named locks

**Step(s):**
1. After the test, check IS_USED_LOCK(name) for the used lock names and performance_schema.metadata_locks
2. Check SHOW PROCESSLIST for waiting queries on the named lock
3. Check for open transactions on tms_table_states

**Reproduction Result(s):**
1. Before the fix no GET_LOCK mechanism is applied on MySQL, so no named lock exists and concurrent writes race freely

**Fix Result(s):**
1. IS_USED_LOCK returns NULL after the requests finish, meaning the lock is released (RELEASE_LOCK is always executed in finally)
2. No GET_LOCK or InnoDB lock is held longer than 30 seconds and no transaction stays open on tms_table_states after the request ends

#### 3.2.No leftover lock rows after failed or timed-out requests

**Prerequisite(s):**
1. JMeter soak test (1-2 minutes) is prepared to catch leaked locks
2. A scenario is included where a lock acquisition fails or a request times out

**Step(s):**
1. Run the soak test including failing and timing-out requests
2. Take a DB snapshot of tms_table_states before and after the test
3. Check for leftover lock rows and open transactions

**Reproduction Result(s):**
1. Failed concurrent requests can leave duplicated lock rows on tms_table_states

**Fix Result(s):**
1. A failed lock acquisition does not insert a lock row and a failed request still releases GET_LOCK
2. No leftover tms_table_states lock rows exist after the test and no InnoDB row locks remain on tms_table_states
3. GET_LOCK timeout is 30 seconds: the waiter either gets the lock within 30 seconds or fails and never waits forever

### 4.Error Message Content

#### 4.1.Locked table error includes the table number during quick table change

**Prerequisite(s):**
1. A target table is already locked or blocked by another user
2. A reservation for the same period exists and can be edited

**Step(s):**
1. Perform a quick table change for the reservation to the already locked target table
2. Observe the returned error message

**Reproduction Result(s):**
1. Both requests can succeed or the error message does not clearly identify the locked table

**Fix Result(s):**
1. The request fails and the returned error message includes the table number of the locked target table

### 5.Cross-Table Isolation

#### 5.1.Concurrent operations on different tables do not block each other

**Prerequisite(s):**
1. Two or more distinct vacant tables exist for the same outlet, date and period
2. JMeter or two user sessions are prepared

**Step(s):**
1. Run concurrent lock requests and change-table requests, each targeting a different table, in parallel
2. Check the responses and the final table states

**Reproduction Result(s):**
1. Requests on different tables can succeed but the same-table race is still present

**Fix Result(s):**
1. All requests on different tables succeed independently with no cross-table blocking
2. The one-winner-per-table rule still applies only within the same table key

### 6.MSSQL Behaviour Unchanged

#### 6.1.MSSQL lock path regression check

**Prerequisite(s):**
1. A MSSQL TMS outlet is available
2. Two user sessions operate on the same outlet, date, period and table

**Step(s):**
1. Repeat the two-user concurrent lock on the same table on MSSQL
2. Repeat the two-user concurrent change-table save on the same table on MSSQL

**Reproduction Result(s):**
1. On MSSQL the second request already fails with the existing locked/blocked error because sp_getapplock is used

**Fix Result(s):**
1. MSSQL behaviour is unchanged after the fix: the second request still fails with the existing locked/blocked error and only one lock row exists

### 7.Regression Scope

The following related modules and scenarios must be retested after the fix:

- Floor plan lock / lock table API (POST table_states, action lock and block)
- Update reservation field change table flow (updateResvField, Quick Edit Table server lock on save introduced by HERO-67685)
- tms_table_states records integrity after normal single-user operations
- Normal single-user lock and change-table flows remain unaffected
- Out of scope items confirmed untouched: UNIQUE index and duplicate-row cleanup, extra occupancy validation on standard TMS
