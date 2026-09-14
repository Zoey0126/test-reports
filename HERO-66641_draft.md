# POS-TMS Interface - Support to return Payment total and Room charge payment for third party CRM integration Test Report

## Functional Testing

### 1.Payment Data in Thank You Message

#### 1.1.Payment total equals the sum of all tender types
**Prerequisite(s):**
1. POS is connected to the internal TMS module and message capture is available
2. A check with a reservation attached (mark arrival done) is open

**Step(s):**
1. Settle the check with more than one tender type (e.g. cash 50.00 and credit card 20.00)
2. Capture the Thank You Message sent from POS to TMS after completion

**Test Result(s):**
1. The Thank You Message includes the new payment total field
2. The payment total equals 70.00, the sum of all tender types applied to the check
3. The check total and check number remain present and unchanged

#### 1.2.Payment details include payment code, amount and name for each payment
**Prerequisite(s):**
1. POS is connected to the internal TMS module and message capture is available

**Step(s):**
1. Settle a check with two different payment methods
2. Capture the Thank You Message payload

**Test Result(s):**
1. The payment detail list contains one entry per applied payment
2. Each entry includes the payment code, payment amount and payment type or name (e.g. code 0001 with amount 50.00 and type cash, code 0002 with amount 20.00 and type credit_card)
3. Payment tips are not included in the message

#### 1.3.Room charge payment is reported correctly
**Prerequisite(s):**
1. A reservation check that will be settled by the room charge payment method is available
2. Message capture is available

**Step(s):**
1. Settle the reservation check using the room charge payment method
2. Capture the Thank You Message payload

**Test Result(s):**
1. The amount settled via the room charge payment method is reported in the payment details
2. The payment total includes the room charge amount together with other tender types

#### 1.4.Existing fields remain unchanged
**Prerequisite(s):**
1. Message capture is available and the previous message format is known

**Step(s):**
1. Complete a reservation check and capture the Thank You Message
2. Compare the existing fields against the previous message format

**Test Result(s):**
1. Existing fields such as check total and check number keep the same field names and values
2. The new fields are additive and do not alter the existing message structure

### 2.Zero and Boundary Scenarios

#### 2.1.Zero payment total when no payment is applied
**Prerequisite(s):**
1. A check attached with a reservation can be completed without any payment applied per the existing trigger rules

**Step(s):**
1. Complete the check without applying any payment
2. Capture the Thank You Message

**Test Result(s):**
1. The message returns a zero value for the payment total when no payment is applied
2. The payment detail list contains no payment entries and no error occurs

#### 2.2.Zero room charge value when room charge payment is not used
**Prerequisite(s):**
1. A reservation check that will be settled without the room charge payment method is available

**Step(s):**
1. Settle the check using non-room-charge tender types only
2. Capture the Thank You Message

**Test Result(s):**
1. No room charge amount is reported, or the room charge related value is zero
2. The payment total reflects only the applied non-room-charge payments

#### 2.3.Multiple payments across tender types are summed correctly
**Prerequisite(s):**
1. A check on which several payments of mixed tender types (cash, credit card, room charge) can be applied

**Step(s):**
1. Apply three or more payments of different tender types to the check and settle it
2. Capture the Thank You Message

**Test Result(s):**
1. The payment total equals the sum of all applied payments
2. Each payment appears once in the details with its own code, amount and type
3. Decimal amounts are transmitted accurately without rounding errors

### 3.Message Trigger Events Regression

#### 3.1.Thank You Message fires on normal check completion with payment data
**Prerequisite(s):**
1. The existing Thank You Message trigger configuration is unchanged

**Step(s):**
1. Complete and fully settle a reservation check
2. Verify the Thank You Message is fired automatically

**Test Result(s):**
1. The Thank You Message is triggered by the existing trigger logic without any manual operator action
2. The message contains the new payment total and payment details fields

#### 3.2.Release payment, void check and adjust payment update the check total
**Prerequisite(s):**
1. A settled reservation check is available and message capture is enabled

**Step(s):**
1. Release the payment after completion and capture the Thank You Message
2. Void a settled check and capture the message
3. Adjust the payment after completion and capture the message

**Test Result(s):**
1. Each of the three operations triggers the Thank You Message and updates the check total as in the existing behavior
2. The message is delivered without error in each scenario

#### 3.3.Void payment does not update the check total
**Prerequisite(s):**
1. A settled reservation check with at least one voidable payment is available

**Step(s):**
1. Void one payment of the settled check
2. Observe the check total and the Thank You Message behavior

**Test Result(s):**
1. Voiding the payment does not update the check total, consistent with the existing behavior
2. No unexpected change is introduced by this operation

### 4.Backward Compatibility With TMS Instances Not Consuming New Fields
**Prerequisite(s):**
1. A TMS instance running the version before this enhancement is available, or a consumer that ignores unknown fields
2. Message capture is available

**Step(s):**
1. Complete a reservation check on the POS with the extended message format enabled
2. Observe how the older TMS instance processes the received message

**Test Result(s):**
1. The older TMS instance ignores the unknown new fields without error
2. The existing fields are processed as before and no message failure or data corruption occurs
