# Update external libraries of device manager v2 in 2026 Q4 Test Report

## Functional Testing

### 1.Library Version Verification

#### 1.1.Verify upgraded library versions in the deployment package
**Prerequisite(s):**
1. Device manager v2 build containing the 2026 Q4 library upgrades is deployed in the test environment
2. Access to the build artifact and its dependency manifest is available

**Step(s):**
1. Open the deployment package or generate the dependency report of device manager v2
2. Check the packaged versions of jackson-core, jackson-databind, log4j-api and log4j-core

**Test Result(s):**
1. jackson-core is upgraded to version 2.22.2
2. jackson-databind is upgraded to version 2.22.2
3. log4j-api is upgraded to version 2.26.1
4. log4j-core is upgraded to version 2.26.1
5. No residual old versions of these libraries exist in the package

#### 1.2.Verify device manager v2 startup and service health with upgraded libraries
**Prerequisite(s):**
1. Device manager v2 with the upgraded libraries is deployed
2. Service start and stop permissions are available on the test server

**Step(s):**
1. Start the device manager v2 service
2. Check the startup logs and service health status
3. Call a basic device manager v2 health or enquiry endpoint

**Test Result(s):**
1. The service starts successfully without NoClassDefFoundError, ClassNotFoundException or dependency version conflict errors
2. Startup logs are generated normally by log4j 2.26.1
3. The health or enquiry endpoint returns a normal response

### 2.JSON Data Processing (Jackson)

#### 2.1.Verify JSON serialization and deserialization of device data
**Prerequisite(s):**
1. Device manager v2 service is running with jackson-core and jackson-databind 2.22.2
2. A test device record exists in the system

**Step(s):**
1. Call a device manager v2 API that returns device data in JSON format
2. Submit a request payload containing complete device fields to a device manager v2 API
3. Compare the request and response data with the stored device record

**Test Result(s):**
1. Device data is serialized to JSON correctly with no data loss or field corruption
2. Incoming JSON payloads are deserialized into the expected device objects
3. Field values such as numbers, dates and text round-trip consistently between request, storage and response

#### 2.2.Verify handling of malformed JSON payload
**Prerequisite(s):**
1. Device manager v2 service is running with jackson 2.22.2
2. An API client tool is available to send custom requests

**Step(s):**
1. Send a request with syntactically invalid JSON (e.g. a missing closing brace) to a device manager v2 API
2. Send a request with a valid JSON structure but a wrong data type in a field (e.g. text in a numeric field)

**Test Result(s):**
1. The service rejects the malformed payload with a proper error response instead of crashing
2. A clear error message is returned and logged, and other requests continue to be processed normally

#### 2.3.Verify JSON processing with special characters and large payload
**Prerequisite(s):**
1. Device manager v2 service is running with jackson 2.22.2

**Step(s):**
1. Submit a device payload containing special characters and unicode text (e.g. accents, CJK characters, quotes) in name fields
2. Submit a device payload with a large number of items or long text values near the boundary of normal operation

**Test Result(s):**
1. Special characters are serialized, stored and returned without corruption or encoding errors
2. The large payload is processed successfully and the response remains consistent

### 3.Logging Behavior (Log4j)

#### 3.1.Verify log output at different log levels
**Prerequisite(s):**
1. Device manager v2 is running with log4j-api and log4j-core 2.26.1
2. Access to the log configuration and log files is available

**Step(s):**
1. Trigger normal device operations and check the INFO level logs
2. Trigger an error condition (e.g. an invalid request) and check the ERROR level logs with stack traces
3. Adjust the configured log level if applicable and confirm the output follows the configuration

**Test Result(s):**
1. Normal operations produce INFO level logs with correct timestamps and message content
2. Error conditions produce ERROR level logs including full stack traces
3. Log output respects the configured log level and format

#### 3.2.Verify log file rotation and continued writing
**Prerequisite(s):**
1. Log rotation is configured for the device manager v2 logs

**Step(s):**
1. Generate a volume of logs that exceeds the rotation threshold
2. Continue device operations after rotation occurs

**Test Result(s):**
1. Log files rotate correctly and no log entries are lost or corrupted during rotation
2. The service keeps writing logs to the new file without restart or errors

### 4.Device Manager v2 Functional Regression

#### 4.1.Device registration and enquiry flow
**Prerequisite(s):**
1. Device manager v2 with the upgraded libraries is running
2. A valid device is available for registration

**Step(s):**
1. Register a new device through the standard device manager v2 flow
2. Enquire the registered device details
3. Modify the device information and save

**Test Result(s):**
1. The device is registered successfully and appears in the device list
2. The enquiry returns correct and complete device details
3. The modification is saved and reflected in subsequent enquiries

#### 4.2.Device status update and deactivation flow
**Prerequisite(s):**
1. A registered device exists in device manager v2

**Step(s):**
1. Update the device status (e.g. enable or disable) through the standard flow
2. Deactivate the device
3. Attempt to operate with the deactivated device

**Test Result(s):**
1. The status update is applied and displayed correctly
2. The deactivation completes successfully
3. The deactivated device is not allowed to perform operations

#### 4.3.Invalid device operation is rejected
**Prerequisite(s):**
1. Device manager v2 is running and accessible

**Step(s):**
1. Attempt to register a device with missing mandatory fields
2. Attempt to enquire a device with a non-existent device ID

**Test Result(s):**
1. Registration with missing mandatory fields is rejected with a clear validation message
2. The enquiry with a non-existent device ID returns a proper not-found response without system errors
