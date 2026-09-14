# TMS 2.0 - POS getMembersByConditions fails on SQL Server (GROUP BY memb_id with SELECT *) Test Report

## Functional Testing

### 1.Member Search with Membership Join on SQL Server

#### 1.1.Search member by membership number on SQL Server

**Prerequisite(s):**
1. A SQL Server TMS environment is available as the repro target
2. At least 1 active member with membership record(s) (membership no) exists
3. Postman is available to call the TMS REST API, or POS member search uses this API

**Step(s):**
1. Call GET {base}/rest_api/tms/members_by_conditions?is_json_response=true&membership_no={membershipNo}&page=1&limit=10
2. Check the HTTP status code and the response body

**Reproduction Result(s):**
1. SQL Server throws error Msg 8120: Column is invalid in the select list because it is not contained in either an aggregate function or the GROUP BY clause
2. The API call fails and no paged member list is returned

**Fix Result(s):**
1. The API returns HTTP 200 with members[] and paging { page, current_page_record_count, total_record_count, page_count }
2. No SQL Msg 8120 occurs and the response field contract is unchanged compared with the pre-fix reply

#### 1.2.Search member by membership group number on SQL Server

**Prerequisite(s):**
1. A SQL Server TMS environment is available
2. At least 1 active member with a membership group no exists

**Step(s):**
1. Call members_by_conditions with the membership_group_no join condition
2. Check the HTTP status code and the response body

**Reproduction Result(s):**
1. SQL Server throws error Msg 8120 and the search fails

**Fix Result(s):**
1. The API returns HTTP 200 with members[] and paging data
2. The returned member matches the membership group no condition and no SQL error occurs

#### 1.3.Search member by interface code on SQL Server

**Prerequisite(s):**
1. A SQL Server TMS environment is available
2. At least 1 active member with a membership interface code exists

**Step(s):**
1. Call members_by_conditions with the interface_code join condition
2. Check the HTTP status code and the response body

**Reproduction Result(s):**
1. SQL Server throws error Msg 8120 and the search fails

**Fix Result(s):**
1. The API returns HTTP 200 with members[] and paging data
2. The returned member matches the interface code condition and no SQL error occurs

### 2.Paging and Sorting

#### 2.1.Paging is not duplicated when one member matches multiple membership rows

**Prerequisite(s):**
1. A SQL Server TMS environment is available
2. The same member has more than 1 membership row matching the search condition

**Step(s):**
1. Call members_by_conditions with a membership join condition that matches all membership rows of the same member
2. Check the member entries and the paging counts in the response

**Reproduction Result(s):**
1. The SQL Server query fails with Msg 8120, so duplicated paging rows cannot even be observed

**Fix Result(s):**
1. The member appears exactly once in the paged list because distinct memb_id is paged first
2. total_record_count and page_count reflect distinct members only

#### 2.2.Sorting by membership number is deterministic

**Prerequisite(s):**
1. A SQL Server TMS environment is available
2. The same member has more than 1 membership row with different membership numbers

**Step(s):**
1. Call members_by_conditions with sort by membership number
2. Repeat the request several times and observe the ordering of results

**Reproduction Result(s):**
1. The ORDER BY membership number query fails with Msg 8120 on SQL Server

**Fix Result(s):**
1. The sort is deterministic using MIN(mmsh_member) when one member matches multiple memberships
2. Repeated requests return the members in the same stable order

### 3.Cross-Database Consistency

#### 3.1.MySQL and SQL Server return the same paged member list

**Prerequisite(s):**
1. Both a SQL Server environment and a MySQL environment are available with the same member and membership data

**Step(s):**
1. Call members_by_conditions with the same membership join condition, page and limit on SQL Server
2. Call the same API on MySQL
3. Compare the member list and paging data of both responses

**Reproduction Result(s):**
1. On MySQL (ONLY_FULL_GROUP_BY off) the paged member list returns normally, but on SQL Server the same search fails with Msg 8120, so the results cannot match

**Fix Result(s):**
1. SQL Server and MySQL both return the same paged member list and paging data
2. The result set and paging are identical for the same search on both databases

### 4.Regression Scope

The following related modules and scenarios must be retested after the fix:

- POS member search that uses getMembersByConditions (payload fields unchanged, no new or removed fields)
- Member search without membership join conditions (no GROUP BY path)
- Paging counts based on COUNT(DISTINCT memb_id)
- MySQL regression with strict and non-strict ONLY_FULL_GROUP_BY modes
- Full guest profile loading after paging (memb_id IN list)
