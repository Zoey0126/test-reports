# KDS2.0 - Order Handling - Pantry Message and Rush Order Test Report

## Functional Testing

### 1.Check-Level Pantry Message Display

#### 1.1.First Check-Level Pantry Message Is Displayed with Timestamp

**Prerequisite(s):**
1. A check with at least one item is open on the KDS
2. The POS supports sending a pantry message at check level

**Step(s):**
1. Send a check-level pantry message from the POS for the open check
2. Observe the ticket on the KDS tickets listing screen and in the ticket details

**Test Result(s):**
1. The pantry message and its creation timestamp are displayed on the ticket
2. The message content matches what was sent from the POS

#### 1.2.Latest Two Check-Level Pantry Messages Are Shown on Top

**Prerequisite(s):**
1. A check already has one check-level pantry message displayed
2. The POS can send additional pantry messages for the same check

**Step(s):**
1. Send a second check-level pantry message from the POS
2. Send a third check-level pantry message from the POS
3. Observe the pantry message list on the ticket

**Test Result(s):**
1. After the second message, the two most recent pantry messages and their corresponding timestamps are displayed on the ticket
2. The latest message appears on top of the list
3. After the third message, the two most recent messages are shown and the oldest message is no longer displayed

#### 1.3.Check-Level Pantry Message Content Up to 256 Characters

**Prerequisite(s):**
1. A check is open on the POS with the KDS connected

**Step(s):**
1. Send a check-level pantry message with exactly 256 characters
2. Send a check-level pantry message longer than 256 characters
3. Observe the KDS display for both cases

**Test Result(s):**
1. The 256-character message is displayed in full without losing valid content
2. Content beyond the 256-character limitation is rejected or truncated per the agreed rule and never breaks the KDS ticket display

### 2.Item-Level Pantry Message Display

#### 2.1.Latest Item-Level Pantry Message Replaces the Previous Message

**Prerequisite(s):**
1. A KDS ticket item exists and the POS can send an item-level pantry message

**Step(s):**
1. Send an item-level pantry message for the ticket item
2. Send a different item-level pantry message for the same item
3. Observe the message shown under the item

**Test Result(s):**
1. The latest pantry message and its corresponding timestamp are displayed on the ticket item
2. The old message is replaced by the new message and is no longer shown on the item

#### 2.2.Item-Level Pantry Message 256-Character Limitation

**Prerequisite(s):**
1. A KDS ticket item exists and the POS is connected

**Step(s):**
1. Send an item-level pantry message with exactly 256 characters
2. Send an item-level pantry message with more than 256 characters
3. Observe the KDS item display

**Test Result(s):**
1. The 256-character message is displayed correctly on the item
2. Content beyond 256 characters is handled per the limitation rule without corrupting the item display

### 3.Pantry Message Display on Expo and Station Screens

**Prerequisite(s):**
1. Both an Expo screen and a Station screen are available in the KDS environment
2. A check with a check-level pantry message and an item with an item-level pantry message exist

**Step(s):**
1. Open the ticket on the Expo screen and observe the check-level and item-level pantry messages
2. Open the same ticket on the Station screen and observe the same messages

**Test Result(s):**
1. Check-level pantry messages are displayed on both the Expo and Station screens accordingly
2. Item-level pantry messages are displayed on both the Expo and Station screens accordingly
3. Message content and timestamps are consistent between the two screens

### 4.Rush Order Indicator and Count

#### 4.1.First Rush Shows Rush Indicator with Rush Time

**Prerequisite(s):**
1. A KDS ticket with at least one item is displayed
2. The POS supports marking a ticket item as Rush

**Step(s):**
1. Mark the ticket item as Rush from the POS for the first time
2. Observe the line item on the KDS

**Test Result(s):**
1. The Rush (1) indicator is displayed under the line item
2. The rush time (the time it was sent from the POS) is displayed with the item in hh:mm:ss format

#### 4.2.Second Rush Increases the Count and Keeps the Initial Rush Time

**Prerequisite(s):**
1. A ticket item has already been marked as Rush once and shows Rush (1) with its rush time

**Step(s):**
1. Mark the same ticket item as Rush again from the POS
2. Observe the rush indicator and rush time on the line item

**Test Result(s):**
1. The rush count increases to Rush (2)
2. The rush timestamp remains unchanged and still shows the initial rush time

### 5.Rush Order Display on Expo and Station Screens

**Prerequisite(s):**
1. A ticket item has been marked as Rush at least once
2. Both an Expo screen and a Station screen are available

**Step(s):**
1. Open the ticket on the Expo screen and observe the rush indicator and rush time on the item
2. Open the same ticket on the Station screen and observe the same information

**Test Result(s):**
1. The rush information (rush count and rush time) is displayed on both the Expo and Station screens
2. The rush time format is hh:mm:ss and is consistent on both screens

### 6.Rush Order Applies to Item Level Only

**Prerequisite(s):**
1. The POS exposes rush operations for checks and for individual items

**Step(s):**
1. Attempt to mark a whole check as Rush from the POS
2. Mark an individual item as Rush from the POS
3. Observe the KDS display for both operations

**Test Result(s):**
1. Check-level rush order is not supported and no check-level rush indicator is displayed
2. Only the marked item shows the rush indicator, rush time and rush count
