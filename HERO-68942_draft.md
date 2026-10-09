# HERO-68942 - HKJC - TMS - Solace tool for export file support .cnt and .pos files

## Test Environment Information
- Local Test Environment
- HQ Test Environment
- HKJC Test Environment
- Solace Messaging Infrastructure configured and accessible
- TMS Export File Path directory configured

## Functional Testing

### F1. Solace Tool - .cnt File Transfer Verification

#### F1.1. Verify .cnt File Detection and Transfer via Solace
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. At least one .cnt export file exists in the configured File Path directory
5. File name pattern is configured to match the .cnt file

**Step(s):**
1. Place a valid .cnt export file in the configured File Path directory
2. Ensure the file name matches the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects the .cnt file
5. Verify the .cnt file is transferred via Solace messaging
6. Verify the .cnt file is received at the destination endpoint
7. Verify the transferred file content is intact and matches the source file

**Test Result(s):**
1. The Solace tool successfully detects the .cnt file in the configured File Path directory
2. The .cnt file is transferred via Solace messaging infrastructure
3. The .cnt file is received at the destination endpoint with correct content
4. The transferred file content matches the source file exactly

#### F1.2. Verify .cnt File Transfer with Multiple Files
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Multiple .cnt export files exist in the configured File Path directory
5. File name pattern is configured to match the .cnt files

**Step(s):**
1. Place multiple .cnt export files with different names in the configured File Path directory
2. Ensure all file names match the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects all .cnt files
5. Verify all .cnt files are transferred via Solace messaging
6. Verify all .cnt files are received at the destination endpoint
7. Verify the order and content of transferred files

**Test Result(s):**
1. The Solace tool successfully detects all .cnt files in the configured File Path directory
2. All .cnt files are transferred via Solace messaging infrastructure
3. All .cnt files are received at the destination endpoint with correct content
4. No files are missed or duplicated during the transfer process

### F2. Solace Tool - .pos File Transfer Verification

#### F2.1. Verify .pos File Detection and Transfer via Solace
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. At least one .pos export file exists in the configured File Path directory
5. File name pattern is configured to match the .pos file

**Step(s):**
1. Place a valid .pos export file in the configured File Path directory
2. Ensure the file name matches the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects the .pos file
5. Verify the .pos file is transferred via Solace messaging
6. Verify the .pos file is received at the destination endpoint
7. Verify the transferred file content is intact and matches the source file

**Test Result(s):**
1. The Solace tool successfully detects the .pos file in the configured File Path directory
2. The .pos file is transferred via Solace messaging infrastructure
3. The .pos file is received at the destination endpoint with correct content
4. The transferred file content matches the source file exactly

#### F2.2. Verify .pos File Transfer with Multiple Files
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Multiple .pos export files exist in the configured File Path directory
5. File name pattern is configured to match the .pos files

**Step(s):**
1. Place multiple .pos export files with different names in the configured File Path directory
2. Ensure all file names match the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects all .pos files
5. Verify all .pos files are transferred via Solace messaging
6. Verify all .pos files are received at the destination endpoint
7. Verify the order and content of transferred files

**Test Result(s):**
1. The Solace tool successfully detects all .pos files in the configured File Path directory
2. All .pos files are transferred via Solace messaging infrastructure
3. All .pos files are received at the destination endpoint with correct content
4. No files are missed or duplicated during the transfer process

### F3. Solace Tool - .zip File Transfer Regression Verification

#### F3.1. Verify .zip File Transfer Still Works (Regression)
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. At least one .zip export file exists in the configured File Path directory
5. File name pattern is configured to match the .zip file

**Step(s):**
1. Place a valid .zip export file in the configured File Path directory
2. Ensure the file name matches the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects the .zip file
5. Verify the .zip file is transferred via Solace messaging
6. Verify the .zip file is received at the destination endpoint
7. Verify the transferred file content is intact and matches the source file

**Test Result(s):**
1. The Solace tool successfully detects the .zip file in the configured File Path directory
2. The .zip file is transferred via Solace messaging infrastructure (existing functionality maintained)
3. The .zip file is received at the destination endpoint with correct content
4. The transferred file content matches the source file exactly

### F4. Solace Tool - Mixed File Type Transfer Verification

#### F4.1. Verify Mixed .cnt, .pos, and .zip File Transfer
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. A mix of .cnt, .pos, and .zip export files exist in the configured File Path directory
5. File name pattern is configured to match all three file types

**Step(s):**
1. Place a combination of .cnt, .pos, and .zip export files in the configured File Path directory
2. Ensure all file names match the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the Solace tool detects all three file types
5. Verify all files (.cnt, .pos, .zip) are transferred via Solace messaging
6. Verify all files are received at the destination endpoint
7. Verify each file type is transferred with correct content

**Test Result(s):**
1. The Solace tool successfully detects all .cnt, .pos, and .zip files in the configured File Path directory
2. All three file types are transferred via Solace messaging infrastructure
3. All files are received at the destination endpoint with correct content
4. Each file type is correctly identified and transferred without confusion

### F5. Solace Tool - File Type Filtering Verification

#### F5.1. Verify Only .cnt, .pos, and .zip Files Are Transferred
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Files of various types (.cnt, .pos, .zip, .csv, .txt, .xml, .pdf, .xlsx) exist in the configured File Path directory
5. File name pattern is configured

**Step(s):**
1. Place files of multiple types (.cnt, .pos, .zip, .csv, .txt, .xml, .pdf, .xlsx) in the configured File Path directory
2. Ensure file names match the configured File name pattern for supported types
3. Trigger or wait for the Solace tool to scan the directory
4. Observe which files the Solace tool detects
5. Verify only .cnt, .pos, and .zip files are transferred
6. Verify unsupported file types (.csv, .txt, .xml, .pdf, .xlsx) are not transferred

**Test Result(s):**
1. The Solace tool only detects .cnt, .pos, and .zip files in the configured File Path directory
2. Only .cnt, .pos, and .zip files are transferred via Solace messaging
3. Unsupported file types (.csv, .txt, .xml, .pdf, .xlsx, etc.) are not detected or transferred
4. The system correctly filters file types to the three supported types only

#### F5.2. Verify Empty Directory Handling
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. The configured File Path directory is empty (no files present)

**Step(s):**
1. Ensure the configured File Path directory is empty
2. Trigger or wait for the Solace tool to scan the directory
3. Observe the Solace tool behavior with an empty directory
4. Verify no errors are thrown
5. Verify the system handles the empty directory gracefully

**Test Result(s):**
1. The Solace tool scans the empty directory without errors
2. No files are transferred (as expected with an empty directory)
3. The system handles the empty directory gracefully with appropriate logging or status messages
4. No system crash or exception occurs

### F6. Solace Tool - File Name Pattern Matching Verification

#### F6.1. Verify File Name Pattern Matching
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Files with names matching and not matching the configured File name pattern exist in the directory

**Step(s):**
1. Place .cnt, .pos, and .zip files with names that match the configured File name pattern in the directory
2. Place .cnt, .pos, and .zip files with names that do NOT match the configured File name pattern in the directory
3. Trigger or wait for the Solace tool to scan the directory
4. Observe which files the Solace tool detects
5. Verify only files matching the File name pattern are transferred
6. Verify files not matching the pattern are not transferred

**Test Result(s):**
1. The Solace tool only detects files whose names match the configured File name pattern
2. Only pattern-matching .cnt, .pos, and .zip files are transferred via Solace messaging
3. Files that do not match the File name pattern are not transferred, even if they have supported extensions
4. The File name pattern matching works correctly in combination with file type filtering

#### F6.2. Verify File Name Pattern with Wildcards
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. File name pattern is configured with wildcard characters (e.g., * or ?)

**Step(s):**
1. Configure the File name pattern with wildcard characters
2. Place .cnt, .pos, and .zip files with names that match the wildcard pattern in the directory
3. Place .cnt, .pos, and .zip files with names that do NOT match the wildcard pattern in the directory
4. Trigger or wait for the Solace tool to scan the directory
5. Verify only files matching the wildcard pattern are transferred

**Test Result(s):**
1. The Solace tool correctly interprets wildcard characters in the File name pattern
2. Only files matching the wildcard pattern are transferred via Solace messaging
3. Files that do not match the wildcard pattern are not transferred
4. Wildcard pattern matching works correctly for all three supported file types

### F7. Solace Tool - Error Handling and Edge Cases

#### F7.1. Verify Invalid File Path Handling
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. An invalid or non-existent File Path is configured

**Step(s):**
1. Configure an invalid or non-existent File Path directory
2. Trigger or wait for the Solace tool to scan the directory
3. Observe the Solace tool behavior with an invalid path
4. Verify appropriate error handling or error message is displayed
5. Verify no system crash occurs

**Test Result(s):**
1. The Solace tool handles the invalid File Path gracefully
2. An appropriate error message or log entry is generated indicating the path is invalid
3. No system crash or unhandled exception occurs
4. The system continues to operate without requiring a restart

#### F7.2. Verify Large File Transfer
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Large .cnt, .pos, and .zip files (exceeding normal size) exist in the directory

**Step(s):**
1. Place large .cnt, .pos, and .zip files in the configured File Path directory
2. Ensure file names match the configured File name pattern
3. Trigger or wait for the Solace tool to scan the directory
4. Observe the transfer process for large files
5. Verify all large files are transferred successfully
6. Verify the transferred file content is intact

**Test Result(s):**
1. The Solace tool successfully detects and transfers large .cnt, .pos, and .zip files
2. All large files are received at the destination endpoint with correct content
3. The transferred file content matches the source file exactly
4. No data corruption or truncation occurs during large file transfer

#### F7.3. Verify Concurrent File Transfer
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured and running
3. The File Path directory is set and accessible
4. Multiple .cnt, .pos, and .zip files are being generated simultaneously by the export process

**Step(s):**
1. Trigger the export process to generate .cnt, .pos, and .zip files simultaneously
2. While files are being generated, trigger or wait for the Solace tool to scan the directory
3. Observe the Solace tool behavior with concurrently generated files
4. Verify all completely written files are transferred
5. Verify partially written files are not transferred (or handled appropriately)

**Test Result(s):**
1. The Solace tool handles concurrent file generation gracefully
2. Completely written .cnt, .pos, and .zip files are transferred successfully
3. Partially written or locked files are not transferred or are retried appropriately
4. No data corruption or race conditions occur during concurrent file transfer

## Compatibility Testing

### C1. Network Environment Compatibility

#### C1.1. Verify Solace Tool File Transfer on Cloud Network
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured on Cloud network
3. The File Path directory is set and accessible
4. .cnt, .pos, and .zip export files exist in the directory

**Step(s):**
1. Configure the Solace tool to operate on Cloud network
2. Place .cnt, .pos, and .zip files in the configured File Path directory
3. Trigger the Solace tool to transfer files
4. Verify all three file types are transferred successfully on Cloud network
5. Verify file integrity at the destination

**Test Result(s):**
1. The Solace tool successfully transfers .cnt, .pos, and .zip files on Cloud network
2. All file types are received at the destination with correct content
3. No network-related errors occur during transfer on Cloud

#### C1.2. Verify Solace Tool File Transfer on Local Network
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured on Local network
3. The File Path directory is set and accessible
4. .cnt, .pos, and .zip export files exist in the directory

**Step(s):**
1. Configure the Solace tool to operate on Local network
2. Place .cnt, .pos, and .zip files in the configured File Path directory
3. Trigger the Solace tool to transfer files
4. Verify all three file types are transferred successfully on Local network
5. Verify file integrity at the destination

**Test Result(s):**
1. The Solace tool successfully transfers .cnt, .pos, and .zip files on Local network
2. All file types are received at the destination with correct content
3. No network-related errors occur during transfer on Local network

#### C1.3. Verify Solace Tool File Transfer on OnPremise Network
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured on OnPremise network
3. The File Path directory is set and accessible
4. .cnt, .pos, and .zip export files exist in the directory

**Step(s):**
1. Configure the Solace tool to operate on OnPremise network
2. Place .cnt, .pos, and .zip files in the configured File Path directory
3. Trigger the Solace tool to transfer files
4. Verify all three file types are transferred successfully on OnPremise network
5. Verify file integrity at the destination

**Test Result(s):**
1. The Solace tool successfully transfers .cnt, .pos, and .zip files on OnPremise network
2. All file types are received at the destination with correct content
3. No network-related errors occur during transfer on OnPremise network

#### C1.4. Verify Solace Tool File Transfer on Wired Network
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured on Wired network
3. The File Path directory is set and accessible
4. .cnt, .pos, and .zip export files exist in the directory

**Step(s):**
1. Configure the Solace tool to operate on Wired network
2. Place .cnt, .pos, and .zip files in the configured File Path directory
3. Trigger the Solace tool to transfer files
4. Verify all three file types are transferred successfully on Wired network
5. Verify file integrity at the destination

**Test Result(s):**
1. The Solace tool successfully transfers .cnt, .pos, and .zip files on Wired network
2. All file types are received at the destination with correct content
3. No network-related errors occur during transfer on Wired network

#### C1.5. Verify Solace Tool File Transfer on Wireless Network
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. Solace messaging infrastructure is configured on Wireless network
3. The File Path directory is set and accessible
4. .cnt, .pos, and .zip export files exist in the directory

**Step(s):**
1. Configure the Solace tool to operate on Wireless network
2. Place .cnt, .pos, and .zip files in the configured File Path directory
3. Trigger the Solace tool to transfer files
4. Verify all three file types are transferred successfully on Wireless network
5. Verify file integrity at the destination

**Test Result(s):**
1. The Solace tool successfully transfers .cnt, .pos, and .zip files on Wireless network
2. All file types are received at the destination with correct content
3. No network-related errors occur during transfer on Wireless network

### C2. Export Configuration Compatibility

#### C2.1. Verify Solace Tool with General Sales Export V2 Configuration
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. General Sales Export V2 interface is configured with Export Path, Messaging Interface Code, and Solace settings
3. The File Path directory matches the Export Path configuration
4. .cnt, .pos, and .zip export files are generated by the General Sales Export V2 interface

**Step(s):**
1. Configure the General Sales Export V2 interface with Solace messaging
2. Trigger the export to generate .cnt, .pos, and .zip files in the Export Path
3. Observe the Solace tool detects and transfers the files
4. Verify the Messaging Interface Code is correctly used for routing
5. Verify all three file types are transferred via the configured Solace messaging interface

**Test Result(s):**
1. The Solace tool correctly integrates with General Sales Export V2 configuration
2. .cnt, .pos, and .zip files generated by the export are detected and transferred
3. The Messaging Interface Code is correctly applied for Solace message routing
4. All three file types are successfully transferred via the Solace messaging interface

## Appendix

### JIRA Issue Information
- **Issue Key**: HERO-68942
- **Summary**: HKJC - TMS - Solace tool for export file support .cnt and .pos files
- **Issue Type**: Improvement
- **Component**: RESERVATION
- **Status**: READY FOR QA

### Requirement Description

**Use Case / Background:**
HKJC have three export files need to transfer via Solace. Solace tool for exported file transfer was developed. Previous the tool supports transfer of .zip files. It's required now to support the file type to .cnt and .pos files as well.

**Requirement:**
System detects and sends all the relevant .cnt / .pos / .zip files in the directory set in File Path and match File name pattern. ** Only limit to these 3 types, no need to send other file types.

### Supported File Types
| File Type | Extension | Status |
| --- | --- | --- |
| .cnt | .cnt | New support (this improvement) |
| .pos | .pos | New support (this improvement) |
| .zip | .zip | Existing support (regression) |
| Other types | .csv, .txt, .xml, .pdf, .xlsx, etc. | Not supported (excluded by design) |
