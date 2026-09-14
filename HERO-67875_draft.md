# POS Feature - Digital Dine - Support Prepaid Amount and Tips display on Table Card, Check Total, Ordering Basket, and printed documents (POS Dev) Test Report

## Functional Testing

### 1.Configuration for Prepaid Display

#### 1.1.Display Check Extra Information in Ordering Basket Configuration

**Prerequisite(s):**
1. Access to the Infrasys POS platform with configuration rights
2. Target shop, outlet and station exist in the system

**Step(s):**
1. Go to POS System > Ordering Setup > Config by Location and select Display Check Extra Information in Ordering Basket
2. Click Add New and set Apply To to the target scope (All Locations, Shop, Outlet or Station), filling in the corresponding Shop, Outlet or Station fields
3. Click Add Row, set Display Information to Pre-Paid and Pre-Paid Tips, then click Save

**Test Result(s):**
1. The configuration record is saved successfully with the selected scope and display information
2. The Pre-Paid and Pre-Paid Tips entries appear in the Display Check Extra Information in Ordering Basket record list

#### 1.2.Switch Check Info Setting for Pre-Paid and Pre-Paid Tips

**Prerequisite(s):**
1. Access to the Infrasys POS platform with configuration rights

**Step(s):**
1. Go to POS System > Ordering Setup > Config by Location and select Switch Check Info Setting
2. Click Add New and set Apply To together with the target Shop, Outlet or Station as needed
3. Set Pre-Paid = Yes and Pre-Paid Tips = Yes, then click Save

**Test Result(s):**
1. The switch check info record is saved with Pre-Paid and Pre-Paid Tips enabled for the target scope
2. The setting takes effect on POS stations within the configured scope

#### 1.3.Check Info Authorities Visibility Control

**Prerequisite(s):**
1. Access to the Infrasys POS platform with authority control rights
2. A rule under Variable exists in Set Check Info Authorities

**Step(s):**
1. Go to POS System > Authority Control > Set Check Info Authorities
2. Click the edit icon of the target rule under Variable
3. Set Visibility to Yes for Pre-Paid and Pre-Paid Tips and save, then set Visibility to No and save again
4. Check the prepaid display on the POS for both settings

**Test Result(s):**
1. With Visibility = Yes, prepaid amount and tips are visible according to the configuration
2. With Visibility = No, the prepaid information is hidden on the POS screens

### 2.Prepaid Display on Table Floor Plan and Card View

**Prerequisite(s):**
1. Prepaid configuration and authorities are enabled for the target location
2. A table has Digital Dine default payments (prepaid amount and prepaid tips) not yet converted to check payments

**Step(s):**
1. Open the table floor plan and locate the table card with prepaid records
2. Check the prepaid amount total and prepaid tips total displayed on the card
3. Switch to the card view and check the same table

**Test Result(s):**
1. The default payment total and default payment tips total are displayed on the table card in both floor plan and card view
2. The displayed totals match the sum of the table default payments and default payment tips

### 3.Prepaid Display in Ordering Basket Extra Information

**Prerequisite(s):**
1. Display Check Extra Information in Ordering Basket is configured with Pre-Paid and Pre-Paid Tips for the station
2. A table has default payments not yet converted to check payments

**Step(s):**
1. Open the ordering basket for the table with prepaid records
2. Check the extra information area of the basket

**Test Result(s):**
1. The prepaid amount and prepaid tips are displayed in the ordering basket extra information
2. The values match the table default payment totals

### 4.Prepaid Display in Whole Check View

**Prerequisite(s):**
1. Prepaid configuration and authorities are enabled
2. A check contains default payments not yet converted to check payments

**Step(s):**
1. Open the whole check view for the check with default payments
2. Check the check total area for prepaid information

**Test Result(s):**
1. The default payment total and default payment tips total are displayed in the whole check view
2. The values are consistent with the table card and ordering basket display

### 5.Default Payments Converted to Check Payments Are Excluded

**Prerequisite(s):**
1. A table has default payments with prepaid amount and tips displayed
2. The cashier module is available

**Step(s):**
1. Note the prepaid amount and prepaid tips totals displayed for the table
2. Add the default payments to the cashier so that they become check payments
3. Re-check the table card, ordering basket and whole check view

**Test Result(s):**
1. The default payments that became check payments are no longer counted towards the default payment total and default payment tips
2. The prepaid display reflects only the remaining default payments that have not been converted

### 6.Printing Variables on Guest Check Receipt

#### 6.1.Prepaid Printing Variables Print Correct Values

**Prerequisite(s):**
1. A print format for Guest Check Receipt is configured with the PrePaidAmount and PrePaidTips printing variables
2. A table has default payments not yet converted to check payments

**Step(s):**
1. Go to POS System > Printing Setup > Print Formats and confirm the variables are added to the check or receipt format
2. Print the guest check receipt for the table with default payments
3. Compare the printed values with the prepaid totals shown on the POS

**Test Result(s):**
1. The receipt prints the default payment amount total and default payment tips total at the positions of the PrePaidAmount and PrePaidTips variables
2. The printed values match the prepaid totals displayed on the POS screens

#### 6.2.Prepaid Variables Are Available for the Guest Check Receipt Slip

**Prerequisite(s):**
1. Access to the Infrasys POS platform print format editor

**Step(s):**
1. Open POS System > Printing Setup > Print Formats and add or edit a record
2. On the Print Format tab, check the loop and slip availability of the PrePaidAmount and PrePaidTips variables
3. Try to add the variables to slip types other than the Guest Check Receipt

**Test Result(s):**
1. PrePaidAmount and PrePaidTips are available for the Guest Check Receipt slip with no loop belonged to
2. The variables are not offered for slip types outside the Guest Check Receipt

### 7.Configuration Scope Is Honoured

**Prerequisite(s):**
1. Prepaid display configuration is created with Apply To = Station for one specific station only
2. A second station in the same outlet is available

**Step(s):**
1. Open the table floor plan and ordering basket on the configured station and check the prepaid display
2. Open the same views on the second station that is not covered by the configuration

**Test Result(s):**
1. The configured station shows the prepaid amount and prepaid tips according to the configuration
2. The station outside the configuration scope does not show the prepaid extra information
