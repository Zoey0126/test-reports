# Cloud hosted (Offline Mode) [Phase 3] - Support sync park order Test Report

## Functional Testing

### 1.Park Order Sync from CH to SS

#### 1.1.Park Orders Sync from Cloud Hosted to Smart Station When Not in Standalone Mode

**Prerequisite(s):**
1. A Simple Smart Station with offline mode is deployed and connected to Cloud Hosted (CH)
2. The smart station is not in standalone mode
3. Digital Dine (POSgAPI) can create park orders

**Step(s):**
1. Create a park order from Digital Dine while CH is the primary
2. Check the park order records on the smart station
3. Modify or add another park order on CH and re-check the smart station

**Test Result(s):**
1. Park order files are synced from CH to the smart station
2. The park orders on the smart station match the records on CH, including updates

#### 1.2.SS Receives New Park Orders from MQ After Switching to SS During CH Downtime

**Prerequisite(s):**
1. The smart station is operating in SS + MQ mode after switching over due to CH downtime
2. Digital Dine can still submit park order requests

**Step(s):**
1. With CH unreachable, create new park orders from Digital Dine
2. Check the park order records received by the smart station through MQ
3. Operate on the received park orders at the smart station

**Test Result(s):**
1. The smart station starts receiving new park order records from MQ after the switch to SS + MQ activation
2. New park orders created during CH downtime are available on the smart station and can be handled normally

### 2.Park Order Sync from SS to CH After Recovery

**Prerequisite(s):**
1. Park orders were created on the smart station while CH was unreachable
2. CH has recovered and the operation is switched back from SS to CH

**Step(s):**
1. Switch the operation back from SS to CH after CH recovery
2. Check the park order records on CH that were created on the smart station during the downtime
3. Open the synced park orders on CH

**Test Result(s):**
1. Park orders created in SS are synced back to CH upon CH recovery and the operation switch back from SS to CH
2. The synced park orders are complete and can be handled normally on CH

### 3.CH Works as Primary After Switch Back

**Prerequisite(s):**
1. The operation has been switched back from SS to CH after recovery
2. Digital Dine (POSgAPI) is connected to CH again

**Step(s):**
1. Create a new park order from Digital Dine after the switch back
2. Check where the new park order is received (CH or SS)
3. Check the smart station for any new park order records

**Test Result(s):**
1. CH works as the primary and receives new park orders from Digital Dine (POSgAPI)
2. The smart station no longer receives new park orders directly after the switch back

### 4.Standalone Mode Boundary

#### 4.1.No CH to SS Park Order Sync While in Standalone Mode

**Prerequisite(s):**
1. The smart station is in standalone mode
2. CH is reachable

**Step(s):**
1. Create or update park orders on CH
2. Check the park order records on the smart station in standalone mode

**Test Result(s):**
1. While in standalone mode, park orders are not synced from CH to the smart station
2. The standalone park order data on the smart station stays unchanged by CH activity

#### 4.2.Leaving Standalone Mode Triggers SS to CH Park Order Sync

**Prerequisite(s):**
1. The smart station is in standalone mode and holds park order records created locally
2. The connection to CH is available

**Step(s):**
1. Click Leave standalone mode on the smart station
2. Wait for the sync to complete
3. Check the park order records on CH

**Test Result(s):**
1. When leaving standalone mode, park orders are synced from SS to CH
2. The park orders created during standalone mode are available on CH after the station leaves standalone mode

### 5.Park Order Data Integrity After Round-Trip Sync

**Prerequisite(s):**
1. The smart station supports offline mode and CH connectivity can be controlled
2. Several park orders with different item counts exist

**Step(s):**
1. Create park orders on CH, sync them to SS, switch to SS during CH downtime and create more park orders on SS
2. Recover CH, switch back and let the sync complete
3. Compare every park order record before and after the round-trip sync

**Test Result(s):**
1. All park orders exist exactly once on CH after the round trip, with no loss and no duplication
2. Park order content (items, quantities and timestamps) is consistent before and after the sync
