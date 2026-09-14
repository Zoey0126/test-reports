# KDS2-DEV-03-2-20: Get tickets which within last 24 hours Test Report

## Functional Testing

### 1.Active Tickets Retrieval

#### 1.1.Retrieve All Active Tickets Without Pagination Parameters

**Prerequisite(s):**
1. The KDS backend service is running and GET /api/kds/v1/getTickets is accessible
2. The shop has multiple active/working tickets (not delivered, not void/cancelled) with different modified times
3. The shop also has delivered and void tickets modified more than 24 hours ago

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=active and without page or pageSize parameters
2. Compare the returned ticket list against all active tickets in the shop

**Test Result(s):**
1. All active/working tickets are returned in full regardless of their modified time
2. No delivered or void/cancelled ticket is returned by the type=active request
3. The response length equals the total number of active tickets, confirming the former fixed count limits DELIVERED_TICKET_RETRIEVE_LIMIT (50) and VOID_TICKET_RETRIEVE_LIMIT (50) no longer cap the result

#### 1.2.Pagination Parameters Are Ignored for Active Tickets

**Prerequisite(s):**
1. The shop has more than 20 active/working tickets
2. GET /api/kds/v1/getTickets is accessible

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=active, page=1 and pageSize=10
2. Count the returned tickets

**Test Result(s):**
1. The full list of active tickets is returned; page and pageSize are ignored for type=active
2. No active ticket is missing from the response

### 2.Inactive Tickets 24-Hour Time Window

#### 2.1.Delivered and Void Tickets Within Last 24 Hours Are Returned

**Prerequisite(s):**
1. A ticket was delivered 2 hours ago
2. Another ticket was fully voided 5 hours ago
3. GET /api/kds/v1/getTickets is accessible

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=inactive
2. Search the response for both tickets

**Test Result(s):**
1. The delivered ticket with modified time within the last 24 hours is returned
2. The void/cancelled ticket with modified time within the last 24 hours is returned
3. Both tickets show correct status and modified timestamps

#### 2.2.Inactive Tickets Older Than 24 Hours Are Excluded

**Prerequisite(s):**
1. A ticket was delivered 30 hours ago
2. Another ticket was fully voided 3 days ago
3. GET /api/kds/v1/getTickets is accessible

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=inactive
2. Search the response for both tickets

**Test Result(s):**
1. The delivered ticket older than 24 hours is not returned
2. The void/cancelled ticket older than 24 hours is not returned
3. Only delivered and void tickets with ticket.modified >= now - 24 hours are included

### 3.Pagination Behavior for Inactive Tickets

#### 3.1.Default and Custom Pagination with Page Metadata

**Prerequisite(s):**
1. More than 500 delivered/void tickets modified within the last 24 hours exist in the shop
2. GET /api/kds/v1/getTickets is accessible

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=inactive without page or pageSize parameters and inspect the response
2. Send the request again with page=2 and pageSize=100 and inspect the response
3. Fetch all pages sequentially and merge the ticket lists

**Test Result(s):**
1. Without parameters the response defaults to page 1 with default page size 500
2. Page metadata (page, pageSize, totalElements, totalPages) is returned and matches the dataset
3. With page=2 and pageSize=100 the second page of up to 100 tickets is returned
4. Merged pages contain all inactive tickets exactly once, with no duplicates or missing records

#### 3.2.Invalid Pagination Parameter Values

**Prerequisite(s):**
1. GET /api/kds/v1/getTickets is accessible
2. At least one inactive ticket modified within the last 24 hours exists

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=inactive and page=0
2. Send the request with page=-1 and then with pageSize=-1
3. Send the request with a non-numeric pageSize value

**Test Result(s):**
1. Invalid values are handled gracefully per the API contract, either falling back to the defaults (page 1, pageSize 500) or returning a clear client-side validation error
2. The service never returns an unhandled server error or crashes

### 4.Inactive Tickets Are Ordered Newest First

**Prerequisite(s):**
1. Several delivered and void tickets with different modified times within the last 24 hours exist
2. GET /api/kds/v1/getTickets is accessible

**Step(s):**
1. Send GET /api/kds/v1/getTickets with type=inactive
2. Compare the modified timestamps of the returned tickets in order

**Test Result(s):**
1. Inactive tickets are sorted by ticket.modified in descending order (newest first)

### 5.Type Parameter Validation

**Prerequisite(s):**
1. GET /api/kds/v1/getTickets is accessible
2. Active tickets and recent inactive tickets exist in the shop

**Step(s):**
1. Send GET /api/kds/v1/getTickets without the type parameter
2. Send the request with type=all (a value other than active or inactive)

**Test Result(s):**
1. The missing or invalid type parameter is rejected with a clear client-side error describing the required values active or inactive
2. No ticket data is returned for an invalid request and the service remains stable

### 6.KDS Client Reload and Sync

#### 6.1.Client Initialization After Login Loads Active and Inactive Tickets

**Prerequisite(s):**
1. The KDS client is configured against the updated backend
2. The shop has active tickets and delivered/void tickets within the last 24 hours spanning more than one inactive page

**Step(s):**
1. Log in to the KDS client and wait for initialization to complete
2. Observe the tickets listing screen and the API calls made by the client

**Test Result(s):**
1. The client calls the getTickets API for both type=active and type=inactive
2. The client automatically fetches all pages of inactive tickets when more than one page exists
3. All active tickets and recent inactive tickets are displayed on the tickets listing screen

#### 6.2.Reload Button Refreshes the List and Removes Expired Tickets

**Prerequisite(s):**
1. The KDS client is logged in and showing tickets
2. An inactive ticket shown on screen was modified more than 24 hours ago and has aged out of the API result

**Step(s):**
1. Click the Reload button on the top right corner of the tickets listing screen
2. Compare the on-screen tickets with the latest API response

**Test Result(s):**
1. The client reloads tickets from the API for both types
2. Old tickets that are no longer in the API response are deleted from the display
3. Inactive tickets are kept for at least 24 hours and are removed only after a reload or initialization action
