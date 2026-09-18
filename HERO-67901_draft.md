# Handling the deprecated features for RabbitMQ 4.3.X about creating queue Test Report

## Functional Testing

### 1.Publish Notification Queue Creation

#### 1.1.Verify Publish Notification Queue Uses Durable True
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The POS module publish notification shell is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the publish notification shell on the POS module
2. Inspect the queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The publish notification queue is created successfully with durable set to true
2. No "Fail to create queue" error appears in the error log
3. The publish notification workflow operates normally on RabbitMQ 4.3.X

### 2.Central Push Notification Queue Creation

#### 2.1.Verify Central Push Notification Queue Uses Durable True
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The POS module central push notification shell is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the central push notification shell on the POS module
2. Inspect the queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The central push notification queue is created successfully with durable set to true
2. No "Fail to create queue" error appears in the error log
3. The central push notification workflow operates normally on RabbitMQ 4.3.X

### 3.IPTV Interface Response Queue Creation

#### 3.1.Verify IPTV Interface Response Queue Uses Durable True With Auto Delete
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The MQ host exists and the IPTV interface shell is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the IPTV interface shell that creates the response queue
2. Inspect the response queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The IPTV interface response queue is created successfully with durable set to true and auto delete set to true
2. No "Fail to create queue" error appears in the error log
3. The IPTV interface response workflow operates normally on RabbitMQ 4.3.X

### 4.Portal Station Load Test Response Queue Creation

#### 4.1.Verify Portal Station Load Test Response Queue Uses Durable True With Auto Delete
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The portal station load test shell is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the portal station load test shell that creates the response queue
2. Inspect the response queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The portal station load test response queue is created successfully with durable set to true and auto delete set to true
2. No "Fail to create queue" error appears in the error log
3. The portal station load test workflow operates normally on RabbitMQ 4.3.X

### 5.API Portal Response Queue Creation

#### 5.1.Verify API Portal Response Queue Uses Durable True With Auto Delete
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The API portal service is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the API portal flow that creates the response queue
2. Inspect the response queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The API portal response queue is created successfully with durable set to true and auto delete set to true
2. No "Fail to create queue" error appears in the error log
3. The API portal workflow operates normally on RabbitMQ 4.3.X

### 6.Download Sales Data Queue Creation

#### 6.1.Verify Download Sales Data Queue Uses Durable True
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The download sales data shell is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the download sales data shell on the POS module
2. Inspect the queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The download sales data queue is created successfully with durable set to true
2. No "Fail to create queue" error appears in the error log
3. The download sales data workflow operates normally on RabbitMQ 4.3.X

### 7.Portal Station And Smart Station Workflow

#### 7.1.Verify Download Sales Data From Master For Smart Station Queue
**Prerequisite(s):**
1. RabbitMQ 4.3.X is installed and running on the testing environment
2. The smart station download sales data from master workflow is available
3. The error log is cleared before testing

**Step(s):**
1. Trigger the download sales data from master flow on a smart station
2. Inspect the queue created in the RabbitMQ management interface
3. Check the error log for any "Fail to create queue" errors

**Test Result(s):**
1. The queue for the smart station download sales data from master workflow is created with durable set to true
2. No "Fail to create queue" error appears in the error log
3. The smart station download sales data from master workflow operates normally on RabbitMQ 4.3.X

### 8.Error Log Verification After Full Scenario Run

#### 8.1.No Fail To Create Queue Errors After All Scenarios
**Prerequisite(s):**
1. All queue creation scenarios (1.1 to 7.1) have been executed
2. The error log was cleared at the start of the test run

**Step(s):**
1. Open the error log after the full test run
2. Search for the previously reported "Fail to create queue" error string

**Test Result(s):**
1. The error log contains no "Fail to create queue" errors from any of the affected POS module workflows
2. The system works normally without queue creation errors after upgrading RabbitMQ to 4.3.X
