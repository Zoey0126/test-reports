# Libraries Upgrade 2026 Q4 IP2COM Test Report

## Functional Testing

### 1.IP2COM Service Startup And Library Verification

#### 1.1.Verify Service Starts Normally After Library Upgrade
**Prerequisite(s):**
1. IP2COM is deployed with the upgraded external libraries
2. The host machine and network environment are available

**Step(s):**
1. Start or restart the IP2COM service
2. Check the service status
3. Review the startup logs

**Test Result(s):**
1. The IP2COM service starts successfully without errors
2. No dependency conflict or startup failure is found in the logs

#### 1.2.Verify Upgraded Library Versions
**Prerequisite(s):**
1. IP2COM build with the upgraded libraries is deployed

**Step(s):**
1. Check the library versions in the deployment package or dependency manifest
2. Compare with the target versions

**Test Result(s):**
1. The upgraded libraries match the following target versions:

| Library | Target Version |
| --- | --- |
| pdfbox | 3.0.8 |
| json | 20260814 |
| log4j-api | 2.26.1 |
| log4j-core | 2.26.1 |

2. No old library version remains in the runtime classpath

### 2.Core Functionality Verification

#### 2.1.IP Device To COM Port Mapping
**Prerequisite(s):**
1. IP2COM service is running with the upgraded libraries
2. A network printer or IP device is available and reachable

**Step(s):**
1. Create or open the IP to COM port mapping for the target device
2. Check the mapped COM port status

**Test Result(s):**
1. The IP device is mapped to the virtual COM port successfully
2. The mapping status is stable and the connection is established

#### 2.2.Printing Through Mapped COM Port
**Prerequisite(s):**
1. An IP device is mapped to a virtual COM port by IP2COM
2. A printing application is configured to use the mapped COM port

**Step(s):**
1. Send a print job through the mapped COM port
2. Check the output at the IP device

**Test Result(s):**
1. The print job is transmitted through the mapped COM port successfully
2. The content is printed completely and correctly at the IP device

#### 2.3.PDF Processing With Upgraded Pdfbox
**Prerequisite(s):**
1. IP2COM is running with pdfbox 3.0.8
2. A task involving PDF processing is available

**Step(s):**
1. Trigger the task that processes PDF content
2. Check the processed output

**Test Result(s):**
1. The PDF is processed successfully with pdfbox 3.0.8 without errors
2. The processed content is complete and correct

### 3.Service Logging With Upgraded Log4j
**Prerequisite(s):**
1. IP2COM is running with log4j-api 2.26.1 and log4j-core 2.26.1

**Step(s):**
1. Perform several IP2COM operations to generate service logs
2. Check the log output and log file rotation

**Test Result(s):**
1. Service logs are written correctly with the expected level and format
2. No logging error or log loss is observed during operations
