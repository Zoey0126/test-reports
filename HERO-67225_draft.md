# HERO-67225 Test Report

## Functional Testing

### F1.POS Windows Client Launch and Basic Operations After Java 25 Upgrade

#### F1.1.Verify POS Windows Client Launches Successfully With Java 25 Runtime

**Prerequisite(s):**
1. The new POS Windows Client build packaged with Java 25 runtime is available for installation
2. A clean Workstation (Windows 11 Enterprise) is available in the local test environment
3. POS backend (Cloud / Local / OnPremise) is reachable from the workstation
4. A valid user account exists in the system

**Step(s):**
1. Install the new POS Windows Client build that uses Java 25 runtime on the workstation
2. Launch the POS Client from the desktop shortcut or start menu
3. Verify the application startup process completes without errors
4. Log in with a valid user account
5. Observe the main POS screen rendering and navigation

**Test Result(s):**
1. The POS Windows Client installs successfully with the bundled Java 25 runtime
2. The application launches without runtime errors or missing Java module errors
3. The login screen renders correctly and the user is able to log in
4. The main POS screen renders all UI elements correctly (buttons, panels, navigation)

#### F1.2.Verify Core POS Operations Function Correctly With Java 25 Runtime

**Prerequisite(s):**
1. The POS Windows Client with Java 25 runtime is installed and launched successfully
2. User is logged in with valid credentials
3. At least one menu item, table, and payment method are configured in the system

**Step(s):**
1. Open a new check at a table
2. Add menu items to the check
3. Apply a discount if applicable
4. Print the check
5. Go to the cashier screen and complete a payment
6. Close the check
7. Verify reports (e.g. Check Listing, Sales Total) can be generated from the workstation

**Test Result(s):**
1. New check opens and table assignment works correctly
2. Menu items are added to the check without errors
3. Discount application works as expected
4. Check printing works correctly with Java 25 print service
5. Payment processing at the cashier screen completes successfully
6. Check closure functions correctly
7. Reports can be generated from the workstation without errors

### F2.Java 25 Specific Features and Behaviors

#### F2.1.Verify Built-in Browser (JavaFX) Functionality With Java 25

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is installed and running
2. JavaFX is bundled with the Java 25 runtime distribution
3. Functions that rely on the built-in browser (e.g. Report, Review Check/Receipt) are accessible

**Step(s):**
1. Open the Report module from the POS Windows Client
2. Generate a report that uses the built-in JavaFX browser to render
3. Verify the report content displays correctly
4. Open the Review Check/Receipt function that uses the built-in browser
5. Verify the content renders correctly

**Test Result(s):**
1. The JavaFX built-in browser initializes correctly with Java 25 runtime
2. Reports render correctly in the built-in browser
3. Review Check/Receipt functions render correctly in the built-in browser
4. No JavaFX module loading errors or compatibility warnings appear

#### F2.2.Verify Print Service Works With Java 25 Runtime

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is installed and running
2. A printer is configured and connected to the workstation
3. Print formats (e.g. kitchen slip, check, action slip) are configured in POS

**Step(s):**
1. Open a check and add items to trigger kitchen slip printing
2. Print the customer check
3. Trigger an action slip print (e.g. mark delivery)
4. Trigger a report print
5. Verify all print outputs render correctly on the printer

**Test Result(s):**
1. Kitchen slip prints correctly with Java 25 print service
2. Customer check prints correctly
3. Action slip prints correctly
4. Report printing functions correctly with the new Java runtime
5. No print spooler errors or missing print service errors occur

### F3.System Stability and Resource Usage

#### F3.1.Verify POS Windows Client Stability Under Java 25 Runtime

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is installed and running
2. The workstation has monitoring tools available (e.g. Task Manager) to observe resource usage
3. POS backend is reachable and a normal day's worth of operations can be simulated

**Step(s):**
1. Launch the POS Windows Client
2. Perform a sequence of typical operations (open checks, add items, apply payments, generate reports) for an extended period
3. Observe memory and CPU usage at intervals during operation
4. Verify no crashes, hangs, or memory leaks occur over time

**Test Result(s):**
1. Memory usage remains stable during prolonged operation under Java 25 runtime
2. CPU usage is comparable to or better than the previous Java runtime
3. No crashes or hangs occur during extended operation
4. No memory leaks or growing heap usage patterns are observed

## Compatibility Testing

### C1.Windows Operating System Compatibility

#### C1.1.Verify POS Windows Client With Java 25 on Windows 11 Enterprise

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is available for installation
2. Workstations running Windows 11 Enterprise are available in the local test environment

**Step(s):**
1. Install the POS Windows Client with Java 25 runtime on Windows 11 Enterprise
2. Launch the application
3. Perform representative operations (login, open check, payment, report generation)
4. Verify all functions operate correctly on Windows 11 Enterprise

**Test Result(s):**
1. The POS Windows Client installs and launches successfully on Windows 11 Enterprise
2. All representative operations function correctly on Windows 11 Enterprise
3. No Windows-specific compatibility errors are observed

### C2.Network Compatibility

#### C2.1.Verify POS Windows Client With Java 25 Across Network Configurations

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is installed and running
2. POS backend is reachable across Cloud, Local, OnPremise, Wireless and Wired network configurations

**Step(s):**
1. Connect the workstation to the Cloud network and verify POS Client connectivity and operations
2. Repeat the test under Local, OnPremise, Wireless and Wired networks
3. Verify each network configuration allows normal POS Client operations

**Test Result(s):**
1. The POS Windows Client with Java 25 runtime connects to the backend successfully across all network configurations
2. All operations (login, check operations, payment, reports) function correctly in each network configuration
3. No network-related compatibility issues are introduced by the Java 25 upgrade

### C3.HTTP/HTTPS Protocol Compatibility

#### C3.1.Verify HTTP and HTTPS Communication Works With Java 25 Runtime

**Prerequisite(s):**
1. POS Windows Client with Java 25 runtime is installed and running
2. Backend services accessible via both HTTP and HTTPS protocols are available

**Step(s):**
1. Configure the POS Windows Client to communicate with backend services via HTTP
2. Perform login and core POS operations and verify HTTP-based communication works
3. Switch the configuration to HTTPS
4. Perform login and core POS operations and verify HTTPS-based communication works

**Test Result(s):**
1. HTTP communication between the POS Windows Client and backend services works correctly with Java 25 runtime
2. HTTPS communication works correctly with proper certificate validation
3. No SSL/TLS handshake failures or protocol errors occur under Java 25

## Test Environment Information

- **Test Environment**: Local test environment, HQ test environment, ER test environment
- **Test Devices**: Workstation (Windows 11 Enterprise)
- **Component**: POS Windows Client
- **Issue Type**: Improvement
- **Test Network**: Cloud, Local, OnPremise, Wireless, Wired
- **Test Protocols**: HTTP, HTTPS
- **Built-in Browser**: JavaFX
- **QC Tester**: Ryan Xie (ryan.xie@shijigroup.com)
- **QC Reviewer**: Eric Fang (eric.fang@shijigroup.com)

## Appendix

### Acceptance Criteria (from JIRA HERO-67225)

Upgrade Java version to 25 - Windows Client

The POS Windows Client must be upgraded to use the Java 25 runtime. After the upgrade:

- The POS Windows Client must install and launch successfully on supported Windows operating systems
- All core POS functions (login, check operations, payments, reports, printing, built-in browser) must continue to work correctly
- HTTP and HTTPS communication with backend services must continue to function correctly
- The system must remain stable under typical workloads with no crashes, hangs or memory leaks
