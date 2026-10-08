# HERO-69218 Test Report

## Functional Testing

### F1.Simple Ordering Kiosk Mode — Unable To Order Items

#### F1.1.Reproduce The Ordering Failure In Simple Ordering Kiosk Mode

**Prerequisite(s):**
1. The station is configured in Infrasys POS platform > POS system > Station setup > Stations with Ordering mode = "Self-order Kiosk" and Ordering Mode Sub Type = "Simple Ordering Kiosk Mode"
2. The target menu list is configured and published for the kiosk outlet (with at least one menu / item available for ordering)
3. The POS client is deployed with the legacy (pre-fix) build for reproduction purposes
4. Test environment: Local test environment
5. Test device: Workstation (Windows 11 Enterprise) configured as a self-order kiosk station

**Step(s):**
1. Launch the POS client in simple ordering kiosk mode on the target kiosk station
2. Log in to the kiosk client (or confirm the kiosk is in self-service mode)
3. Open the ordering panel
4. Click on the target menu list

**Reproduction Result(s):**
1. The POS client launches and enters simple ordering kiosk mode
2. The ordering panel opens
3. Clicking on the target menu list produces no response — the menu list does not open and no items are displayed
4. The kiosk is unable to order any items, matching the reported defect symptom

#### F1.2.Verify The Fix Allows Ordering In Simple Ordering Kiosk Mode

**Prerequisite(s):**
1. The station is configured in Infrasys POS platform > POS system > Station setup > Stations with Ordering mode = "Self-order Kiosk" and Ordering Mode Sub Type = "Simple Ordering Kiosk Mode"
2. The target menu list is configured and published for the kiosk outlet (with at least one menu / item available for ordering)
3. The POS client is deployed with the fixed build
4. Test environment: Local test environment
5. Test device: Workstation (Windows 11 Enterprise) configured as a self-order kiosk station

**Step(s):**
1. Launch the POS client in simple ordering kiosk mode on the target kiosk station
2. Log in to the kiosk client (or confirm the kiosk is in self-service mode)
3. Open the ordering panel
4. Click on the target menu list
5. Verify the menu list opens and displays the items inside
6. Select an item, configure modifiers / quantity as needed, and add the item to the order
7. Verify the item is added to the order and the kiosk is able to place the order

**Fix Result(s):**
1. The POS client launches and enters simple ordering kiosk mode successfully
2. The ordering panel opens
3. Clicking on the target menu list opens the menu list and displays the items inside — the kiosk is able to order items
4. Selecting an item, configuring modifiers / quantity and adding the item to the order works as expected
5. The order is placed successfully and the order is reflected correctly in the kiosk ordering flow

#### F1.3.Regression — Verify Self-Order Kiosk And Other Ordering Modes Are Preserved

**Prerequisite(s):**
1. The fixed POS client build is deployed
2. The station is configured with multiple ordering modes — Self-order Kiosk (Simple Ordering Kiosk Mode and other sub types) and standard table service / quick service ordering
3. Test environment: Local test environment
4. Test device: Workstation (Windows 11 Enterprise)

**Step(s):**
1. Launch the POS client in standard table service / quick service ordering mode and verify normal ordering works (open menu list, order items, close check)
2. Switch the station to Self-order Kiosk mode with a different sub type (e.g. standard kiosk mode if available) and verify ordering works
3. Switch back to Simple Ordering Kiosk Mode and re-verify the fixed ordering flow (open menu list, order items)
4. Inspect the POS client and LS logs for any ordering-mode-related errors

**Fix Result(s):**
1. Standard table service / quick service ordering mode continues to work as expected after the fix — no regression is introduced
2. Other Self-order Kiosk sub types continue to work as expected after the fix
3. Simple Ordering Kiosk Mode continues to work as expected after the fix — the menu list opens and items can be ordered
4. No ordering-mode-related errors are observed in the POS client or LS logs

## Test Environment Information

- Local test environment

## Appendix

**JIRA Acceptance Criteria (HERO-69218):**

- Station in simple ordering kiosk mode is unable to order any items.
- Station setup: Infrasys POS platform > POS system > Station setup > Stations > target station; Ordering mode = Self-order Kiosk; Ordering Mode Sub Type = Simple Ordering Kiosk Mode.
- Workflow: 1) Login the client in simple ordering kiosk mode; 2) Open the ordering panel; 3) Click on the target menu list; 4) System cannot open the menu list and has no response.
- Expected result: System opens the menu list, displays the items inside and is able to order items.
- After the fix, the simple ordering kiosk mode must allow ordering items, and other ordering modes must continue to work as expected.
