# Update external libraries of printing service in 2026 Q4 Test Report

## Functional Testing

### 1.Printing Service Startup And Library Verification

#### 1.1.Verify Service Starts Normally After Library Upgrade
**Prerequisite(s):**
1. Printing service is deployed with the upgraded external libraries
2. POS environment is running normally

**Step(s):**
1. Start or restart the printing service
2. Check the service status
3. Review the startup logs

**Test Result(s):**
1. The printing service starts successfully without errors
2. No class loading error, dependency conflict or startup failure is found in the logs

#### 1.2.Verify Upgraded Library Versions
**Prerequisite(s):**
1. Printing service build with the upgraded libraries is deployed

**Step(s):**
1. Check the library versions in the deployment package or dependency manifest
2. Compare with the target versions

**Test Result(s):**
1. The upgraded libraries match the following target versions:

| Library | Target Version |
| --- | --- |
| json | 20260814 |
| commons-logging | 1.4.0 |
| pdfbox | 3.0.8 |
| fontbox | 3.0.8 |
| log4j-api | 2.26.1 |
| log4j-core | 2.26.1 |
| amqp-client | 5.35.0 |

2. No old library version remains in the runtime classpath

### 2.Printing Functionality Verification

#### 2.1.Receipt Printing
**Prerequisite(s):**
1. Printing service is running with the upgraded libraries
2. A printer is configured and connected
3. A check is available for printing

**Step(s):**
1. Perform a receipt printing action on the POS
2. Check the printed receipt

**Test Result(s):**
1. The receipt is printed successfully with complete and correct content
2. Layout, alignment and special characters are rendered correctly

#### 2.2.Kitchen Order Printing
**Prerequisite(s):**
1. Printing service is running with the upgraded libraries
2. A kitchen printer is configured for the target outlet

**Step(s):**
1. Place an order with items on the POS
2. Check the kitchen order ticket printed at the kitchen printer

**Test Result(s):**
1. The kitchen order ticket is printed automatically after ordering
2. Item names, quantities and modifiers are printed correctly

#### 2.3.PDF Based Printing And Font Rendering
**Prerequisite(s):**
1. Printing service is running with pdfbox 3.0.8 and fontbox 3.0.8
2. A print job that generates PDF output is available

**Step(s):**
1. Trigger a print job that goes through the PDF generation path
2. Check the generated output and the final printed result

**Test Result(s):**
1. The PDF is generated successfully with pdfbox 3.0.8 without errors
2. Fonts are embedded and rendered correctly with fontbox 3.0.8, including multilingual characters

### 3.Message And Data Handling

#### 3.1.Print Job Consumption Via AMQP Message Queue
**Prerequisite(s):**
1. Printing service is running with amqp-client 5.35.0
2. The message queue broker is reachable

**Step(s):**
1. Send a print job request to the message queue
2. Observe the printing service consumption behavior

**Test Result(s):**
1. The printing service connects to the message queue and consumes the print job normally
2. The print output is produced without message loss or connection error

#### 3.2.JSON Parsing For Print Job Data
**Prerequisite(s):**
1. Printing service is running with the upgraded json library

**Step(s):**
1. Send a print job with JSON payload containing normal, multilingual and special characters
2. Check the parsed data and the print output

**Test Result(s):**
1. The JSON payload is parsed correctly without parsing errors
2. All fields including special and multilingual characters are printed as expected

### 4.Service Logging With Upgraded Log4j
**Prerequisite(s):**
1. Printing service is running with log4j-api 2.26.1 and log4j-core 2.26.1

**Step(s):**
1. Perform several print operations to generate service logs
2. Check the log output and log file rotation

**Test Result(s):**
1. Service logs are written correctly with the expected level and format
2. No logging error or log loss is observed during print operations
