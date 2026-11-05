# HERO-70044 Test Report

In the table.change event of Push Notification, the table number format of the from_table_no and to_table_no fields should be consistent. During a Table Change operation on a check whose table number starts with a letter, the from_table_no field kept a leading zero while the to_table_no field removed it, so the two fields used different formats. The fix is delivered in the current version, and no extra zero is appended before the from table number.

## Functional Testing

### 1. From and To Table Numbers in the Table Change Event

#### 1.1. Table Change from Table V11 to Table V10 Sends Consistent Table Numbers

**Prerequisite(s):**
1. The POS client is available and a push notification configuration with the table.change event is enabled.
2. An old check with table number V11 exists.
3. The table.change event payload can be inspected.

**Step(s):**
1. Open the old check with table number V11 in the POS client.
2. Click function "Change Table".
3. Enter the target table number V10.
4. Click button "OK".
5. Inspect the from_table_no and to_table_no fields in the table.change event of Push Notification.

**Reproduction Result(s):**
1. Before the fix, from_table_no was set to "0V11", while to_table_no was set to "V10".
2. The two fields used different formats because an extra leading zero was kept in the from table number.

**Fix Result(s):**
1. from_table_no is set to "V11".
2. to_table_no is set to "V10".
3. Both fields use the same format and no extra leading zero is appended before the table number.

#### 1.2. Table Change Between Table Numbers with Another Non-Numeric Prefix

**Prerequisite(s):**
1. A push notification configuration with the table.change event is enabled.
2. An old check with a table number starting with another non-numeric character, for example A11, exists.
3. A target table with table number A10 exists.

**Step(s):**
1. Open the old check with table number A11 in the POS client.
2. Click function "Change Table".
3. Enter the target table number A10.
4. Click button "OK".
5. Inspect the from_table_no and to_table_no fields in the table.change event of Push Notification.

**Reproduction Result(s):**
1. Before the fix, from_table_no was set to "0A11" while to_table_no was set to "A10".
2. The extra leading zero was kept in the from table number when the table number started with a non-numeric character.

**Fix Result(s):**
1. from_table_no is set to "A11".
2. to_table_no is set to "A10".
3. No extra leading zero is appended before the from table number.

#### 1.3. Table Change Between Numeric Table Numbers Sends Consistent Table Numbers

**Prerequisite(s):**
1. A push notification configuration with the table.change event is enabled.
2. An old check with numeric table number 11 exists.
3. A target table with numeric table number 10 exists.

**Step(s):**
1. Open the old check with table number 11 in the POS client.
2. Click function "Change Table".
3. Enter the target table number 10.
4. Click button "OK".
5. Inspect the from_table_no and to_table_no fields in the table.change event of Push Notification.

**Reproduction Result(s):**
1. Before the fix, the leading zero issue was observed when the table number starts with a non-numeric character.
2. The numeric table number flow is included as a regression check on the table.change event format.

**Fix Result(s):**
1. from_table_no is set to "11".
2. to_table_no is set to "10".
3. Both fields use the same format without any extra leading zero.

### 2. Regression on the Change Table Operation

#### 2.1. Check Is Moved to the Target Table with Content Unchanged

**Prerequisite(s):**
1. An old check with table number V11 and several items exists.
2. The target table V10 is not occupied by another check.

**Step(s):**
1. Open the old check with table number V11 in the POS client.
2. Click function "Change Table".
3. Enter the target table number V10 and click button "OK".
4. Open the check under table V10.
5. Check the table status of V11 in the POS client.

**Reproduction Result(s):**
1. Before the fix, the check was moved to the target table normally.
2. The reported defect affected only the from table number format in the table.change event of Push Notification.

**Fix Result(s):**
1. The check is displayed under the target table V10.
2. The check items and the check total are unchanged after the table change.
3. The source table V11 becomes vacant.

#### 2.2. Change Table Back to the Original Table Sends a Consistent Event

**Prerequisite(s):**
1. A check has been changed from table V11 to table V10.
2. A push notification configuration with the table.change event is enabled.

**Step(s):**
1. Open the check under table V10 in the POS client.
2. Click function "Change Table".
3. Enter the original table number V11 and click button "OK".
4. Inspect the from_table_no and to_table_no fields in the table.change event of Push Notification.

**Reproduction Result(s):**
1. Before the fix, the same extra leading zero was kept in from_table_no when the check was changed back to a table starting with a letter.
2. The from table number and the to table number used different formats for the second table change.

**Fix Result(s):**
1. from_table_no is set to "V10".
2. to_table_no is set to "V11".
3. Both fields use the same format with no extra leading zero.

## Test Environment

| Item | Details |
|------|---------|
| JIRA Ticket | HERO-70044 |
| Summary | Push Notification - Extra leading zero is not removed from the "From Table Number" field during table change |
| Issue Type | Bug |
| Test Target | Table Change function in the POS client and the table.change event of Push Notification |
| Test Client | Infrasys POS client |
| Data Setup | Old checks with table numbers V11, A11 and 11, and free target tables V10, A10 and 10 |
| New Setup | None |
| Verification Points | from_table_no and to_table_no format in the table.change event for letter-prefixed and numeric table numbers; check content after table change; repeated table change |
