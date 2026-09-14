# POS - LPS Debit Card - Mask balances and show "Card is not valid" for invalid cards Test Report

## Functional Testing

### 1.Card Status and Message Display

#### 1.1.Normal status display remains unchanged
**Prerequisite(s):**
1. The LPS Debit Card loyalty interface is configured and connected to the middleware
2. A card with status Normal is available
3. The Mask Check Value Result Balance option is set to Yes

**Step(s):**
1. Open a check and click the function Loyalty SVC Check Value
2. Swipe the valid card or input the card number
3. Observe the enquiry result screen

**Test Result(s):**
1. The status shows Normal as before
2. Card Balance, Gift Balance and Total Balance display their raw values unchanged
3. No masking and no additional invalid message are applied

#### 1.2.Expired card shows Expired (Card is not valid) with masked balances
**Prerequisite(s):**
1. The LPS Debit Card interface is configured
2. A card that returns isExpired = true is available
3. The Mask Check Value Result Balance option is set to Yes

**Step(s):**
1. Open a check and click the function Loyalty SVC Check Value or Loyalty SVC Check Value (Key-in)
2. Swipe the expired card or key in its card number
3. Observe the enquiry result screen

**Test Result(s):**
1. The status shows Expired (Card is not valid) in the English UI, or Expired (此卡已失效) in the Traditional Chinese UI
2. Card Balance, Gift Balance and Total Balance display as ***
3. The user is not misled by any raw balance value

#### 1.3.Expired flag takes precedence over other non-Normal status
**Prerequisite(s):**
1. The Mask Check Value Result Balance option is set to Yes
2. The middleware can simulate a card that returns isExpired = true together with a non-Normal status such as PendingSales

**Step(s):**
1. Trigger an enquiry where the middleware returns isExpired = true and the status PendingSales at the same time
2. Observe the status message on the enquiry result screen

**Test Result(s):**
1. The status shows Expired (Card is not valid) and the Expired message takes precedence over the returned non-Normal status
2. Card Balance, Gift Balance and Total Balance display as ***

#### 1.4.PendingSales status shows PendingSales (Card is not valid) with masked balances
**Prerequisite(s):**
1. The Mask Check Value Result Balance option is set to Yes
2. A card that returns status PendingSales with isExpired not true is available

**Step(s):**
1. Perform an enquiry with the PendingSales card through Loyalty SVC Check Value
2. Observe the enquiry result screen

**Test Result(s):**
1. The status shows PendingSales (Card is not valid) in the English UI, or PendingSales (此卡已失效) in the Traditional Chinese UI
2. Card Balance, Gift Balance and Total Balance display as ***

#### 1.5.Other non-Normal status shows the status with the Card is not valid message
**Prerequisite(s):**
1. The Mask Check Value Result Balance option is set to Yes
2. The middleware can simulate a card returning a status that is not Normal and not PendingSales (e.g. Freeze) with isExpired not true

**Step(s):**
1. Perform an enquiry with the simulated card
2. Observe the enquiry result screen

**Test Result(s):**
1. The status shows the returned status followed by the invalid message in the format ReturnedStatus (Card is not valid) in the English UI, or ReturnedStatus (此卡已失效) in the Traditional Chinese UI
2. Card Balance, Gift Balance and Total Balance display as ***

#### 1.6.Blank status shows the invalid message without status name prefix
**Prerequisite(s):**
1. The Mask Check Value Result Balance option is set to Yes
2. The middleware can simulate a card returning a blank or empty status

**Step(s):**
1. Perform an enquiry where the middleware returns a blank status
2. Observe the enquiry result screen

**Test Result(s):**
1. The status shows only (Card is not valid) in the English UI or (此卡已失效) in the Traditional Chinese UI without any status name prefix
2. Card Balance, Gift Balance and Total Balance display as ***

### 2.Balance Masking Setup Control

#### 2.1.Masking option set to No displays raw balances for invalid cards
**Prerequisite(s):**
1. The Mask Check Value Result Balance option of the target LPS Debit Card interface is set to No (default value)
2. A card with an invalid status (non-Normal) is available

**Step(s):**
1. Go to Interface Control > Interfaces, select the target LPS Debit Card interface and confirm Mask Check Value Result Balance is set to No under General Setup
2. Perform a card enquiry with the invalid card
3. Observe the status message and the balance values

**Test Result(s):**
1. The status message is displayed as described for the invalid status (status followed by the Card is not valid message)
2. Card Balance, Gift Balance and Total Balance display their raw values instead of ***

### 3.Enquiry Workflow Regression

#### 3.1.Card swipe enquiry flow regression
**Prerequisite(s):**
1. The LPS Debit Card interface is configured and connected to the middleware

**Step(s):**
1. Open a check and click the function Loyalty SVC Check Value
2. Swipe a card and complete the enquiry
3. Close the enquiry screen and repeat with a card of a different status

**Test Result(s):**
1. The enquiry is triggered to the external party API and returns the card details as before
2. The result screen is displayed correctly for each card status without errors

#### 3.2.Manual key-in enquiry flow regression
**Prerequisite(s):**
1. The LPS Debit Card interface is configured and connected to the middleware

**Step(s):**
1. Open a check and click the function Loyalty SVC Check Value (Key-in)
2. Manually input a card number and complete the enquiry

**Test Result(s):**
1. The enquiry completes successfully and the status display and balance masking follow the same rules as the swipe flow

#### 3.3.Display-only change on invalid card
**Prerequisite(s):**
1. The Mask Check Value Result Balance option is set to Yes
2. An invalid card is available

**Step(s):**
1. Perform an enquiry with the invalid card and observe the masked balances
2. Proceed with any allowed action on the card as before the change (e.g. continue the existing operation)

**Test Result(s):**
1. All users see the masked balances and there is no override to reveal unmasked values
2. The user can still proceed with the allowed actions as before, so the change does not block any existing operation
