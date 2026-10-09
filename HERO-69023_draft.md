# HERO-69023 Bug Fix Test Report - Incorrect Municipality Tax Calculation After Waive by Override Condition

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Municipality Tax Waive via Override Condition

#### F1.1.Verify Municipality Tax (Tax 9) Waived Correctly After Assigning Check Type with Override Condition

**Prerequisite(s):**
1. Shiji Fiscal Exchange (SFE) interface is configured and available
2. Municipality tax is enabled: System Management > System Configuration > POS module > "Support municipality tax" = Yes
3. Tax setup with municipality tax: POS System > Ordering Setup > Tax & S.C. Types > Tax (index: 1) > Rate (%) = 24, Municipality Tax Index = 9, Municipality Rate (%) = 0.5%
4. Municipality tax setup: POS System > Ordering Setup > Tax & S.C. Types > Tax (index: 9) > Rate (%) = 24
5. Override condition for waive Municipality Tax is created: POS System > Ordering Setup > Override Conditions > General tab > Custom Type = target check type; Tax & S.C. tab > Tax 9 = Waive Tax for All Items
6. Override condition applied to old items when check type changed: POS System > Ordering Setup > Config by Location > Override Conditions Setting > Tax 9 > "Apply override conditions on old items when check type is changed" = Yes
7. A new check is open and an item with Tax 1 and Municipality Tax 9 has been ordered

**Step(s):**
1. Open the check with the ordered item (Tax 1 and Municipality Tax 9 applied)
2. Verify the tax calculation before waive: Tax 1 calculated at 24%, Municipality Tax 9 calculated at 0.5%
3. Click function "Assign Check Type" and select the target check type that triggers the override condition
4. Observe the tax calculation after the override condition is applied
5. Verify Municipality Tax (Tax 9) is waived from the check

**Reproduction Result(s):**
1. After assigning the check type, Municipality Tax (Tax 9) is waived
2. Tax 1 is still calculated with the Municipality tax rate (0.5%) instead of rolling back to 24%
3. The check total is incorrectly calculated because Tax 1 retains the wrong rate
4. The system posts the incorrect tax value to SFE

**Fix Result(s):**
1. After assigning the check type, Municipality Tax (Tax 9) is waived correctly
2. Tax 1 rate rolls back to 24% after Municipality Tax (Tax 9) is waived
3. The check total is calculated correctly with Tax 1 at 24% and Municipality Tax 9 waived
4. The system posts the correct tax value to SFE matching the expected posting packet

#### F1.2.Verify Tax 1 Rate Rollback to 24% After Municipality Tax Waive

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. Municipality tax is enabled and tax types are configured (Tax 1 Rate = 24%, Municipality Tax Index = 9, Municipality Rate = 0.5%)
3. Override condition to waive Tax 9 for all items is configured and assigned to a target check type
4. "Apply override conditions on old items when check type is changed" is set to Yes

**Step(s):**
1. Open a new check
2. Order an item that has both Tax 1 and Municipality Tax 9 applied
3. Verify the initial tax breakdown: Tax 1 = 24%, Municipality Tax 9 = 0.5% on the item
4. Click "Assign Check Type" and select the target check type to trigger the override condition
5. Check the tax rate applied to Tax 1 on the item after the waive

**Reproduction Result(s):**
1. After the override condition is applied, Tax 1 still uses the Municipality tax rate (0.5%) instead of 24%
2. The tax amount for Tax 1 is incorrectly calculated based on the 0.5% rate
3. The item tax breakdown shows Tax 1 with the wrong rate

**Fix Result(s):**
1. After the override condition is applied, Tax 1 rate rolls back to 24%
2. The tax amount for Tax 1 is correctly calculated based on the 24% rate
3. The item tax breakdown shows Tax 1 at 24% and Municipality Tax 9 waived

#### F1.3.Verify SFE Posting Packet Matches Expected Value After Municipality Tax Waive

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. SFE interface is configured and connected
3. The reference expected posting packet "Expected posting packet for SFE after waive Municipality tax (Provided by SFE)" is available for comparison
4. Override condition to waive Tax 9 is configured and assigned to a target check type

**Step(s):**
1. Open a new check and order an item with Tax 1 and Municipality Tax 9
2. Click "Assign Check Type" and select the target check type to trigger the override condition
3. Verify Municipality Tax (Tax 9) is waived and Tax 1 rate rolls back to 24%
4. Trigger the check posting to SFE (e.g., settle/payment the check)
5. Capture the SFE posting packet sent by the system
6. Compare the captured SFE posting packet with the reference expected posting packet

**Reproduction Result(s):**
1. The SFE posting packet contains incorrect tax values: Tax 1 calculated with the Municipality tax rate (0.5%) instead of 24%
2. Municipality Tax is waived from the check so the transaction total matches on the SFE packet, but the tax breakdown is incorrect
3. The posted tax values do not match the reference expected posting packet provided by SFE

**Fix Result(s):**
1. The SFE posting packet contains correct tax values: Tax 1 calculated at 24% after Municipality Tax waive
2. Municipality Tax (Tax 9) is waived and not included in the posting
3. The posted tax values match the reference expected posting packet provided by SFE
4. The transaction total and tax breakdown are correct in the SFE posting

#### F1.4.Verify Override Condition Applied to Old Items When Check Type Changed

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. "Apply override conditions on old items when check type is changed" is set to Yes under POS System > Ordering Setup > Config by Location > Override Conditions Setting

**Step(s):**
1. Open a new check with default check type
2. Order multiple items with Tax 1 and Municipality Tax 9 applied
3. Click "Assign Check Type" to change the check type to the target check type that triggers the override condition
4. Verify the override condition is applied to the already-ordered (old) items
5. Verify Municipality Tax (Tax 9) is waived on all old items
6. Verify Tax 1 rate rolls back to 24% on all old items

**Reproduction Result(s):**
1. The override condition is applied to old items, waiving Municipality Tax 9
2. Tax 1 on old items still calculates with the Municipality tax rate (0.5%) instead of rolling back to 24%
3. The tax calculation for all old items is incorrect

**Fix Result(s):**
1. The override condition is applied to old items, waiving Municipality Tax 9 correctly
2. Tax 1 on all old items rolls back to 24% after Municipality Tax waive
3. The tax calculation for all old items is correct with Tax 1 at 24% and Municipality Tax 9 waived

### F2.Regression Testing - Tax Calculation and Related Reports

#### F2.1.Verify Normal Municipality Tax Calculation Without Waive (No Regression)

**Prerequisite(s):**
1. Municipality tax is enabled: System Management > System Configuration > POS module > "Support municipality tax" = Yes
2. Tax setup: Tax (index: 1) > Rate (%) = 24, Municipality Tax Index = 9, Municipality Rate (%) = 0.5%
3. Municipality tax setup: Tax (index: 9) > Rate (%) = 24
4. No override condition to waive Municipality Tax is applied to the check type in use

**Step(s):**
1. Open a new check with a check type that does NOT trigger the Municipality Tax waive override condition
2. Order an item with Tax 1 and Municipality Tax 9 applied
3. Verify the tax calculation: Tax 1 = 24%, Municipality Tax 9 = 0.5%
4. Verify the check total includes both Tax 1 and Municipality Tax 9 amounts
5. Settle the check and verify the SFE posting packet

**Reproduction Result(s):**
1. Not applicable - this is a regression test to ensure normal functionality is not broken
2. (If regression occurs) Tax 1 calculation is incorrect or Municipality Tax is not applied when it should be

**Fix Result(s):**
1. Tax 1 is calculated at 24% and Municipality Tax 9 at 0.5% as expected (no regression)
2. The check total includes both tax amounts correctly
3. The SFE posting packet contains correct tax values for both Tax 1 and Municipality Tax 9

#### F2.2.Verify Municipality Tax Waive for Single Item (No Other Items Affected)

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. Override condition to waive Tax 9 is configured and assigned to a target check type

**Step(s):**
1. Open a new check
2. Order Item A with Tax 1 and Municipality Tax 9
3. Order Item B with Tax 1 only (no Municipality Tax)
4. Click "Assign Check Type" and select the target check type to trigger the override condition
5. Verify Municipality Tax (Tax 9) is waived on Item A only
6. Verify Tax 1 on Item A rolls back to 24%
7. Verify Item B tax calculation is unaffected

**Reproduction Result(s):**
1. (If regression occurs) Item B tax calculation is affected by the waive on Item A
2. (If regression occurs) Tax 1 on Item A does not roll back correctly

**Fix Result(s):**
1. Municipality Tax (Tax 9) is waived on Item A only; Item B is unaffected
2. Tax 1 on Item A rolls back to 24% after Municipality Tax waive
3. Item B tax calculation remains correct and unaffected by the override condition on Item A

#### F2.3.Verify Revenue Report (Municipality Tax EMEA) - POS0297 Reflects Correct Tax Values After Waive

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. A check with Municipality Tax waived via override condition has been settled on the current business day
3. Revenue Report (Municipality Tax EMEA) - POS0297 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Revenue Report (Municipality Tax EMEA) - POS0297
3. Select Date Type = Current Business Day
4. Select the target shop and outlet where the waive check was settled
5. Generate the report
6. Verify the Municipality Tax column shows waived (zero) for the check where override condition was applied
7. Verify Tax 1 column shows the correct 24% tax amount for the waived check

**Reproduction Result(s):**
1. (If regression occurs) The Revenue Report shows incorrect Municipality Tax or Tax 1 values for the waived check
2. (If regression occurs) Tax 1 amount on the report does not reflect the 24% rollback

**Fix Result(s):**
1. The Revenue Report (POS0297) shows Municipality Tax as waived (zero) for the override-condition check
2. The Tax 1 column reflects the correct 24% tax amount after Municipality Tax waive
3. The report data is consistent with the actual check calculation and SFE posting

#### F2.4.Verify Detail Tax Exempt Check Listing After Discount Report - CUS0317 Reflects Correct Tax Exempt Data

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. A check with Municipality Tax waived via override condition has been settled on the current business day
3. Detail Tax Exempt Check Listing After Discount Report - CUS0317 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Detail Tax Exempt Check Listing After Discount Report - CUS0317
3. Select Date Type = Current Business Day
4. Select the target shop and outlet where the waive check was settled
5. Generate the report
6. Verify the tax exempt check listing reflects the Municipality Tax waive correctly
7. Verify the remaining Tax 1 amount is correct at 24%

**Reproduction Result(s):**
1. (If regression occurs) The report shows incorrect tax exempt data or does not reflect the waive correctly
2. (If regression occurs) Tax 1 amount on the report does not match the expected 24% calculation

**Fix Result(s):**
1. The Detail Tax Exempt Check Listing (CUS0317) correctly reflects the Municipality Tax waive
2. Tax 1 amount shows the correct 24% calculation after waive
3. The report data is consistent with the check calculation and SFE posting

#### F2.5.Verify Cashier Report - POS0084 Reflects Correct Tax Summary After Waive

**Prerequisite(s):**
1. All setup steps from F1.1 Prerequisite(s) are completed
2. A check with Municipality Tax waived via override condition has been settled on the current business day
3. Cashier Report - POS0084 is available in the report module

**Step(s):**
1. Navigate to Reports module
2. Open Cashier Report - POS0084
3. Select Date Type = Current Business Day
4. Select the target shop, outlet, and cashier who settled the waive check
5. Generate the report
6. Verify the tax summary section reflects Municipality Tax as waived
7. Verify Tax 1 summary is correct at 24% rate

**Reproduction Result(s):**
1. (If regression occurs) The Cashier Report tax summary shows incorrect values for the waived check
2. (If regression occurs) Tax 1 total does not match the expected 24% calculation

**Fix Result(s):**
1. The Cashier Report (POS0084) tax summary correctly reflects Municipality Tax as waived
2. Tax 1 total shows the correct 24% calculation after waive
3. The report totals are consistent with the actual settled check data

## Appendix

### Bug Description (HERO-69023)

**Component:** POS

**Current Behavior:**
It is available for Shiji Fiscal Exchange interface. When a check type with an override condition to waive Municipality Tax (Tax 9) is assigned, the system waives Municipality Tax 9 but still calculates Tax 1 with the Municipality tax rate (0.5%) instead of rolling back to 24%. The system posts the incorrect value to SFE.

**Workflow:**
1. Open new check
2. Order item with Tax 1 and Municipality Tax 9
3. Click function "Assign Check Type" to select the target check to trigger override condition
4. System applies override condition to waive Tax 9 (Municipality Tax 9 is waived)
5. System wrongly calculates the check after waived Municipality Tax and posts the incorrect value to SFE (Tax 1 still calculated with the Municipality tax rate, and Municipality Tax is waived from check that the transaction total is matched on SFE packet)

**Expected Result:**
System calculates the check after waived Municipality Tax correctly and posts the expected value to SFE. Tax rate of Tax 1 should rollback to 24% after Municipality Tax (Tax 9) is waived. Reference to attachment "Expected posting packet for SFE after waive Municipality tax (Provided by SFE)".

### Regression Test Scope - Related Modules and Functions

| Module / Function | Related Report | Regression Focus |
| --- | --- | --- |
| Tax Calculation Engine (Tax 1 rate rollback) | - | Verify Tax 1 reverts to base rate (24%) when Municipality Tax is waived |
| Override Conditions (Waive Tax for All Items) | - | Verify override condition correctly waives target tax without affecting other tax rates |
| SFE Posting (Shiji Fiscal Exchange) | - | Verify SFE posting packet matches expected values after Municipality Tax waive |
| Municipality Tax Calculation | POS0297 Revenue Report (Municipality Tax EMEA) | Verify report reflects correct Municipality Tax and Tax 1 values |
| Tax Exempt Check Listing | CUS0317 Detail Tax Exempt Check Listing After Discount Report | Verify tax exempt data reflects waive correctly |
| Cashier Tax Summary | POS0084 Cashier Report | Verify cashier tax summary totals are correct after waive |
| Slip Revenue Report | CUS0285 Slip Revenue Report (with Additional Average Data) | Verify revenue data reflects correct tax calculation |
| Cross Outlet Revenue | POS0099 Cross Outlet Revenue Report | Verify cross outlet revenue reflects correct tax values |
