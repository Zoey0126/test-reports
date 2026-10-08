# HERO-69077 Test Report

## Functional Testing

### F1.Terminate Threads Replacement For Reset App And Server

#### F1.1.Verify Reset App And Server Terminates Client Thread Using The New Java 25 Handling

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running Windows 11 Enterprise with the upgraded Java 25 runtime
2. The POS client is able to start and connect to the local server successfully (cloud / local network)
3. The POS client is in a normal operating state with at least one active ordering session / open thread before the reset is triggered
4. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Open an ordering session and confirm the client thread is running (for example via the Windows task manager / process monitor)
3. Trigger the "Reset app and server" function from the POS client
4. Observe the client thread termination behaviour
5. Verify the client app and server are reset as expected (client restarts and re-establishes connection to the local server)

**Test Result(s):**
1. The "Reset app and server" function completes without throwing the deprecated thread-termination exception that was raised on Java 25
2. The client thread is terminated via the new Java 25 handling, and the client app and server are reset successfully
3. The POS client restarts and reconnects to the local server without manual intervention
4. No `UnsupportedOperation` / `Thread.terminate` deprecation runtime errors are shown in the client or server logs

#### F1.2.Verify No Deprecated Thread Termination Call Remains After Java 25 Upgrade

**Prerequisite(s):**
1. The fixed POS client build with the upgraded Java 25 runtime and Gradle 9.7.0 is deployed on a Workstation
2. Access to the local server logs and POS client logs
3. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Perform a sequence of normal POS operations (open outlet, open check, order items, close check, daily start / daily close)
3. Trigger the "Reset app and server" function multiple times during the session
4. Inspect the POS client and local server logs for any deprecated thread-termination related stack traces or warnings

**Test Result(s):**
1. All normal POS operations complete successfully without any deprecated thread-termination related errors in the logs
2. Multiple "Reset app and server" operations complete without throwing the deprecated terminate-threads exception
3. The upgraded Java 25 / Gradle 9.7.0 build starts up and operates without any deprecated-code runtime errors

### F2.Regression — Existing Reset And Recovery Flows

#### F2.1.Verify Existing Reset App And Server Recovery Behaviour Is Preserved

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running the upgraded Java 25 runtime
2. The POS client is in a normal operating state with active ordering / payment data
3. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Open an outlet, perform daily start, open a check, order items and apply a payment
3. Trigger the "Reset app and server" function while there is unsaved / in-flight data
4. Confirm the recovery flow after reset (the client restarts and the in-flight data is handled per existing recovery rules)
5. Re-open the POS client after reset and verify the previously closed checks are intact

**Test Result(s):**
1. The "Reset app and server" function completes without error and the recovery behaviour matches the pre-upgrade (Java 17) flow
2. In-flight data is handled per the existing recovery rules — no data loss or corruption is introduced by the new thread-termination handling
3. Closed checks and historical data remain intact after the reset and client restart
4. The new Java 25 handling produces the same end-user experience as the previous Java 17 implementation

## Test Environment Information

- Local test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69077):**

- Terminate threads feature still compiles on Java 25, but it no longer stops a thread. Since JDK 20 the method always throws with an exception. Any remaining call that worked on Java 17 will fail at runtime on Java 25.
- When the user performs "Reset app and server" in the client, the system must use the new handling to replace the existing deprecated function for terminating the client thread.
- The upgraded Java 25 / Gradle 9.7.0 build must start up and operate without any deprecated-code runtime errors.
