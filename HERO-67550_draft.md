# InterfaceApiSalesEnquiryBirchStreetComponent Performance Optimization Test Report

## Report Information

| Field | Value |
| --- | --- |
| Name | Interface API Sales Enquiry Birch Street Component |
| Code | InterfaceApiSalesEnquiryBirchStreetComponent |
| Issue Key | HERO-67550 |
| Issue Type | Improvement |

## Parameters to Validate

| Parameters Name | Description | Options | Default |
| --- | --- | --- | --- |
| Enquiry date range | Date range used by Birch Street sales enquiry | Short range (less than 7 days), Long range (more than 7 days) | Short range |
| Outlet / shop scope | Shops included in the enquiry | Single Outlet, Multiple Outlets | Single Outlet |

## Data Fields to Validate

| Fields Name | Fields Description | Field Calculation |
| --- | --- | --- |
| Sales enquiry result | Sales data returned by InterfaceApiSalesEnquiryBirchStreetComponent | Must match pre-optimization result for the same request |
| Interface log | Enhanced log output for the Birch Street sales enquiry | Log records enquiry timing, key request identifiers and failure details without breaking the enquiry |

## Report Data Accuracy Verification

### 1.Enquiry Result Unchanged After Optimization

#### 1.1.Single Outlet Short Date Range
**Prerequisite(s):**
1. Birch Street sales enquiry interface is configured
2. Known sales data exists for a single outlet and a date range of less than 7 days
3. A baseline result from the previous build is available

**Step(s):**
1. Trigger InterfaceApiSalesEnquiryBirchStreetComponent for the single outlet and short date range
2. Capture the response payload
3. Compare item, amount and count values with the baseline

**Test Result(s):**
1. Enquiry completes successfully
2. Returned sales data matches the baseline for the same request
3. No data loss or duplicated rows

#### 1.2.Multiple Outlets Long Date Range
**Prerequisite(s):**
1. Sales data exists across multiple outlets for a date range of more than 7 days

**Step(s):**
1. Trigger the Birch Street sales enquiry for multiple outlets and a long date range
2. Verify totals and row counts against known source POS sales

**Test Result(s):**
1. Enquiry completes successfully
2. Aggregated sales data is correct
3. No timeout or truncated result caused by the optimization

### 2.Performance Optimization

#### 2.1.Response Time Improved For Large Enquiry
**Prerequisite(s):**
1. A large sales data set is available (multiple outlets, more than 7 days)
2. Previous average response time for the same enquiry is recorded

**Step(s):**
1. Run the same Birch Street sales enquiry used in the baseline
2. Record elapsed time from request to successful response
3. Repeat the enquiry three times and take the average

**Test Result(s):**
1. Average response time is improved compared with the pre-optimization baseline
2. The enquiry remains stable across repeated runs
3. No error is returned under the same data volume

### 3.Log Enhancement

#### 3.1.Success Path Writes Enhanced Logs
**Prerequisite(s):**
1. Interface log / application log for Birch Street sales enquiry is accessible

**Step(s):**
1. Run a successful sales enquiry
2. Open the interface log
3. Verify timing and request identifiers are recorded

**Test Result(s):**
1. Enhanced log entries are written for the successful enquiry
2. Logs include enough information to trace the request without exposing unnecessary secrets
3. Logging does not change the enquiry result

#### 3.2.Failure Path Writes Enhanced Logs
**Prerequisite(s):**
1. A failure can be simulated (invalid outlet, disconnected dependency, or forced error)

**Step(s):**
1. Trigger the enquiry with the failure condition
2. Open the interface log
3. Verify failure details are recorded

**Test Result(s):**
1. The failure is logged with enhanced diagnostic detail
2. The API still returns the existing error handling result
3. No uncaught exception is left only in a silent failure

## Report Functional Verification

### 1.Existing Birch Street Integration Unchanged
**Prerequisite(s):**
1. Downstream Birch Street consumer of the sales enquiry is available

**Step(s):**
1. Run the enquiry from the existing integration path
2. Confirm the consumer can parse the response as before

**Test Result(s):**
1. Existing integration continues to work
2. Response contract is unchanged except for performance and logging

## Report Compatibility Verification

### 1.Support Data Service Compatibility - MySQL and TiDB
**Step(s):**
1. Run a short-range enquiry against MySQL
2. Run a long-range enquiry against TiDB

**Test Result(s):**
1. Both data sources return correct sales data
2. Performance optimization does not break either data service

## Test Environment
- **Version**: v1.0.0
- **Environment**: HQ Test Environment, QC Cloud Server

## Appendix

### Acceptance Criteria(from JIRA)
1. Performance Optimization of InterfaceApiSalesEnquiryBirchStreetComponent
2. Log Enhancement for the same component
3. Enquiry result accuracy must remain unchanged
