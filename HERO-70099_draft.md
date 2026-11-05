# HERO-70099 Test Report

A user reported that the Table Fee by Date access right is set for a general user, but when the user clicks to view a record on the Table Fee by Date page, a no permission message is shown on the view page. The access right checking was incorrect, and the same access right checking is required when the user clicks into the view page.

## Functional Testing

### 1. No Permission Error Is Shown on the View Page for a User With the Table Fee by Date Access Right

**Prerequisite(s):**
1. A user with administrator access to the admin page is available.
2. A general user account exists for the test.
3. Table Fee by Date records exist in TMS.

**Step(s):**
1. Go to the admin page > User Management > Access Control > TMS module.
2. Tick the Table Fee by Date access right for the general user and save.
3. Log in as the general user and open the Table Fee by Date page.
4. Click on a table fee record to open the view page.

**Reproduction Result(s):**
1. The Table Fee by Date listing page is displayed for the general user.
2. Clicking the record shows a no permission message on the view page.
3. The detail of the table fee record is not displayed.

**Fix Result(s):**
1. The access right checking uses the correct access right.
2. The view page opens and displays the detail of the selected table fee record.
3. No permission error is shown to the general user.

### 2. Table Fee by Date Access Right Configuration

#### 2.1. Table Fee by Date Read Access Right Is Granted and Saved

**Prerequisite(s):**
1. A user with administrator access to the admin page is available.
2. A general user account exists for the test.

**Step(s):**
1. Go to the admin page > User Management > Access Control > TMS module.
2. Tick the read access right of Table Fee by Date for the general user.
3. Save the access control setting.
4. Reopen the Access Control > TMS module setting of the same general user.

**Reproduction Result(s):**
1. Before the fix, the access control setting can be saved, but the saved right is checked against the wrong access right when the Table Fee by Date page is opened.
2. The general user receives a no permission error on the view page although the right is granted.

**Fix Result(s):**
1. The Table Fee by Date access right is saved successfully for the general user.
2. The access control setting shows the right as ticked after reopening the page.
3. The saved right is applied when the general user opens the Table Fee by Date listing page and the view page.

#### 2.2. Table Fee by Date Read Access Right Is Removed and Saved

**Prerequisite(s):**
1. A user with administrator access to the admin page is available.
2. The Table Fee by Date access right is currently granted to the general user.
3. The general user is able to log in.

**Step(s):**
1. Go to the admin page > User Management > Access Control > TMS module.
2. Untick the Table Fee by Date access right for the general user.
3. Save the access control setting.
4. Reopen the setting and confirm that the right is unticked.

**Reproduction Result(s):**
1. Before the fix, the removal of the right is saved, but the incorrect access right checking keeps the permission result of the page inconsistent with the saved setting.
2. The page does not reflect the saved access control setting reliably.

**Fix Result(s):**
1. The removal of the Table Fee by Date access right is saved successfully.
2. The setting shows the right as unticked after reopening the page.
3. The saved setting is applied to the general user when the Table Fee by Date pages are opened.

### 3. Access Right Enforcement on Table Fee by Date

#### 3.1. Listing Page Opens for a User With the Table Fee by Date Access Right

**Prerequisite(s):**
1. The Table Fee by Date access right is granted to the general user and saved.
2. The general user is able to log in.

**Step(s):**
1. Log in as the general user.
2. Go to the Table Fee by Date page.
3. Observe the listing and the available actions.

**Reproduction Result(s):**
1. Before the fix, the listing page is displayed to the general user with the granted right.
2. The listing remains accessible, but the following view page is blocked by the incorrect checking.

**Fix Result(s):**
1. The Table Fee by Date listing page is displayed without a permission error.
2. The table fee records are listed.
3. The page can be opened repeatedly without a permission error.

#### 3.2. View Page Opens for a User With the Table Fee by Date Access Right

**Prerequisite(s):**
1. The Table Fee by Date access right is granted to the general user and saved.
2. Table Fee by Date records exist.
3. The general user is able to log in.

**Step(s):**
1. Log in as the general user and go to the Table Fee by Date listing page.
2. Click a table fee record to open the view page.
3. Check the record detail shown on the view page.
4. Return to the listing page and open another record.

**Reproduction Result(s):**
1. Before the fix, a no permission message is shown when the record is opened from the listing page.
2. The record detail is not displayed for the general user with the granted right.

**Fix Result(s):**
1. The same access right checking is applied when the user clicks into the view page.
2. The view page opens and shows the detail of the selected record.
3. No permission error is shown, and other records can be opened in the same way.

#### 3.3. Listing Page Shows a Permission Error When the Right Is Removed

**Prerequisite(s):**
1. The Table Fee by Date access right is removed from the general user and saved.
2. The general user is able to log in.

**Step(s):**
1. Log in as the general user.
2. Go to the Table Fee by Date page.
3. Observe the page displayed.

**Reproduction Result(s):**
1. Before the fix, the permission result on the Table Fee by Date page does not follow the saved access control setting because the wrong access right is checked.
2. The behavior of the page is inconsistent with the unticked right.

**Fix Result(s):**
1. The user without the Table Fee by Date access right receives a no permission error on the Table Fee by Date page.
2. No table fee record is listed for the user.
3. The page follows the saved access control setting.

#### 3.4. View Page Shows a Permission Error When the Right Is Removed

**Prerequisite(s):**
1. The Table Fee by Date access right is removed from the general user and saved.
2. Table Fee by Date records exist.
3. The general user is able to log in.

**Step(s):**
1. Log in as the general user.
2. Go to the Table Fee by Date page and observe the result.
3. Attempt to open a table fee record from the page or from a previously bookmarked view page.

**Reproduction Result(s):**
1. Before the fix, the view page does not apply the same access right checking as the listing page.
2. The permission result of the view page is inconsistent with the removed access right.

**Fix Result(s):**
1. The view page applies the same access right checking and shows a no permission error.
2. No table fee record detail is displayed.
3. The result follows the saved access control setting.

### 4. TMS Access Control Regression

#### 4.1. Other TMS Access Rights of the General User Remain Unaffected

**Prerequisite(s):**
1. The general user has other TMS access rights granted and saved.
2. A user with administrator access to the admin page is available.

**Step(s):**
1. Record the other TMS access rights of the general user in Access Control > TMS module.
2. Grant or remove the Table Fee by Date access right and save.
3. Reopen the Access Control > TMS module setting of the general user.
4. Log in as the general user and open the pages controlled by the other TMS access rights.

**Reproduction Result(s):**
1. Before the fix, the other TMS access rights are saved and applied, while the Table Fee by Date checking returns a wrong permission result.
2. The inconsistent result is limited to the Table Fee by Date pages.

**Fix Result(s):**
1. The other TMS access rights remain unchanged after saving the Table Fee by Date setting.
2. The pages controlled by the other TMS access rights behave as before.
3. Only the Table Fee by Date permission result is corrected.

#### 4.2. Table Fee by Date Listing Content Is Unchanged for the Authorized User

**Prerequisite(s):**
1. The Table Fee by Date access right is granted to the general user and saved.
2. Table Fee by Date records are prepared and their dates, period types and fee amounts are recorded.

**Step(s):**
1. Log in as the general user and open the Table Fee by Date listing page.
2. Compare the listed records with the recorded dates, period types and fee amounts.
3. Open the view page of a record and compare the detail with the recorded values.

**Reproduction Result(s):**
1. Before the fix, the listing content is correct, but the view page is blocked by the incorrect access right checking.
2. The record detail cannot be compared because the view page is not displayed.

**Fix Result(s):**
1. The listing shows the same records with unchanged dates, period types and fee amounts.
2. The view page shows the detail that matches the recorded values.
3. The access right fix does not change the table fee data.

## Test Environment

Testing is performed on HKJC TMS on Infrasys Cloud with the admin page, User Management > Access Control > TMS module, and the Table Fee by Date listing and view pages available. The environment includes an administrator account, a general user account for the access right tests, and existing Table Fee by Date records whose dates, period types and fee amounts are recorded before the test.
