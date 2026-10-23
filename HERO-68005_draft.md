# HERO-68005 Bug Fix Test Report - Wrong Check ID of Discount Reference Record After Split Check By Option

## Functional Testing

### F1.Bug Reproduction and Fix Verification - Discount Reference Check ID After Split Check By Option

#### F1.1.Verify Discount Reference Records Have Correct Check ID After Split Check By Option
**Prerequisite(s):**
1. An item discount is configured to allow discount reference: POS System > Discount Setup > Discount Type > Restrictions > Allow Reference input = Allow 2 Discount References, with reference names entered
2. The item discount is available on the POS station
3. The split check function is available

**Step(s):**
1. Open a new check
2. Order an item and apply the target item discount
3. Input two discount references and click "OK"
4. Click the function "Send Check"
5. Open the target check and click the function "Split Check By Option"
6. Select the option "Equal quantity" and input 2 in the input box
7. Click "Enter", then "OK", then "Yes"
8. Click "Print All"
9. Review the check ID of the two discount reference records of the split checks

**Reproduction Result(s):**
1. The two discount reference records are stored with the same check ID
2. One discount reference record has the correct check ID while the other record has an incorrect check ID
3. The discount reference extra info of the split check is linked to the wrong check

**Fix Result(s):**
1. Each discount reference record is stored with the correct check ID of its own split check
2. No discount reference record retains the check ID of the original check
3. The discount reference extra info is linked to the correct split check

#### F1.2.Verify Discount Reference Check ID for Unequal Split Quantity
**Prerequisite(s):**
1. An item discount with 2 allowed discount references is configured
2. The ordered item has a quantity greater than 2

**Step(s):**
1. Open a new check and order the item
2. Apply the target item discount with two discount references
3. Click "Send Check"
4. Open the check and click "Split Check By Option"
5. Split the items by a quantity that is not equal to half of the total quantity
6. Click "OK", "Yes" and "Print All"
7. Review the check ID of every discount reference record in the split checks

**Reproduction Result(s):**
1. At least one discount reference record is stored with the incorrect check ID
2. The discount reference records are not consistently linked to the split checks

**Fix Result(s):**
1. Every discount reference record has the check ID of the split check it belongs to
2. The discount reference records are consistently linked to the correct split checks
3. The discount amount and reference details are correct in each split check

#### F1.3.Verify Discount Reference Records With Single Reference
**Prerequisite(s):**
1. An item discount is configured with only 1 allowed discount reference
2. The ordered item is eligible for the item discount

**Step(s):**
1. Open a new check and order the item
2. Apply the item discount with one discount reference
3. Click "Send Check"
4. Split the check by option and save the split checks
5. Review the check ID of the discount reference record

**Reproduction Result(s):**
1. The single discount reference record may be stored with the incorrect check ID after the split
2. The discount reference record is not linked to the split check correctly

**Fix Result(s):**
1. The single discount reference record is stored with the correct check ID
2. The discount reference record is linked to the correct split check

#### F1.4.Verify Split Check By Option Without Discount Reference
**Prerequisite(s):**
1. An item without any discount reference is ordered in a check

**Step(s):**
1. Open a new check and order items without discount reference
2. Click "Send Check"
3. Click "Split Check By Option" and split the check
4. Review the item and check data of the split checks

**Reproduction Result(s):**
1. The split workflow is completed normally for items without discount reference
2. The check ID of the split checks is correct

**Fix Result(s):**
1. The split workflow is completed normally without regression
2. The item, discount and check ID data of the split checks are correct

#### F1.5.Verify Discount Reference Detail Display After the Fix
**Prerequisite(s):**
1. A check with discount reference records has been split by option and saved
2. The check detail and the discount reference report are available

**Step(s):**
1. Open each split check and open the item discount detail
2. Review the discount reference names and values
3. Open the discount reference record from the check detail
4. Compare the check ID displayed with the split check number

**Reproduction Result(s):**
1. The discount reference detail links to the wrong check ID for one record
2. The reference detail cannot be traced back to the correct split check

**Fix Result(s):**
1. The discount reference detail displays the correct reference names and values
2. The check ID displayed in the reference detail matches the split check number
3. The reference detail can be traced back to the correct split check

### 2.Regression Scope

The following related modules and scenarios must be retested after the change:

- Discount Type setup for Allow 1/2/3 Discount References and reference names
- Item Discount application with discount reference on a normal check
- Split Check By Option with equal quantity and unequal quantity
- Send Check and Print All after Split Check By Option
- Discount reference record display in the check detail and in reports
- Check level discount and item level discount reference combinations
- Void item and void check after the discount reference is applied
- Split Check By Item and Split Table functions with discounted items
