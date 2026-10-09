# HERO-69416 - [Tech][SSDLC - Reliability + Maintainability] TMS 2.0 - SonarQube issue - Part 6 - 2/2

## Test Environment Information
- Local Test Environment
- HQ Test Environment
- TMS 2.0 Operation Module (React frontend)
- TMS Backend (PHP Plugin)

## Functional Testing

### F1. Edit Booking Page - Period Type and OnLoad Verification

#### F1.1. Verify Period Type Selection in Edit Booking Page
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. At least one reservation exists in the system
3. The reservation has period type configuration

**Step(s):**
1. Navigate to Operation > Search view
2. Search for any existing reservation
3. Click the edit button on the reservation
4. Observe the edit booking page loading
5. Locate the Period Type field/form element
6. Interact with the Period Type selector (change selection, verify available options)
7. Save the booking with the selected period type

**Reproduction Result(s):**
1. The "==" check in PeriodType.jsx (line 127) always evaluated to true, causing the period type comparison logic to be ineffective
2. Period type selection may not correctly reflect the chosen value due to the always-true comparison
3. The period type field could display incorrect or default state regardless of user selection

**Fix Result(s):**
1. The incorrect "==" check has been removed from PeriodType.jsx (line 127); the comparison logic now evaluates correctly
2. Period type selection correctly reflects the user's chosen value
3. The period type field displays the correct selected state on the edit booking page

#### F1.2. Verify Edit Booking Page Loads Successfully (OnLoadEditBooking)
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. At least one reservation exists in the system

**Step(s):**
1. Navigate to Operation > Search view
2. Search for any existing reservation
3. Click the edit button on the reservation
4. Observe the edit booking page loading process
5. Verify all form fields and elements render correctly
6. Navigate away and back to verify consistent loading behavior

**Reproduction Result(s):**
1. The OnLoadEditBooking.jsx component (line 24-55) returned a Promise where a void return was expected by the React Component lifecycle, potentially causing unexpected behavior or warnings during component mounting
2. The edit booking page may exhibit inconsistent loading behavior or React lifecycle warnings in the console

**Fix Result(s):**
1. The Promise-returning method in OnLoadEditBooking.jsx (line 24-55) has been corrected to return void as expected by the Component type; no lifecycle warnings are generated
2. The edit booking page loads successfully and consistently without React lifecycle errors
3. All form fields and elements render correctly on the edit booking page

### F2. Mark Arrival and Floor Plan Verification

#### F2.1. Verify Mark Arrival Dialog Opens and Confirms
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. A reservation exists in the Current view that has not yet been marked as arrived
3. Floor plan view is configured and accessible

**Step(s):**
1. Navigate to Operation > Current view
2. Create a new reservation or locate an existing un-arrived reservation
3. Click the Mark Arrival button on the reservation
4. Observe the Mark Arrival dialog opening
5. Click the Confirm button to mark arrival (no need to call POS Interface)
6. Verify the reservation status changes to arrived
7. Verify the seat color on the floor plan changes to indicate arrival status

**Reproduction Result(s):**
1. The MarkArrivalDialog.jsx component (line 63-106) returned a Promise where a void return was expected by the React Component lifecycle, potentially causing inconsistent dialog behavior or lifecycle warnings
2. The Mark Arrival dialog may not open or behave consistently due to the incorrect lifecycle method return type

**Fix Result(s):**
1. The Promise-returning method in MarkArrivalDialog.jsx (line 63-106) has been corrected to return void as expected by the Component type
2. The Mark Arrival dialog opens successfully and consistently
3. The reservation is marked as arrived and the seat color changes correctly on the floor plan

#### F2.2. Verify Floor Plan Frame Rendering and Seat Color Change
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. A reservation has been marked as arrived
3. Floor plan view is configured with seats

**Step(s):**
1. Navigate to the Floor Plan view
2. Observe the floor plan frame rendering
3. Verify that seats with arrived reservations display the correct seat color
4. Click on different seat areas to verify the conditional rendering logic
5. Verify that all conditional code blocks in the floor plan frame produce distinct visual outcomes

**Reproduction Result(s):**
1. The FloorPlanFrame.jsx (line 663-682) contained a conditional structure where all code blocks were identical, meaning the conditional logic was redundant and did not produce different outcomes for different conditions
2. Seat color changes and floor plan rendering may not correctly differentiate between different reservation states due to the identical conditional blocks

**Fix Result(s):**
1. The redundant conditional structure in FloorPlanFrame.jsx (line 663-682) has been removed or corrected so that code blocks produce distinct outcomes per condition
2. The floor plan frame renders correctly with distinct visual outcomes for different conditions
3. Seat color changes correctly to reflect the arrived reservation status

### F3. Change Arrival Cover Dialog Verification

#### F3.1. Verify Change Arrival Cover Dialog Opens and Updates Cover
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. A reservation has been marked as arrived (from F2.1)
3. The arrival icon is accessible on the arrived reservation

**Step(s):**
1. On the arrived reservation, click the arrival icon again
2. Observe the Change Arrival Cover dialog opening
3. Change the arrival cover value in the dialog
4. Click the Confirm button
5. Verify the arrival cover value has been updated successfully

**Reproduction Result(s):**
1. The ChangeArrivalCoverDialog.jsx component (line 24-35) returned a Promise where a void return was expected by the React Component lifecycle, potentially causing the dialog to not open or behave inconsistently
2. The change arrival cover dialog may exhibit unexpected behavior or React lifecycle warnings

**Fix Result(s):**
1. The Promise-returning method in ChangeArrivalCoverDialog.jsx (line 24-35) has been corrected to return void as expected by the Component type
2. The Change Arrival Cover dialog opens successfully
3. The arrival cover value is updated correctly after confirmation

### F4. Memo View Verification

#### F4.1. Verify Memo Display in Each Period
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. Memos exist in multiple periods in the system

**Step(s):**
1. Navigate to Operation > Memo view
2. Observe the memo list display
3. Switch between different periods
4. Verify that memos in each period display normally
5. Add or edit a memo and verify the state updates correctly
6. Verify the memo list updates reflect previous state correctly

**Reproduction Result(s):**
1. The MemoView.jsx (line 147) used setState referencing the previous state without a callback function, which could cause race conditions or incorrect state updates when rapid successive updates occur
2. Memo display in different periods may show stale or incorrect data due to the incorrect setState usage

**Fix Result(s):**
1. The setState call in MemoView.jsx (line 147) now uses a callback function to correctly reference the previous state
2. Memos in each period display normally and correctly
3. State updates are consistent even with rapid successive operations

### F5. Bulk Block Table Verification

#### F5.1. Verify Outlet Option in Bulk Block Table
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. Outlet configuration data exists in the system
3. Bulk block table page is accessible

**Step(s):**
1. Navigate to the Bulk Block Table page
2. Locate the Outlet option/selector
3. Select different outlet options
4. Verify the table data updates correctly based on the selected outlet
5. Test with scenarios where outlet data may be empty or undefined

**Reproduction Result(s):**
1. The BulkBlockTable.jsx (line 255) did not handle the case where "outlet" is null or undefined, which could throw a TypeError when the outlet data is missing or not yet loaded
2. The bulk block table may crash or display an error when outlet data is null or undefined

**Fix Result(s):**
1. The null/undefined check for "outlet" has been added in BulkBlockTable.jsx (line 255); TypeError is no longer thrown
2. The outlet option works normally and gracefully handles null or undefined outlet data
3. The bulk block table data updates correctly based on the selected outlet

#### F5.2. Verify Period Type Option in Bulk Block Table
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. Period type configuration data exists in the system
3. Bulk block table page is accessible

**Step(s):**
1. Navigate to the Bulk Block Table page
2. Locate the Period Type option/selector
3. Select different period type options
4. Verify the table data updates correctly based on the selected period type
5. Test with scenarios where period type data may be empty or undefined

**Reproduction Result(s):**
1. The BulkBlockTable.jsx (line 256) did not handle the case where "periodType" is null or undefined, which could throw a TypeError when the period type data is missing or not yet loaded
2. The bulk block table may crash or display an error when period type data is null or undefined

**Fix Result(s):**
1. The null/undefined check for "periodType" has been added in BulkBlockTable.jsx (line 256); TypeError is no longer thrown
2. The period type option works normally and gracefully handles null or undefined period type data
3. The bulk block table data updates correctly based on the selected period type

### F6. Print Period Reservation Component Verification

#### F6.1. Verify Print Period Reservation Component
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. A reservation with period data exists in the system
3. The Print Period Reservation component is accessible

**Step(s):**
1. Navigate to a reservation that has period reservation data
2. Trigger the Print Period Reservation function
3. Observe the print preview or print output
4. Verify the period reservation data is displayed correctly in the print output

**Reproduction Result(s):**
1. The PrintPeriodResvComp.tsx (line 168) provided a Promise-returning function to a property where a void return was expected, potentially causing the print component to behave unexpectedly
2. The print period reservation function may not execute correctly or may generate warnings

**Fix Result(s):**
1. The Promise-returning function in PrintPeriodResvComp.tsx (line 168) has been corrected to return void as expected by the property type
2. The Print Period Reservation component works correctly and displays period reservation data accurately in the print output

### F7. Interface Field Dialog Verification

#### F7.1. Verify Interface Field Dialog Case Handling
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. Interface field configuration data exists in the system
3. The Interface Field Dialog is accessible

**Step(s):**
1. Navigate to the Interface Field Dialog
2. Trigger different case/option scenarios in the dialog
3. Verify each case produces the correct and distinct outcome
4. Specifically test the case that was duplicated (line 85 duplicate)
5. Verify the dialog handles all field types correctly

**Reproduction Result(s):**
1. The InterfaceFieldDialog.jsx (line 85) contained a duplicate case statement, meaning one of the case branches was unreachable and the duplicate case did not produce a distinct outcome
2. The interface field dialog may not correctly handle the duplicated case scenario, leading to incorrect field behavior

**Fix Result(s):**
1. The duplicate case in InterfaceFieldDialog.jsx (line 85) has been removed or merged; each case now produces a distinct and correct outcome
2. The Interface Field Dialog handles all field types and cases correctly

### F8. Common Function - Car Park Coach Service Verification

#### F8.1. Verify Car Park Coach Service List Handling
**Prerequisite(s):**
1. User is logged in to TMS 2.0 with valid credentials
2. The CommonFunction module is accessible
3. Car park coach service configuration may or may not exist in the system

**Step(s):**
1. Navigate to a view or function that triggers the CommonFunction module
2. Trigger the function that accesses carParkCoachServiceList
3. Test with car park coach service data present
4. Test with car park coach service data absent (null or undefined)
5. Verify the function handles both scenarios without errors

**Reproduction Result(s):**
1. The CommonFunction.jsx (line 2269) accessed carParkCoachServiceList without checking for null or undefined, which could throw a TypeError when the list is missing
2. The common function may crash when car park coach service data is null or undefined

**Fix Result(s):**
1. The null/undefined check for "carParkCoachServiceList" has been added in CommonFunction.jsx (line 2269); TypeError is no longer thrown
2. The common function gracefully handles null or undefined car park coach service data
3. The function works correctly with both present and absent car park coach service data

### F9. TMS API Table Component Verification

#### F9.1. Verify TMS API Table Component Loop Variable Handling
**Prerequisite(s):**
1. User is logged in to TMS with valid credentials
2. The TMS API Table Component is accessible (PHP backend)
3. Table component data exists for processing

**Step(s):**
1. Navigate to a page or function that triggers the TMS API Table Component
2. Trigger the table component processing with loop iterations
3. Verify the loop processes all items correctly
4. Verify the referenced value variable is correctly unset after the loop completes
5. Verify subsequent operations after the loop do not reference stale loop data

**Reproduction Result(s):**
1. The TmsApiTableComponent.php (line 422) did not unset the referenced value variable after the loop, which could cause subsequent code to reference stale or incorrect loop data
2. The table component may produce incorrect results in operations following the loop due to the lingering referenced value

**Fix Result(s):**
1. The referenced value variable in TmsApiTableComponent.php (line 422) is now correctly unset after the loop completes
2. The TMS API Table Component processes all items correctly and subsequent operations do not reference stale loop data
3. The table component produces correct and consistent results

## Regression Test Scope Analysis

### Affected Modules and Functions

The SonarQube fixes in HERO-69416 impact the following modules and functions within the TMS 2.0 system. Regression testing should cover these areas to ensure no functional degradation:

| No. | Module / File | Line(s) | Issue Type | Regression Risk |
| --- | --- | --- | --- | --- |
| 1 | react/Tms/operation/src/components/BookingView/FormElements/PeriodType.jsx | 127 | Logic error (always-true comparison) | Medium - affects period type selection in booking view |
| 2 | react/Tms/operation/src/components/ChangeArrivalCoverDialog.jsx | 24-35 | React lifecycle (Promise return) | Medium - affects change arrival cover dialog |
| 3 | react/Tms/operation/src/components/CommonFunction.jsx | 2269 | Null reference (TypeError risk) | High - common utility used across multiple modules |
| 4 | react/Tms/operation/src/components/FloorPlan/FloorPlanFrame.jsx | 663-682 | Redundant conditional structure | Medium - affects floor plan rendering |
| 5 | react/Tms/operation/src/components/MarkArrivalDialog.jsx | 63-106 | React lifecycle (Promise return) | Medium - affects mark arrival dialog |
| 6 | react/Tms/operation/src/components/OnLoad/OnLoadEditBooking.jsx | 24-55 | React lifecycle (Promise return) | Medium - affects edit booking page loading |
| 7 | react/Tms/operation/src/components/PrintPeriodResvComp.tsx | 168 | React property (Promise return) | Low - affects print period reservation |
| 8 | react/Tms/operation/src/components/dialog/InterfaceFieldDialog.jsx | 85 | Duplicate case statement | Low - affects interface field dialog |
| 9 | react/Tms/operation/src/containers/BulkBlockTable.jsx | 255-256 | Null reference (TypeError risk) | High - affects bulk block table outlet and period type |
| 10 | react/Tms/operation/src/containers/Memo/MemoView.jsx | 147 | setState without callback | Medium - affects memo view state updates |
| 11 | Plugin/Tms/Controller/Component/TmsApiTableComponent.php | 422 | Variable not unset after loop | Medium - affects TMS API table component (backend) |

### Regression Test Recommendations

1. **Booking View and Edit Booking**: Verify period type selection, edit booking page loading, and reservation editing workflows function correctly
2. **Arrival Workflow**: Verify the complete mark arrival and change arrival cover workflow, including floor plan seat color changes
3. **Memo View**: Verify memo display and state updates across all periods
4. **Bulk Block Table**: Verify outlet and period type selection and data display, including edge cases with null/undefined data
5. **Common Functions**: Verify all functions that depend on CommonFunction module, especially car park coach service related features
6. **Print Functions**: Verify print period reservation and related print outputs
7. **Interface Dialogs**: Verify interface field dialog and other dialog components
8. **TMS API Table Component**: Verify backend table component processing and API responses

### Cross-Module Dependencies

- **CommonFunction.jsx** is a shared utility module used across multiple TMS operation components; changes here may have broad impact
- **FloorPlanFrame.jsx** is used in floor plan rendering which is referenced by arrival and reservation workflows
- **BulkBlockTable.jsx** outlet and period type handling may affect bulk operations and reporting

## Appendix

### JIRA Issue Information
- **Issue Key**: HERO-69416
- **Summary**: [Tech][SSDLC - Reliability + Maintainability] TMS 2.0 - SonarQube issue - Part 6 - 2/2
- **Issue Type**: Bug
- **Component**: RESERVATION
- **Status**: READY FOR QA
- **Fix Version**: V1.2.116.0
- **Release Date**: 4th Nov 2026

### SonarQube Issues Addressed

| Description | Location |
| --- | --- |
| Remove this "==" check; it will always be true. Did you mean to use "!="? | react/Tms/operation/src/components/BookingView/FormElements/PeriodType.jsx line 127 |
| Promise-returning method provided where a void return was expected by extended/implemented type 'Component<any, any, any>'. | react/Tms/operation/src/components/ChangeArrivalCoverDialog.jsx line 24-35 |
| TypeError can be thrown as "carParkCoachServiceList" might be null or undefined here. | react/Tms/operation/src/components/CommonFunction.jsx line 2269 |
| Remove this conditional structure or edit its code blocks so that they're not all the same. | react/Tms/operation/src/components/FloorPlan/FloorPlanFrame.jsx line 663-682 |
| Promise-returning method provided where a void return was expected by extended/implemented type 'Component<any, any, any>'. | react/Tms/operation/src/components/MarkArrivalDialog.jsx line 63-106 |
| Promise-returning method provided where a void return was expected by extended/implemented type 'Component<any, any, any>'. | react/Tms/operation/src/components/OnLoad/OnLoadEditBooking.jsx line 24-55 |
| Promise-returning function provided to property where a void return was expected. | react/Tms/operation/src/components/PrintPeriodResvComp.tsx line 168 |
| This case duplicates the one on line 85 | react/Tms/operation/src/components/dialog/InterfaceFieldDialog.jsx line 85 |
| TypeError can be thrown as "outlet" might be null or undefined here. | react/Tms/operation/src/containers/BulkBlockTable.jsx line 255 |
| TypeError can be thrown as "periodType" might be null or undefined here. | react/Tms/operation/src/containers/BulkBlockTable.jsx line 256 |
| Use callback in setState when referencing the previous state. | react/Tms/operation/src/containers/Memo/MemoView.jsx line 147 |
| Make sure that the referenced value variable is unset after the loop. | Plugin/Tms/Controller/Component/TmsApiTableComponent.php line 422 |

### Release Notes Setup Instructions
1. Go to operation > Search view, search any reservation and click the edit button. The edit booking page should open successfully.
2. Go to current view, make a new reservation. Then click the mark arrival button. The mark arrival dialog should open successfully. Click confirm to mark arrival (No need to call POS Interface). The reservation should be arrived and the seat color should be changed. Click the arrival icon again. The change arrival cover dialog should open successfully. Change the arrival cover and click confirm. If success, the arrival cover should change.
3. Go to memo view, check the memo in each period should show normally.
4. Go to bulk block table page. Check the outlet and period type option work normally.
