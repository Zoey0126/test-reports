# HERO-67222 Test Report

## Functional Testing

### F1.Printing Parser — Java 25 Upgrade Verification

#### F1.1.Verify Printing Parser Builds And Starts Under Java 25 And Gradle 9.7.0

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running the upgraded Java 25 runtime
2. The build is compiled with Gradle 9.7.0 and any deprecated printing-parser code has been rewritten
3. A printer device is configured in Printing Management > Printer Setup > Printer Devices (name, short name, printer model, address, port, connection settings) so the printing parser has a target printer to address
4. Test environment: Local test environment

**Step(s):**
1. Launch the POS client under the upgraded Java 25 runtime
2. Confirm the printing parser module initializes successfully during client startup (no deprecated-code runtime errors)
3. Open the Summary of Printer Devices Report (AUDIT0003) and verify the configured printer device info is read correctly by the upgraded printing parser
4. Inspect the POS client and local server logs for any printing-parser-related errors or deprecated stack traces

**Test Result(s):**
1. The POS client starts up successfully under the upgraded Java 25 runtime with Gradle 9.7.0
2. The printing parser module initializes without throwing deprecated-code runtime errors
3. The Summary of Printer Devices Report (AUDIT0003) shows the configured printer device info (name, short name, printer model, address, status, config zone, printer type, port no., connection settings) correctly
4. No printing-parser-related errors or deprecated stack traces are observed in the logs

### F2.Printing Parser — Print Operations

#### F2.1.Verify Standard Print Jobs Work Under The Upgraded Printing Parser

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running the upgraded Java 25 runtime
2. At least one printer device is configured and online in Printing Management > Printer Setup > Printer Devices
3. A closed guest check with item, payment, service charge, tax and discount data exists so that a receipt can be printed
4. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Open an outlet, perform daily start, open a check, order items, apply a payment and close the check
3. Trigger the print function for the closed check (print receipt / guest check)
4. Verify the printout content (check number, business day, items, payments, service charges, taxes, discounts, totals, footer fields) is parsed and rendered correctly
5. Repeat the print job for different check types (revenue / non-revenue / advance order) and discount scenarios

**Test Result(s):**
1. The print job for the closed check is processed by the upgraded printing parser and the receipt is printed correctly
2. The printout content matches the source check data (items, payments, service charges, taxes, discounts and totals are parsed and rendered correctly)
3. Print jobs for different check types and discount scenarios all produce correct printouts
4. No printing-parser-related errors are observed in the logs during the print jobs

#### F2.2.Verify Taiwan GUI Configuration Print Output Under The Upgraded Printing Parser

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running the upgraded Java 25 runtime
2. A Taiwan GUI configuration is set up (prefix, code range, date range) for the target outlet / station
3. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Open the Taiwan GUI Configuration Report (POS0291) for the target outlet / station
3. Verify the report shows the Taiwan GUI configuration (prefix, code range, date range) correctly
4. Trigger any Taiwan GUI related print job and verify the printout is parsed and rendered correctly by the upgraded printing parser

**Test Result(s):**
1. The Taiwan GUI Configuration Report (POS0291) shows the configured Taiwan GUI info (outlet / station, selected date range, date range, prefix, code range) correctly under the upgraded printing parser
2. The Taiwan GUI related print job is processed correctly by the upgraded printing parser
3. No printing-parser-related errors are observed in the logs

### F3.Regression — Existing Reporting And Audit Print Flows

#### F3.1.Verify Reporting And Audit Print Flows Are Preserved After Java 25 Upgrade

**Prerequisite(s):**
1. The fixed POS client build is deployed on a Workstation running the upgraded Java 25 runtime
2. At least one printer device is configured and online
3. Test environment: Local test environment

**Step(s):**
1. Launch the POS client and log in with a valid operator account
2. Open several standard reports (for example Exception Report — CUS0089, API Call Report — CUS0222, Discount Approval Process Report — POS0195) and trigger the print function for each
3. Verify each report printout is rendered correctly by the upgraded printing parser (report title, footer markers, printed at / printed by, report code, page number)
4. Inspect the POS client and local server logs for any printing-parser-related errors

**Test Result(s):**
1. All standard reports (Exception Report, API Call Report, Discount Approval Process Report) print correctly under the upgraded printing parser
2. The report printouts include the correct report title, footer markers, printed at / printed by, report code and page number
3. No printing-parser-related errors or deprecated stack traces are observed in the logs after the Java 25 / Gradle 9.7.0 upgrade

## Test Environment Information

- Local test environment

## Appendix

**JIRA Acceptance Criteria (HERO-67222):**

- Upgrade Java version to 25 — Printing Parser.
- System upgrades to Java 25; Gradle version upgrades to 9.7.0; rewrite deprecated code.
- The upgraded printing parser must build, start and operate correctly under Java 25 / Gradle 9.7.0 without deprecated-code runtime errors.
- Existing print, reporting and audit print flows must continue to work as before the upgrade.
