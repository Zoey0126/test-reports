# POS Interface - Request to enhance External Webhook for HQ On-Premises Server  (Dev2/2) Test Report

Date time: 2026-09-14
Author: QA Team

## Functional Testing

**Prerequisite(s):**
1. The environment is set up with a Dummy Cloud HQ server and an On-Premises HQ server, both deployed with the External Webhook module (Infrasys Cloud Backend version 1.2.115.0).
2. The API Bridge (API Portal) is deployed and accessible; an application rule matching the On-Premises HQ webhook rule application name exists under API Portal -> Advanced Control -> Rules (for this case DefaultApplication - Shiji ST Vendor (PF Testing)), and its Secret Key is available for testing.
3. A SevenRooms webhook rule is configured on On-Premises HQ under API Management -> External Webhook Management, with reference code ShopRefId and venue id 12345.
4. Log files are accessible for verification: webhook.log on the API Bridge host (under /tmp/logs/) and pos_process_webhook.log on the On-Premises HQ server.
5. A REST client (e.g., Postman) is available to build the SevenRooms webhook test requests.

### 1.External Webhook Management on On-Premises HQ

#### 1.1.Verify the External Webhook Management navigation item on On-Premises HQ

**Prerequisite(s):**
1. The On-Premises HQ server is deployed with the External Webhook module (version 1.2.115.0).
2. An account with permission to access API Management is available.

**Step(s):**
1. Log in to the On-Premises HQ server.
2. Navigate to API Management.
3. Locate the External Webhook Management navigation item and click it.

**Test Result(s):**
1. The External Webhook Management navigation item is displayed under API Management on On-Premises HQ.
2. The External Webhook Management page opens without error.

#### 1.2.Verify the SevenRooms webhook rule on the View Detail page

**Prerequisite(s):**
1. The SevenRooms webhook rule is configured on On-Premises HQ with reference code ShopRefId and venue id 12345.

**Step(s):**
1. Navigate to API Management -> External Webhook Management on On-Premises HQ.
2. Check the webhook rule list for the SevenRooms rule.
3. Click the View button of the SevenRooms rule to enter the View Detail page.
4. Check the webhook rule details.

**Test Result(s):**
1. The SevenRooms webhook rule is listed on the External Webhook Management page.
2. The View Detail page shows the webhook rule details, and the reference code and venue id match the configured values (ShopRefId and 12345).

#### 1.3.Verify the External Webhook Management module on the Dummy Cloud HQ server

**Prerequisite(s):**
1. The Dummy Cloud HQ server is deployed with the External Webhook module (version 1.2.115.0).
2. An account with permission to access API Management is available.

**Step(s):**
1. Log in to the Dummy Cloud HQ server.
2. Navigate to API Management -> External Webhook Management.
3. Check the webhook rule list.

**Test Result(s):**
1. The External Webhook Management module is accessible on the Dummy Cloud HQ server.
2. The webhook rules are listed and viewable without error.

### 2.SevenRooms Webhook Event Delivery via API Bridge

#### 2.1.Deliver a valid SevenRooms reservation update event with ARRIVED status

**Prerequisite(s):**
1. All general prerequisites are met.
2. The Secret Key of the DefaultApplication rule is retrieved from API Portal -> Advanced Control -> Rules -> View Detail.

**Step(s):**
1. Build a testing request in Postman to simulate a SevenRooms external webhook: send a POST request to {baseUrl}/api_portal/webhook/seven_rooms/v1/resv_update/{secretKey}, where {baseUrl} is the API Bridge URL and {secretKey} is the Secret Key of the DefaultApplication rule.
2. Use the following request body: [{"event_type":"updated","entity":{"id":"sr-resv-test-001","reference_code":"ShopRefId","venue_id":"12345","status":"ARRIVED","table_numbers":["1"],"arrived_guests":2,"seated_time":"2026-09-03T14:00:00+08:00","updated":"2026-09-03T14:00:30+08:00","first_name":"Test","last_name":"Guest","is_vip":false,"reservation_type":"WALK_IN","notes":"","prepayment":0,"tags":[]}}]
3. Send the request.
4. Check the webhook.log on the API Bridge host (under /tmp/logs/) for the POST Data log of this request.

**Test Result(s):**
1. The request is accepted by the API Bridge and the webhook.log records the POST data of the request.
2. The logged reference code and venue_id match the values from the webhook rule (ShopRefId and 12345).
3. The logged requestParams contain webhook seven_rooms and resv_update, showing that the API Bridge receives the request correctly.

#### 2.2.Deliver a reservation update event with empty optional fields and boundary values

**Prerequisite(s):**
1. All general prerequisites are met.

**Step(s):**
1. Build a POST request to {baseUrl}/api_portal/webhook/seven_rooms/v1/resv_update/{secretKey} with a reservation update payload in which the optional fields are empty or zero-valued and the status is not ARRIVED: [{"event_type":"updated","entity":{"id":"sr-resv-test-002","reference_code":"ShopRefId","venue_id":"12345","status":"CONFIRMED","table_numbers":[],"arrived_guests":0,"seated_time":"2026-09-03T15:00:00+08:00","updated":"2026-09-03T15:00:30+08:00","first_name":"Bound","last_name":"Ary","is_vip":false,"reservation_type":"WALK_IN","notes":"","prepayment":0,"tags":[]}}]
2. Send the request.
3. Check the webhook.log on the API Bridge host.
4. Check the pos_process_webhook.log on the On-Premises HQ server.

**Test Result(s):**
1. The request is accepted and logged by the API Bridge without error.
2. The event is forwarded to On-Premises HQ and processed without error.
3. As the reservation is not in ARRIVED status and contains no table numbers, no check is sent to the Local Server and no unexpected error appears in the logs.

#### 2.3.Deliver a reservation update event with an invalid secret key

**Prerequisite(s):**
1. All general prerequisites are met.
2. An invalid Secret Key value (e.g., invalidSecretKey000) is prepared.

**Step(s):**
1. Build a POST request to {baseUrl}/api_portal/webhook/seven_rooms/v1/resv_update/invalidSecretKey000 with the same valid request body as used in Case 2.1.
2. Send the request.
3. Check the webhook.log on the API Bridge host and the pos_process_webhook.log on the On-Premises HQ server.

**Test Result(s):**
1. The request is rejected by the API Bridge because the Secret Key does not match any rule.
2. No processWebhook call is forwarded to On-Premises HQ via MQ.
3. No new processing entry appears in pos_process_webhook.log on On-Premises HQ.

#### 2.4.Deliver a reservation event with an unknown event type

**Prerequisite(s):**
1. All general prerequisites are met.

**Step(s):**
1. Build a POST request to {baseUrl}/api_portal/webhook/seven_rooms/v1/unknown_event/{secretKey} with the same valid request body as used in Case 2.1.
2. Send the request.
3. Check the webhook.log on the API Bridge host and the logs on the On-Premises HQ server.

**Test Result(s):**
1. The API Bridge does not route the request to the SevenRooms reservation update handler.
2. No processWebhook call is forwarded to On-Premises HQ for the unknown event type.
3. No side effects occur on On-Premises HQ (no check is created or sent to the Local Server).

#### 2.5.Deliver a reservation update event with reference_code and venue_id not matching the webhook rule

**Prerequisite(s):**
1. All general prerequisites are met.

**Step(s):**
1. Build a POST request to {baseUrl}/api_portal/webhook/seven_rooms/v1/resv_update/{secretKey} with a valid request body except that reference_code is set to an unmapped value InvalidShopRef and venue_id is set to 99999.
2. Send the request.
3. Check the webhook.log on the API Bridge host, the On-Premises Call HQ via MQ log, and the pos_process_webhook.log on the On-Premises HQ server.

**Test Result(s):**
1. The reference_code and venue_id cannot be mapped to a valid shop_code and outlet_code.
2. The event is rejected or ignored by On-Premises HQ with a no-match or error entry in the log.
3. No check is sent to the Local Server.

### 3.On-Premises HQ Webhook Processing and Local Server Delivery

#### 3.1.Verify processWebhook forwarding and shop_code/outlet_code mapping via MQ

**Prerequisite(s):**
1. A valid SevenRooms reservation update event has been delivered through the API Bridge (refer to Case 2.1).

**Step(s):**
1. On the API Bridge, check the On-Premises Call HQ via MQ log.
2. Verify that the log shows the API Bridge forwarding the request to On-Premises HQ with a function call of processWebhook().
3. Verify that shop_code and outlet_code are mapped correctly from the passed parameters reference_code (ShopRefId) and venue_id (12345).

**Test Result(s):**
1. The MQ log shows that the API Bridge forwards the request to On-Premises HQ with the function call of processWebhook() (the new route built on On-Prem HQ).
2. shop_code and outlet_code are mapped correctly from reference_code and venue_id according to the webhook rule.

#### 3.2.Verify checks are sent to the Local Server by On-Premises HQ

**Prerequisite(s):**
1. A valid SevenRooms reservation update event with status ARRIVED and table_numbers ["1"] has been processed by On-Premises HQ (refer to Case 2.1).

**Step(s):**
1. On the On-Premises HQ server, open pos_process_webhook.log.
2. Check for the "send to LS" log entry.
3. Verify the count value, which is the number of checks that are going to be sent to the Local Server.
4. Verify that the "Call LS" log appears count times, each performing the function send_check for a different check.

**Test Result(s):**
1. The "send to LS" log appears in pos_process_webhook.log with the expected count of checks.
2. The "Call LS" log appears exactly count times, performing send_check for each check.
3. All checks are delivered to the Local Server successfully.

## Test Environment

- QC Test Environment
  - Dummy Cloud HQ Server (QC Cloud Server Trunk Version)
  - On-Premises HQ Server (QC Office Server Trunk Version)
  - API Bridge (API Portal)
- API Module Version: Infrasys Cloud Backend / External Webhook - 1.2.115.0
