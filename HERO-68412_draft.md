# Update external libraries of POS launcher program in 2026 Q4 Test Report

## Functional Testing

### 1.POS Launcher Startup Verification

#### 1.1.Launcher starts and loads all updated libraries without classpath conflicts

**Prerequisite(s):**
1. The POS launcher build with updated external libraries is deployed to the POS terminal.
2. The terminal is connected to power and the backend network is reachable.
3. Peripheral devices (printer, cash drawer, card reader) are connected.

**Step(s):**
1. Power on the POS terminal and wait for the launcher program to initialize.
2. Observe the startup sequence and check for any error pop-ups or splash screen failures.
3. Open the launcher log file and search for library loading errors or classpath conflict messages.
4. Confirm that the main POS application is launched from the updated launcher.

**Test Result(s):**
1. The POS launcher starts without any error pop-ups during initialization.
2. All updated external libraries are loaded without classpath conflicts reported in the log.
3. The main POS application is launched successfully from the updated launcher.

#### 1.2.Startup log shows no ERROR or FATAL entries from updated libraries

**Prerequisite(s):**
1. The POS launcher with updated log4j-api and log4j-core is deployed.
2. Access to the launcher log directory.

**Step(s):**
1. Start the POS launcher and let it complete initialization.
2. Open the latest launcher log file.
3. Filter log entries for ERROR and FATAL levels related to the updated libraries.

**Test Result(s):**
1. No ERROR or FATAL entries are produced by log4j-api or log4j-core during startup.
2. The log output format and timestamp entries are consistent with the expected log4j configuration.

### 2.Core POS Operations After Library Update

#### 2.1.Sales transaction and receipt printing operate normally

**Prerequisite(s):**
1. The POS launcher with updated libraries is running.
2. A test merchant and terminal are configured.
3. A receipt printer is connected and online.

**Step(s):**
1. Open a new sales transaction on the POS application.
2. Add items to the order and apply any required discounts.
3. Initiate and complete a card payment.
4. Print the receipt and verify the printed content.

**Test Result(s):**
1. The sales transaction is completed without any runtime error.
2. The receipt is printed with the correct transaction details and formatting.

#### 2.2.XML configuration loading works with updated xom library

**Prerequisite(s):**
1. The POS launcher with the updated xom library is deployed.
2. POS configuration XML files are present on the terminal.

**Step(s):**
1. Start the POS launcher and verify that configuration XML files are parsed at startup.
2. Confirm that all configured payment methods and terminal settings are available in the POS application.
3. Modify a configuration value, restart the launcher, and confirm the updated value is applied.

**Test Result(s):**
1. All XML configuration files are parsed correctly by the updated xom library.
2. Configuration changes are reflected after a launcher restart without any parse errors.

#### 2.3.Security and encryption operations work with updated spring-security-crypto

**Prerequisite(s):**
1. The POS launcher with the updated spring-security-crypto library is deployed.
2. Encrypted credentials and keys are configured on the terminal.

**Step(s):**
1. Start the POS launcher and verify that encrypted passwords are decrypted correctly at startup.
2. Perform a secure connection to the backend service using encrypted credentials.
3. Verify that sensitive data stored locally remains properly encrypted.

**Test Result(s):**
1. Encrypted credentials are decrypted successfully and the backend connection is established.
2. Local sensitive data remains correctly encrypted with the updated library.

### 3.Library Version Verification

#### 3.1.All external libraries match the target versions specified for the release

**Prerequisite(s):**
1. The POS launcher build artifacts are available on the terminal.
2. Access to the launcher lib directory or dependency manifest file.

**Step(s):**
1. Locate the POS launcher library directory or open the dependency manifest.
2. List the installed version of each external library.
3. Compare each installed version against the target version defined for the 2026 Q4 release.

| Library | Target Version | Actual Version | Result |
| --- | --- | --- | --- |
| jssc | 2.10.4 | 2.10.4 | Pass |
| json | 20260814 | 20260814 | Pass |
| xom | 1.4.6 | 1.4.6 | Pass |
| jackson-core | 2.22.2 | 2.22.2 | Pass |
| spring-security-crypto | 7.1.1 | 7.1.1 | Pass |
| amqp-client | 5.35.0 | 5.35.0 | Pass |
| log4j-api | 2.26.1 | 2.26.1 | Pass |
| log4j-core | 2.26.1 | 2.26.1 | Pass |
| groovy | 5.1.1 | 5.1.1 | Pass |
| groovy-jsr223 | 5.1.1 | 5.1.1 | Pass |
| libphonenumber | 9.0.38 | 9.0.38 | Pass |

**Test Result(s):**
1. All eleven external libraries are present at their target versions in the POS launcher.
2. No outdated or missing library versions are found during the verification.

### 4.Regression Testing

#### 4.1.Logging functionality works with updated log4j-api and log4j-core

**Prerequisite(s):**
1. The POS launcher with updated log4j-api and log4j-core is deployed.
2. The log4j configuration file is present and valid.

**Step(s):**
1. Trigger various log levels (DEBUG, INFO, WARN, ERROR) by performing POS operations.
2. Open the configured log file and verify that entries are written for each level.
3. Trigger a log file rotation event and verify that rotated files are created correctly.
4. Confirm that the log format includes timestamp, thread name, and log level.

**Test Result(s):**
1. Log entries for all levels are written correctly to the configured log file.
2. Log rotation works as expected and the log format is consistent with the configuration.

#### 4.2.Serial communication works with updated jssc library

**Prerequisite(s):**
1. The POS launcher with the updated jssc library is deployed.
2. A serial peripheral device such as a card reader or barcode scanner is connected.

**Step(s):**
1. Connect the serial peripheral device to the POS terminal.
2. Send a test command to the device from the POS application.
3. Verify that the device responds with the expected data.
4. Perform a transaction that uses the serial device.
5. Disconnect and reconnect the device and repeat the test command.

**Test Result(s):**
1. The serial peripheral device responds correctly to commands sent through the updated jssc library.
2. Reconnection after device disconnect works without requiring a launcher restart.

#### 4.3.JSON parsing and generation work with updated json and jackson-core

**Prerequisite(s):**
1. The POS launcher with updated json and jackson-core libraries is deployed.
2. The backend service is reachable and returns JSON responses.

**Step(s):**
1. Trigger a backend request that returns a JSON response.
2. Parse the JSON response using the json library and verify all fields are extracted correctly.
3. Generate a JSON request payload using jackson-core and send it to the backend.
4. Confirm that the backend accepts and processes the generated payload.

**Test Result(s):**
1. JSON responses are parsed correctly with no missing or malformed fields.
2. JSON request payloads generated by jackson-core are accepted by the backend without errors.

#### 4.4.AMQP messaging works with updated amqp-client

**Prerequisite(s):**
1. The POS launcher with the updated amqp-client library is deployed.
2. An AMQP broker (RabbitMQ) is configured and reachable from the terminal.

**Step(s):**
1. Start the POS launcher and verify the AMQP connection to the broker is established.
2. Subscribe to a test queue from the POS application.
3. Publish a test message to the queue from the backend or another client.
4. Verify that the POS application receives the published message.
5. Temporarily stop the broker, restart it, and confirm the launcher reconnects automatically.

**Test Result(s):**
1. The AMQP connection is established and messages are published and received correctly.
2. The launcher reconnects to the broker automatically after a broker restart.

#### 4.5.Phone number formatting works with updated libphonenumber

**Prerequisite(s):**
1. The POS launcher with the updated libphonenumber library is deployed.
2. Test phone numbers covering domestic and international formats are available.

**Step(s):**
1. Enter a domestic phone number in the POS application and verify the formatted output.
2. Enter an international phone number and verify the formatted output.
3. Enter an invalid phone number and confirm the validation error is displayed.
4. Verify that stored phone numbers use the E.164 format in local storage.

**Test Result(s):**
1. Domestic and international phone numbers are formatted correctly by the updated libphonenumber library.
2. Invalid phone numbers are rejected with an appropriate validation error.

#### 4.6.Groovy scripting works with updated groovy and groovy-jsr223

**Prerequisite(s):**
1. The POS launcher with updated groovy and groovy-jsr223 libraries is deployed.
2. Pre-configured Groovy scripts for business logic are present on the terminal.

**Step(s):**
1. Trigger a business flow that executes a pre-configured Groovy script.
2. Verify that the script output matches the expected result.
3. Execute a script through the JSR-223 scripting engine integration and confirm the result.
4. Trigger a script error scenario and confirm it is handled gracefully without crashing the launcher.

**Test Result(s):**
1. Groovy scripts execute correctly and produce the expected output with the updated libraries.
2. Script errors are handled gracefully without affecting the stability of the POS launcher.
