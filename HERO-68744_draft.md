# POS Interface - VOYT Integration - Fix 'DishUpCall' issues Test Report

## Functional Testing

### 1.Pantry Message Format

#### 1.1.No pantry message applied - pantryMessage fields are not present with null values
**Prerequisite(s):**
1. POS is integrated with VOYT and DishUpCall message capture is enabled
2. An item without any pantry message configured is available on the menu

**Step(s):**
1. Order the item without a pantry message and fire it to the kitchen
2. Perform the dish up operation for the item
3. Check the DishUpCall message payload sent to VOYT

**Test Result(s):**
1. The pantryMessage fields are not present in the payload when no pantry message is applied to the item
2. No field carries the literal value null

#### 1.2.Pantry message applied - values are not enclosed by brackets
**Prerequisite(s):**
1. An item with pantry messages configured for several languages is available
2. DishUpCall message capture is enabled

**Step(s):**
1. Order the item, select the pantry message and fire it
2. Perform the dish up operation and check the payload

**Test Result(s):**
1. Each pantry message language field contains the plain message text
2. No language value is enclosed by square brackets

#### 1.3.Language mapping follows the Language Configuration
**Prerequisite(s):**
1. The Language Configuration assigns specific languages to the language slots (e.g. Japanese in language 4)
2. Pantry messages in multiple languages are configured accordingly

**Step(s):**
1. Configure the Language Configuration with Japanese in language 4 and other languages in their assigned slots
2. Order the item with pantry messages, fire it and perform dish up
3. Check each language slot in the payload

**Test Result(s):**
1. Language 4 carries the Japanese text as configured in the Language Configuration
2. Language 2 carries its own configured language text and is not wrongly set to Japanese
3. Every language slot matches the language assigned in the configuration

### 2.Print Queue Code Fields

#### 2.1.itemPrintQCode1 contains the print queue code instead of the print queue ID
**Prerequisite(s):**
1. An item is configured with a print queue that has both an ID and a code
2. DishUpCall message capture is enabled

**Step(s):**
1. Order the item, fire it and perform dish up
2. Check itemPrintQCode1 in the payload

**Test Result(s):**
1. itemPrintQCode1 contains the print queue code as required by VOYT
2. The print queue ID is no longer sent in this field

#### 2.2.itemPrintQCode2 to itemPrintQCode10 are not filled when the item has no additional print queues
**Prerequisite(s):**
1. An item with only one print queue and no additional queues is available

**Step(s):**
1. Order the item, fire it and perform dish up
2. Check itemPrintQCode2 to itemPrintQCode10 in the payload

**Test Result(s):**
1. No zero value is set in itemPrintQCode2 to itemPrintQCode10 when the item has no print queues
2. The unused print queue code fields are absent from the payload

### 3.Empty and Unused Fields Removal

#### 3.1.Non-mandatory fields without value are removed from the payload
**Prerequisite(s):**
1. DishUpCall message capture is enabled

**Step(s):**
1. Dish up an item that has no values for several non-mandatory fields
2. Inspect the payload for empty fields

**Test Result(s):**
1. Non-mandatory fields without a value are not present in the payload
2. Only fields with actual values and mandatory fields are transmitted

#### 3.2.Language fields not in use are removed from the payload
**Prerequisite(s):**
1. The property uses fewer languages than the five language slots (e.g. only language 1 and language 2 are configured)

**Step(s):**
1. Dish up an item with a pantry message
2. Inspect the language fields in the payload

**Test Result(s):**
1. The language slots that are not in use are not present in the payload
2. Only the configured languages are transmitted

### 4.DishUpCall Flow Regression

#### 4.1.Dish up call message is delivered with correct item data
**Prerequisite(s):**
1. VOYT integration is active and message capture is enabled

**Step(s):**
1. Order multiple items with different configurations (with and without pantry message, with and without additional print queues)
2. Fire and dish up the items one by one

**Test Result(s):**
1. A DishUpCall message is delivered to VOYT for each dish up operation
2. Item data such as item code, item name and quantity remains correct in the message

#### 4.2.Multi-language station configuration regression
**Prerequisite(s):**
1. A station configured with multiple active languages is available

**Step(s):**
1. Perform dish up operations from the multi-language station
2. Check the pantryMessage language values in the payload

**Test Result(s):**
1. All configured languages are populated in the correct slots without brackets or mis-mapped languages
2. The VOYT side receives and displays the data correctly
