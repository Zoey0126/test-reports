# HERO-67223 Test Report

## Functional Testing

### F1.Printing Service Launch and Connectivity After Java 25 Upgrade

#### F1.1.Verify Printing Service Starts Successfully With Java 25 Runtime

**Prerequisite(s):**
1. The new Printing Service build packaged with Java 25 runtime and Gradle 9.7.0 is available for installation
2. A clean Workstation (Windows 11 Enterprise) is available in the local test environment where the Printing Service is hosted
3. POS backend (Cloud / Local / OnPremise) is reachable from the workstation
4. A printer is configured and connected to the workstation

**Step(s):**
1. Install the new Printing Service build that uses Java 25 runtime and Gradle 9.7.0
2. Start the Printing Service
3. Verify the service startup process completes without errors
4. Check the service logs for any Java 25 / Gradle 9.7.0 related startup errors
5. Verify the Printing Service is reachable from the POS Client

**Test Result(s):**
1. The Printing Service installs successfully with the bundled Java 25 runtime and Gradle 9.7.0
2. The service starts without runtime errors or missing Java module errors
3. No Java 25 / Gradle 9.7.0 related startup errors appear in the service logs
4. The Printing Service is reachable from the POS Client

#### F1.2.Verify Core Printing Operations Function Correctly With Java 25 Runtime

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running
2. A printer is configured and connected to the workstation hosting the Printing Service
3. Print formats (kitchen slip, customer check, action slip, report) are configured in POS

**Step(s):**
1. From the POS Client, trigger a kitchen slip print by adding items to a check
2. Trigger a customer check print
3. Trigger an action slip print (e.g. mark delivery, void item)
4. Trigger a report print from the POS Client
5. Verify all print outputs render correctly on the connected printer

**Test Result(s):**
1. Kitchen slip prints correctly through the Printing Service with Java 25 runtime
2. Customer check prints correctly through the Printing Service
3. Action slip prints correctly through the Printing Service
4. Report printing functions correctly through the Printing Service
5. No print spooler errors or missing print service errors occur

### F2.Print Format and Layout Verification

#### F2.1.Verify Print Formats Render Correctly After Java 25 Upgrade

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running
2. Multiple print formats are configured in POS (e.g. kitchen slip with item grouping, check with item/discount/payment sections, action slip with rollback info)
3. A printer is connected and accessible from the Printing Service

**Step(s):**
1. Trigger prints using different configured print formats (kitchen slip, customer check, action slip, receipt)
2. Verify the layout of each printed output
3. Verify the content of each printed output (item names, prices, discounts, payments, footer)
4. Verify multi-language support (e.g. English, Chinese, Thai) is not affected by the Java 25 upgrade

**Test Result(s):**
1. All configured print formats render correctly through the Printing Service with Java 25 runtime
2. The layout (positions, alignment, fonts) of each printed output is correct and matches the previous Java runtime behavior
3. The content (item names, prices, discounts, payments, footer) is correctly printed
4. Multi-language support is preserved after the Java 25 upgrade

### F3.Deprecated Code Rewrite Verification

#### F3.1.Verify Refactored Code Paths Function Equivalently After Java 25 Upgrade

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running (with deprecated code rewritten)
2. Print formats and printer configurations are in place
3. A POS Client is available to trigger print actions

**Step(s):**
1. Identify print operations that historically relied on deprecated Java APIs (e.g. legacy print service APIs, deprecated image/font handling)
2. Trigger prints that exercise these refactored code paths
3. Verify the printed output matches the expected format and content
4. Compare the printed output with the previous Java runtime version output to confirm equivalence

**Test Result(s):**
1. The rewritten code paths produce the same printed output as the previous Java runtime version
2. No functionality regression is introduced by the deprecated code rewrite
3. The refactored code runs without exceptions or warnings under Java 25
4. All print formats that previously worked continue to work correctly

### F4.System Stability and Resource Usage

#### F4.1.Verify Printing Service Stability Under Java 25 Runtime

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running
2. The workstation has monitoring tools available (e.g. Task Manager, JVM monitoring) to observe resource usage
3. A high-volume print scenario can be simulated (e.g. multiple concurrent print requests)

**Step(s):**
1. Start the Printing Service
2. Trigger a sequence of print operations (kitchen slip, customer check, action slip, report) for an extended period
3. Trigger concurrent print requests to simulate a high-volume scenario
4. Observe memory and CPU usage at intervals during operation
5. Verify no crashes, hangs, or memory leaks occur over time

**Test Result(s):**
1. Memory usage remains stable during prolonged operation under Java 25 runtime
2. CPU usage is comparable to or better than the previous Java runtime
3. No crashes or hangs occur during extended operation or under concurrent print load
4. No memory leaks or growing heap usage patterns are observed

## Compatibility Testing

### C1.Windows Operating System Compatibility

#### C1.1.Verify Printing Service With Java 25 on Windows 11 Enterprise

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is available for installation
2. A Workstation running Windows 11 Enterprise is available in the local test environment

**Step(s):**
1. Install the Printing Service with Java 25 runtime on Windows 11 Enterprise
2. Start the service and verify connectivity from the POS Client
3. Perform representative print operations (kitchen slip, check, action slip, report)
4. Verify all functions operate correctly on Windows 11 Enterprise

**Test Result(s):**
1. The Printing Service installs and starts successfully on Windows 11 Enterprise
2. The service is reachable from the POS Client on Windows 11 Enterprise
3. All representative print operations function correctly on Windows 11 Enterprise
4. No Windows-specific compatibility errors are observed

### C2.Network Compatibility

#### C2.1.Verify Printing Service With Java 25 Across Network Configurations

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running
2. POS backend and POS Client are reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations

**Step(s):**
1. Connect the workstation to the Cloud network and verify Printing Service connectivity and operations
2. Repeat the test under Local, OnPremise, Wireless and Wired networks
3. Verify each network configuration allows normal Printing Service operations

**Test Result(s):**
1. The Printing Service with Java 25 runtime operates correctly across all network configurations
2. Print operations triggered from the POS Client function correctly in each network configuration
3. No network-related compatibility issues are introduced by the Java 25 upgrade

### C3.HTTP/HTTPS Protocol Compatibility

#### C3.1.Verify HTTP and HTTPS Communication Works With Java 25 Runtime

**Prerequisite(s):**
1. The Printing Service with Java 25 runtime is installed and running
2. Backend services accessible via both HTTP and HTTPS protocols are available

**Step(s):**
1. Configure the Printing Service to communicate with backend services via HTTP
2. Trigger print operations and verify HTTP-based communication works
3. Switch the configuration to HTTPS
4. Trigger print operations and verify HTTPS-based communication works

**Test Result(s):**
1. HTTP communication between the Printing Service and backend services works correctly with Java 25 runtime
2. HTTPS communication works correctly with proper certificate validation
3. No SSL/TLS handshake failures or protocol errors occur under Java 25

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS Printing Service
- **Issue Type**: Improvement
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **Test Protocols**: HTTP, HTTPS
- **Built-in Browser**: N/A (Printing Service)
- **QC Tester**: Ryan Xie (ryan.xie@shijigroup.com)
- **QC Reviewer**: Eric Fang (eric.fang@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-67223)

Upgrade Java version to 25 - Printing Service

Release Notes:

- System upgrades to Java 25
- Gradle version upgrades to 9.7.0
- Rewrite deprecated code
- Setup: None

After the upgrade:

- The Printing Service must install and start successfully on supported Windows operating systems
- All core print operations (kitchen slip, customer check, action slip, report) must continue to work correctly
- The rewritten deprecated code paths must produce equivalent printed output to the previous Java runtime version
- HTTP and HTTPS communication with backend services must continue to function correctly
- The system must remain stable under typical workloads with no crashes, hangs or memory leaks
