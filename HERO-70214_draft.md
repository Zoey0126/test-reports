# HERO-70214 Test Report

On the HKJC TMS Operation reservation screen, confirming a special amenity updated the selected-item tag with the price and quantity, but the Special Amenities additional charge stayed at the amount already on the reservation. When that category had no charge, the box stayed at 0.00 and the save action stored that 0.00 category total, while the service line price was still stored.

## Functional Testing

### 1. Special Amenities Additional Charge Stays 0.00 After Confirming on Edit Reservation

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation without any Special Amenities additional charge exists and is opened in Edit Reservation.
3. A chargeable special amenity with unit price $10.00 is available for selection.

**Step(s):**
1. Open the existing reservation in Edit Reservation.
2. Add Special Amenities and select the chargeable special amenity with unit price $10.00 and quantity 1.
3. Click "Confirm" to close the service dialog.
4. Observe the selected-item tag and the Special Amenities additional charge box.
5. Save the reservation.
6. Reopen the reservation in Edit Reservation and check the saved Special Amenities category total and the service line price.

**Reproduction Result(s):**
1. The selected-item tag shows the price and quantity of the amenity (unit price $10.00 x1 = $10.00).
2. The Special Amenities additional charge box and its total stay at 0.00.
3. The save action stores 0.00 as the Special Amenities category total, while the service line price is stored.
4. After reopening the reservation, the tag shows the amenity price and the charge box still shows 0.00.

**Fix Result(s):**
1. After confirming the amenity, the Special Amenities additional charge box shows 10.00.
2. The amount 10.00 is included in the reservation total charge.
3. The save action stores 10.00 as the Special Amenities category total.
4. After reopening the reservation, both the charge box and the saved category total show 10.00.

### 2. Special Amenities Additional Charge on Edit Reservation

#### 2.1. Charge Box Shows the Selected Amount After the Dialog Closes

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation exists and is opened in Edit Reservation.
3. A chargeable special amenity with unit price $10.00 is available for selection.

**Step(s):**
1. Open the reservation in Edit Reservation.
2. Add Special Amenities and select the chargeable special amenity with unit price $10.00 and quantity 1.
3. Click "Confirm".
4. Observe the Special Amenities additional charge box and the reservation total charge.

**Reproduction Result(s):**
1. Before the fix, the charge box keeps the amount loaded with the reservation and stays at 0.00 when the category had no charge.
2. Only the selected-item tag is refreshed with the amenity price and quantity.

**Fix Result(s):**
1. The category-charge refresh runs after the service dialog is closed.
2. The Special Amenities additional charge box shows 10.00.
3. The reservation total charge includes the selected amount.

#### 2.2. Charge Amount Is Calculated from Unit Price Times Quantity

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation without any Special Amenities additional charge exists and is opened in Edit Reservation.
3. A chargeable special amenity with unit price $10.00 is available for selection.

**Step(s):**
1. Open the reservation in Edit Reservation.
2. Add Special Amenities and select the chargeable special amenity with unit price $10.00 and quantity 2.
3. Click "Confirm".
4. Observe the selected-item tag, the Special Amenities additional charge box and the reservation total charge.
5. Save the reservation and reopen it in Edit Reservation.

**Reproduction Result(s):**
1. Before the fix, the selected-item tag shows the quantity and the price of the two units, but the charge box and its total stay at 0.00.
2. The save action stores the 0.00 category total.

**Fix Result(s):**
1. The selected-item tag shows the amenity price for the selected quantity.
2. The Special Amenities additional charge shows 20.00, which is the unit price $10.00 multiplied by quantity 2.
3. The saved Special Amenities category total is 20.00 and it is included in the reservation total charge.

#### 2.3. Charge Box Replaces the Amount Already Loaded on the Reservation

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation already carrying a Special Amenities additional charge of $20.00 exists and is opened in Edit Reservation.
3. A chargeable special amenity with unit price $10.00 is available for selection.

**Step(s):**
1. Open the reservation in Edit Reservation and confirm the existing Special Amenities additional charge of 20.00.
2. Add Special Amenities and select the chargeable special amenity with unit price $10.00 and quantity 1.
3. Click "Confirm".
4. Observe the Special Amenities additional charge box and the reservation total charge.
5. Save the reservation and reopen it in Edit Reservation.

**Reproduction Result(s):**
1. Before the fix, the charge box keeps the amount already loaded with the reservation instead of recalculating it.
2. The total remains at the previously loaded amount and the new selection is not reflected in the category charge.

**Fix Result(s):**
1. The Special Amenities additional charge box is recalculated after the dialog closes.
2. The charge box and the reservation total charge reflect the selected amenity amount.
3. The saved Special Amenities category total matches the recalculated charge.

### 3. Special Amenities Additional Charge on New Reservation

#### 3.1. Charge Box Shows the Selected Amount on New Reservation

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A chargeable special amenity with unit price $10.00 is available for selection.
3. No reservation charge total is preloaded for the selected category.

**Step(s):**
1. Create a new reservation on the TMS Operation reservation screen.
2. Add Special Amenities and select the chargeable special amenity with unit price $10.00 and quantity 1.
3. Click "Confirm".
4. Observe the selected-item tag, the Special Amenities additional charge box and the reservation total charge.

**Reproduction Result(s):**
1. Before the fix, the selected-item tag is refreshed with the amenity price and quantity while the charge box stays at 0.00.
2. The confirmation closes the service dialog without running the category-charge refresh.

**Fix Result(s):**
1. The category-charge refresh runs after the service dialog closes on New Reservation.
2. The Special Amenities additional charge box shows 10.00.
3. The amount is included in the reservation total charge.

#### 3.2. Category Total Is Saved on New Reservation

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A chargeable special amenity with unit price $10.00 is available for selection.

**Step(s):**
1. Create a new reservation and add Special Amenities with the chargeable special amenity, unit price $10.00 and quantity 1.
2. Click "Confirm" and confirm the charge box shows 10.00.
3. Save the reservation.
4. Reopen the saved reservation in Edit Reservation and check the Special Amenities category total.

**Reproduction Result(s):**
1. Before the fix, the save action writes 0.00 as the Special Amenities category total.
2. The service line price is stored, so the selected-item tag still shows the price after reopening.

**Fix Result(s):**
1. The selected amount 10.00 is saved as the Special Amenities category total.
2. The saved category total and the service line price are consistent after reopening the reservation.
3. The reservation total charge includes the saved category total.

### 4. Other Charges on the Reservation Screen

#### 4.1. Other Self-Selected Services Are Recalculated After Confirm

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation exists and is opened in Edit Reservation.
3. A chargeable self-selected service that is configured on the same service dialog as special amenities is available for selection.

**Step(s):**
1. Open the reservation in Edit Reservation.
2. Add the self-selected service on the same service dialog and select a chargeable option.
3. Click "Confirm" to close the dialog.
4. Observe the additional charge box and the reservation total charge of the related category.

**Reproduction Result(s):**
1. Before the fix, the confirm action closes the dialog without running the category-charge refresh, so the category charge stays at the loaded amount.
2. Only the selected-item tag is updated with the selected option.

**Fix Result(s):**
1. The same confirm action runs the category-charge refresh for other self-selected services on that dialog.
2. The related category charge shows the selected amount.
3. The amount is included in the reservation total charge and is saved as the category total.

#### 4.2. Car Park and Coach Charges Are Unchanged After Confirming Special Amenities

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation with car park, coach go, and coach return charges exists and is opened in Edit Reservation.
3. The car park, coach go, and coach return charges are recorded before the test.

**Step(s):**
1. Open the reservation in Edit Reservation and record the car park, coach go, and coach return charges.
2. Add Special Amenities and select a chargeable special amenity.
3. Click "Confirm".
4. Compare the car park, coach go, and coach return charges with the recorded values.
5. Save the reservation and reopen it in Edit Reservation.

**Reproduction Result(s):**
1. Before the fix, the Special Amenities additional charge is not recalculated, while car park, coach go, and coach return continue to recalculate after confirm.
2. The car park, coach go, and coach return charges already show the correct amounts.

**Fix Result(s):**
1. Car park, coach go, and coach return charges are unchanged after confirming the special amenity.
2. The three charges recalculate as before and remain correct in the reservation total charge.
3. The saved amounts are unchanged after reopening the reservation.

#### 4.3. Table Fee Still Recalculates When the Table Changes

**Prerequisite(s):**
1. The user is logged in to HKJC TMS with access to the TMS Operation reservation screen.
2. A reservation with a table assigned exists and is opened in Edit Reservation.
3. Table fees are configured for the table types used in the test.

**Step(s):**
1. Open the reservation in Edit Reservation and record the current table fee.
2. Add Special Amenities with a chargeable special amenity and click "Confirm".
3. Change the table of the reservation to a table with a different table fee.
4. Observe the table fee charge and the Special Amenities additional charge.
5. Save the reservation.

**Reproduction Result(s):**
1. Before the fix, the table fee recalculates when the table changes, while the Special Amenities additional charge stays at the loaded amount.
2. The reservation total charge keeps the previous Special Amenities amount.

**Fix Result(s):**
1. The table fee still recalculates when the table changes.
2. The Special Amenities additional charge keeps the selected amount and both charges are included in the reservation total charge.
3. The saved reservation stores the correct table fee and Special Amenities category total.

## Test Environment

Testing is performed on the HKJC TMS Operation reservation screen in HKJC TMS on Infrasys Cloud, with New Reservation and Edit Reservation available to the logged-in user. Chargeable special amenities with a known unit price, table fees configured for the table types used in the test, and reservation records containing car park, coach go, and coach return charges are prepared in the test environment. No additional setup is required for the fix.
