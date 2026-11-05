# HERO-69188 Test Report

Basic9 (MATE CRM) is a new membership interface for Infrasys POS. This release adds member and employee enquiry, member and employee discounts, coupon handling, voucher and point redemption, employee payment, and membership printing variables through the Basic9 middleware. The feature is available for fine dining mode in horizontal desktop view only and is not available in vertical mobile view, API portal, or multiple membership interface. Development follows the study of the 17 third-party API calls recorded in HERO-59075 under Epic HERO-47101.

## Functional Testing

### 1. Interface Availability and Restrictions

#### 1.1. SVC Enquiry Is Unavailable in Vertical Mobile View

**Prerequisite(s):**
1. Infrasys POS is running in fine dining mode.
2. The ordering screen is displayed in vertical mobile view.
3. A user with a user number assigned is logged in.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".
3. Enter a valid Basic9 card number and click "Search".

**Test Result(s):**
1. System prompts error message "Cannot perform this function in this mode".
2. The action aborts and no member enquiry screen is displayed.
3. No Basic9 API call is triggered.

#### 1.2. SVC Enquiry Is Available in Horizontal Desktop View

**Prerequisite(s):**
1. Infrasys POS is running in fine dining mode.
2. The ordering screen is displayed in horizontal desktop view.
3. The Basic9 interface is attached to the outlet and a test member exists in the middleware.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".
3. Enter a valid Basic9 card number and click "Search".

**Test Result(s):**
1. Member enquiry screen is displayed with tabs in order: Member Listing, Member Detail, Card Info, Discount, Coupon, Applied Coupon, Used Coupon.
2. Member information returned from the Basic9 middleware is displayed.
3. Point total and discount amount are shown.

#### 1.3. Basic9 Membership Function Is Not Available in API Portal

**Prerequisite(s):**
1. The Basic9 vendor and membership interface are configured in Infrasys POS.
2. API portal access is available for the outlet.

**Step(s):**
1. Access the API portal.
2. Attempt to use the Basic9 membership enquiry or membership payment function.

**Test Result(s):**
1. The Basic9 membership function is not available in the API portal.
2. No member, coupon, voucher, or point operation can be performed through the API portal.

#### 1.4. Basic9 Membership Function Is Not Available for Multiple Membership Interface

**Prerequisite(s):**
1. More than one membership interface is configured and attached to the outlet.
2. A user with a user number assigned is logged in.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".
3. Search for a Basic9 member.

**Test Result(s):**
1. The Basic9 membership function is not available when multiple membership interfaces are configured.
2. The member enquiry cannot be completed and no Basic9 member is returned.

#### 1.5. SVC Enquiry Is Available in Fine Dining Mode Only

**Prerequisite(s):**
1. The outlet is operating in an ordering mode other than fine dining.
2. A user with a user number assigned is logged in.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "Cannot perform this function in this mode".
2. The action aborts and the workflow ends.
3. The enquiry screen is not displayed.

### 2. Basic9 Interface Configuration

#### 2.1. Basic9 Vendor Record Is Saved

**Prerequisite(s):**
1. The user has access to Interface Control > Vendors.
2. Basic9 middleware connection details are available.

**Step(s):**
1. Go to Interface Control > Vendors and click "Add New".
2. Select Interface Type = Membership Interface and Key = basic9.
3. Fill in the remaining vendor details and click "Save".

**Test Result(s):**
1. The Basic9 vendor record is saved successfully.
2. The vendor is selectable when creating a membership interface.
3. Vendor key is displayed as "basic9".

#### 2.2. Basic9 Membership Interface Configuration Is Saved

**Prerequisite(s):**
1. The Basic9 vendor record exists.
2. Middleware provides URL, version, API key, company name, hotel code, facility code, facility type code, MSR code, and language index.

**Step(s):**
1. Go to Interface Control > Interfaces and click "Add New".
2. Select Interface Type = Membership Interface and Vendor = the Basic9 vendor.
3. Enter a unique interface code.
4. Fill in Connection Setup (URL, Version default "1", API Key), General (Company Name, Hotel Code, Facility Code, Facility Type Code, MSR code, Language Index), and Order Type Code Setup (Dine In, Take Out).
5. Click "Save".

**Test Result(s):**
1. The interface record is saved successfully with the unique code.
2. All mandatory fields retain the entered values after reopening the record.
3. Version defaults to "1" when it is not explicitly changed.
4. The interface is available for outlet attachment.

#### 2.3. Member Enquiry, List, Detail, and Card Info Display Configuration Is Saved

**Prerequisite(s):**
1. The Basic9 membership interface record exists.

**Step(s):**
1. Open the Basic9 membership interface setup.
2. Configure Member Enquiry Searching Setup with Card Number, Mobile, Member Name, and Birthday checked.
3. Configure Member List Setup, Member Detail Setup, and Card Info Setup field selections.
4. Click "Save" and reopen the interface record.

**Test Result(s):**
1. The setup is saved successfully.
2. The selected enquiry textboxes are displayed when SVC Enquiry is opened.
3. Member Listing, Member Detail, and Card Info tabs display only the configured fields.

#### 2.4. Basic9 Interface Is Attached to Outlet

**Prerequisite(s):**
1. The Basic9 membership interface record exists.
2. The target shop, outlet, and station are known.

**Step(s):**
1. Go to POS System > Interface Configurations > Outlet Settings and click "Add New".
2. Select Apply To = Outlet and select the target shop, outlet, and station.
3. Select Interface = the Basic9 interface created for this release.
4. Click "Save".

**Test Result(s):**
1. The outlet setting record is saved successfully.
2. The Basic9 interface is applied at the target outlet.
3. SVC Enquiry uses the Basic9 interface at that outlet.

#### 2.5. Basic9 Interface Is Attached to Payment Settings for Voucher, Point, and Employee

**Prerequisite(s):**
1. The Basic9 membership interface record exists.
2. Payment methods for Voucher, Point, and Employee are available.

**Step(s):**
1. Go to POS System > Interface Configurations > Payment Settings and click "Add New".
2. Select Apply To = Outlet and select the target shop, outlet, and station.
3. Select Interface = the Basic9 interface.
4. Set General Payment Type = Voucher, Point, or Employee and click "Save" for each type.

**Test Result(s):**
1. Each payment setting record is saved successfully.
2. The Basic9 interface is applied to the selected payment types.
3. The corresponding payment methods use Basic9 membership processing in the cashier.

#### 2.6. SVC Employee Enquiry Button Is Added to Display Panel

**Prerequisite(s):**
1. The Basic9 membership interface record exists.
2. The target ordering display panel page is accessible.

**Step(s):**
1. Go to POS System > Station Setup > Display Panels and select the ordering display panel zone.
2. Edit the target display panel page and click "Add New Button".
3. Select Type = Function and Available Options = SVC Employee Enquiry.
4. Optionally set Parameter Default Interface = the Basic9 interface.
5. Click "Submit" and then "Save".

**Test Result(s):**
1. The button is saved successfully in the "Ordering" or "Ordering Function" zone.
2. The "SVC Employee Enquiry" button is displayed on the ordering panel.
3. Clicking the button opens the Basic9 employee enquiry screen.

#### 2.7. Payment Method Buttons Are Configured for Basic9 Payment

**Prerequisite(s):**
1. The Basic9 interface is attached to a payment method.
2. The target ordering display panel page is accessible.

**Step(s):**
1. Go to POS System > Station Setup > Display Panels and edit the target display panel page.
2. Click "Add New Button" and select Type = Payment Method.
3. Select the target payment method attached to the Basic9 interface.
4. Set Full Payment = No or Allow multiple payments = Yes.
5. Click "Submit" and then "Save".

**Test Result(s):**
1. The payment method button is saved successfully.
2. The button is available in the cashier for the Basic9 payment method.
3. The payment method can be used for a partial or multiple-payment flow.

#### 2.8. Membership Printing Variables Are Added to Print Formats

**Prerequisite(s):**
1. The guest check and receipt print formats are accessible.
2. A test member and test employee exist in the Basic9 middleware.

**Step(s):**
1. Go to POS System > Printing Setup > Print Formats and add a new record or open an existing check/receipt format.
2. Select the "Print Format" tab.
3. Add the existing membership variables and the new membership variables listed in the release notes.
4. Define the remaining printing content and click "Save".

**Test Result(s):**
1. The print format is saved successfully.
2. All added membership variables are accepted by the print format.
3. Guest check and receipt can be printed without invalid-variable errors.

### 3. SVC Enquiry Member Search and Tabs

#### 3.1. SVC Enquiry Is Rejected in Mobile View

**Prerequisite(s):**
1. Infrasys POS is running in fine dining mode.
2. The ordering screen is displayed in mobile view.
3. A user with a user number assigned is logged in.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "Cannot perform this function in this mode".
2. The action aborts and the workflow ends.
3. The enquiry screen is not displayed.

#### 3.2. SVC Enquiry Requires a User Number

**Prerequisite(s):**
1. Infrasys POS is running in fine dining mode and horizontal desktop view.
2. The logged-in user has no user number assigned.

**Step(s):**
1. Open a new check.
2. Click function "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 3.3. SVC Enquiry Is Blocked by a Member from Another Membership Interface

**Prerequisite(s):**
1. The check is attached with a member from a membership interface other than Basic9.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open the check.
2. Click function "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "Has different interface member".
2. System prompts error message "Please clear current member first".
3. The action aborts and the workflow ends.

#### 3.4. SVC Enquiry Is Blocked by an Attached Basic9 Employee

**Prerequisite(s):**
1. The check is attached with a Basic9 employee.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open the check.
2. Click function "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "Please clear employee first".
2. The action aborts and the workflow ends.

#### 3.5. Enquiry Textboxes Follow Member Enquiry Searching Setup

**Prerequisite(s):**
1. The Basic9 interface is attached to the outlet.
2. Member Enquiry Searching Setup is accessible.

**Step(s):**
1. Uncheck "Mobile" in Member Enquiry Searching Setup and save.
2. Open a new check and click "SVC Enquiry".
3. Recheck "Mobile", save, and reopen "SVC Enquiry".

**Test Result(s):**
1. The mobile number textbox is hidden when "Mobile" is unchecked.
2. The mobile number textbox is displayed when "Mobile" is checked.
3. Card number, member name, and birthday textboxes follow their own setup flags.

#### 3.6. Card Number Accepts MSR, Swipe Card, and Manual Input

**Prerequisite(s):**
1. An MSR device and a test Basic9 card are available.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a card number manually and search.
3. Swipe the card through the MSR and search.
4. Use the swipe-card function and search.

**Test Result(s):**
1. Manual input populates the card number field and triggers card search.
2. MSR input populates the card number field and triggers card search.
3. Swipe-card input populates the card number field and triggers card search.

#### 3.7. Swipe Card Search Uses Card Number by Default

**Prerequisite(s):**
1. The Basic9 interface is attached to the outlet.
2. A test Basic9 card is available.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Swipe the card without entering any other search criterion.
3. Click "Search".

**Test Result(s):**
1. System searches by card number by default.
2. The returned member is displayed under Member Detail.
3. Point total and discount amount are shown.

#### 3.8. Birthday Accepts Only Eight Numeric Characters

**Prerequisite(s):**
1. The Basic9 interface is attached to the outlet.
2. Member Enquiry Searching Setup has "Birthday" checked.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday shorter than 8 characters.
3. Enter a birthday containing non-numeric characters.
4. Enter an exactly 8-character numeric birthday and click "Search".

**Test Result(s):**
1. Short input is rejected or cannot be searched.
2. Non-numeric input is rejected.
3. An exactly 8-character numeric birthday triggers birthday search.

#### 3.9. Search Criteria Priority Is Card Number, Mobile, Member Name, Birthday

**Prerequisite(s):**
1. The Basic9 interface is attached to the outlet.
2. All four enquiry textboxes are enabled.
3. Test data has different members for each criterion.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Fill card number, mobile number, member name, and birthday at the same time.
3. Click "Search".

**Test Result(s):**
1. System searches by card number first.
2. The result corresponds to the card number, not the other filled fields.
3. Removing the card number makes mobile number the next priority, followed by member name and birthday.

#### 3.10. Card Search Defaults to Member Detail Tab

**Prerequisite(s):**
1. A single Basic9 member matches the test card number.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search by card number.

**Test Result(s):**
1. Member Detail is the default tab.
2. Tabs are displayed in order: Member Listing, Member Detail, Card Info, Discount, Coupon, Applied Coupon, Used Coupon.
3. Member information is shown according to Member Detail Setup.

#### 3.11. Multiple Member Name Results Default to Member Listing

**Prerequisite(s):**
1. At least two Basic9 members share the same first name.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search by member name.

**Test Result(s):**
1. Member Listing is the default tab.
2. All returned members are listed according to Member List Setup.
3. Member Detail is not displayed until a member is selected.

#### 3.12. Multiple Birthday Results Default to Member Listing

**Prerequisite(s):**
1. At least two Basic9 members share the same birthday.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search by birthday.

**Test Result(s):**
1. Member Listing is the default tab.
2. All returned members are listed according to Member List Setup.
3. Member Detail is not displayed until a member is selected.

#### 3.13. Member Can Be Selected from Member Listing

**Prerequisite(s):**
1. A name or birthday search returns more than one member.
2. At least one returned member has status "Active".

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search by member name or birthday.
3. Select a target member in Member Listing.

**Test Result(s):**
1. The selected member is highlighted.
2. Member Detail is available for the selected member.
3. The "Set Member" button is displayed when the selected member is Active.

#### 3.14. Tab Clicked Without Member Selection Shows Warning

**Prerequisite(s):**
1. A name or birthday search returns more than one member.
2. No member is selected in Member Listing.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search by member name or birthday.
3. Click a tab other than Member Listing.

**Test Result(s):**
1. System displays warning message "Please search or select a member".
2. The system stays on Member Listing.
3. No member detail or discount list is displayed.

#### 3.15. Set Member Button Is Displayed for Active Member

**Prerequisite(s):**
1. The Get Member Details API returns member status "Active".
2. The member is selected or returned as a single result.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search and display the Active member.

**Test Result(s):**
1. The "Set Member" button is displayed.
2. The member detail information is visible.
3. The point total and discount amount are displayed.

#### 3.16. Set Member Button Is Hidden for Non-Active Member

**Prerequisite(s):**
1. The Get Member Details API returns a member status other than "Active".

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search and display the non-Active member.

**Test Result(s):**
1. The "Set Member" button is not displayed.
2. The member detail information is still readable.
3. The member cannot be attached to the check.

#### 3.17. Card Info Tab Displays Card Information from Get Member Details

**Prerequisite(s):**
1. A Basic9 member is returned by the Get Member Details API.
2. Card Info Setup is configured.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search for the member.
3. Click the "Card Info" tab.

**Test Result(s):**
1. Card information returned from the Get Member Details API is displayed.
2. Fields follow Card Info Setup.
3. No additional API call is triggered by switching to Card Info.

#### 3.18. Member-Dependent Tabs Require a Set Member

**Prerequisite(s):**
1. A member is searched by card number or mobile number but is not set to the check.
2. The Discount, Coupon, Applied Coupon, and Used Coupon tabs are visible.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search a member but do not click "Set Member".
3. Click "Discount", "Coupon", "Applied Coupon", or "Used Coupon".

**Test Result(s):**
1. System displays warning message "Please set a member".
2. The selected tab does not load discount or coupon data.
3. The system stays at the enquiry screen.

#### 3.19. Discount Tab Opens Member Discount Workflow for Set Member

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open SVC Enquiry for the check with the set member.
2. Click the "Discount" tab.

**Test Result(s):**
1. The member discount list is retrieved through the Query Discounts API.
2. The discount list screen is displayed.
3. The apply member discount workflow is available.

#### 3.20. Coupon Tab Opens Coupon Enquiry Workflow for Set Member

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. Available coupons exist in the middleware.

**Step(s):**
1. Open SVC Enquiry for the check with the set member.
2. Click the "Coupon" tab.

**Test Result(s):**
1. Available coupons are retrieved through the Query Coupons API.
2. The coupon list is displayed for selection.
3. The apply coupon workflow is available.

#### 3.21. Applied Coupon Tab Opens Applied Coupon Workflow for Set Member

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. A Basic9 coupon has already been applied to the check.

**Step(s):**
1. Open SVC Enquiry for the check with the set member.
2. Click the "Applied Coupon" tab.

**Test Result(s):**
1. Coupons applied to the current check are listed.
2. The "Void Coupon" operation is available.
3. The applied coupon and void coupon workflow is opened.

#### 3.22. Used Coupon Tab Displays Used Coupon Records

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. Coupons with status "Used" exist in the middleware.

**Step(s):**
1. Open SVC Enquiry for the check with the set member.
2. Click the "Used Coupon" tab.

**Test Result(s):**
1. Coupons with status "Used" are retrieved through the Query Coupons API.
2. Used coupon information is displayed in read-only mode.
3. No apply or void operation is available on this tab.

### 4. Member Enquiry by Card Number

#### 4.1. Card Number Lookup Returns Member Details and Points

**Prerequisite(s):**
1. A valid Basic9 card number exists in the middleware.
2. The Basic9 interface is attached to the outlet and the user has a user number.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the card number and click "Search".
3. Observe the API calls and the displayed tabs.

**Test Result(s):**
1. System triggers "Member Login" with the card number.
2. On success, system triggers "Get Member Details".
3. On success, system triggers "Get Points" with the access token from Member Login.
4. The enquiry result returns to step 16 of the SVC Enquiry workflow with member information displayed.

#### 4.2. Member Login Connection Failure on Card Lookup

**Prerequisite(s):**
1. The middleware test endpoint is configured to fail the connection for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a card number and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The workflow ends and no Get Member Details call is made.

#### 4.3. Member Login Error Response on Card Lookup

**Prerequisite(s):**
1. The middleware test endpoint is configured to return an error code for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a card number and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 4.4. Get Member Details Connection Failure on Card Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint is configured to fail the connection for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a card number and click "Search".

**Test Result(s):**
1. Member Login is completed successfully.
2. System prompts error message "No response" for Get Member Details.
3. The action aborts and stays at the enquiry screen.

#### 4.5. Get Member Details Error Response on Card Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a card number and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No point balance query is performed.

### 5. Member Enquiry by Mobile Number

#### 5.1. Mobile Number Lookup Returns the First Member and Points

**Prerequisite(s):**
1. A valid Basic9 mobile number exists in the middleware.
2. The Basic9 interface is attached to the outlet and the user has a user number.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the mobile number and click "Search".
3. Observe the API calls and the displayed member.

**Test Result(s):**
1. System triggers "Query Members" with the mobile number.
2. On success, system takes the card number of the first returned member and triggers "Member Login".
3. On success, system triggers "Get Member Details" and then "Get Points".
4. The enquiry result returns to step 16 of the SVC Enquiry workflow.

#### 5.2. Query Members Connection Failure on Mobile Lookup

**Prerequisite(s):**
1. The middleware test endpoint is configured to fail the connection for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 5.3. Query Members Error Response on Mobile Lookup

**Prerequisite(s):**
1. The middleware test endpoint returns an error code for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 5.4. Member Login Connection Failure on Mobile Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns at least one member.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. Query Members is completed successfully.
2. System prompts error message "No response" for Member Login.
3. The action aborts and stays at the enquiry screen.

#### 5.5. Member Login Error Response on Mobile Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns at least one member.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 5.6. Get Member Details Connection Failure on Mobile Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No point balance query is performed.

#### 5.7. Get Member Details Error Response on Mobile Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a mobile number and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No point balance query is performed.

### 6. Member Enquiry by Member Name

#### 6.1. Single Member Returned by Name Lookup

**Prerequisite(s):**
1. The test member name matches exactly one Basic9 member.
2. The Basic9 interface is attached to the outlet and the user has a user number.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the member name and click "Search".

**Test Result(s):**
1. System triggers "Query Members" with the member name.
2. For one returned member, system triggers "Member Login" with the returned card number.
3. On success, system triggers "Get Member Details".
4. System returns to step 16 of the SVC Enquiry workflow with Member Detail displayed.

#### 6.2. Multiple Members Returned by Name Lookup

**Prerequisite(s):**
1. The test member name matches more than one Basic9 member.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the member name and click "Search".

**Test Result(s):**
1. System triggers "Query Members" with the member name.
2. No Member Login is triggered until a member is selected.
3. Member Listing is displayed as the default tab with the returned members.

#### 6.3. Query Members Connection Failure on Name Lookup

**Prerequisite(s):**
1. The middleware test endpoint fails the connection for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 6.4. Query Members Error Response on Name Lookup

**Prerequisite(s):**
1. The middleware test endpoint returns an error code for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 6.5. Member Login Connection Failure on Name Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns exactly one member.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 6.6. Member Login Error Response on Name Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns exactly one member.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 6.7. Get Member Details Connection Failure on Name Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The member is not displayed.

#### 6.8. Get Member Details Error Response on Name Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a member name and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. The member is not displayed.

### 7. Member Enquiry by Birthday

#### 7.1. Single Member Returned by Birthday Lookup

**Prerequisite(s):**
1. The test birthday matches exactly one Basic9 member.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the exact 8-character birthday and click "Search".

**Test Result(s):**
1. System triggers "Query Members" with the birthday.
2. For one returned member, system triggers "Member Login" with the returned card number.
3. On success, system triggers "Get Member Details".
4. System returns to step 16 of the SVC Enquiry workflow with Member Detail displayed.

#### 7.2. Multiple Members Returned by Birthday Lookup

**Prerequisite(s):**
1. The test birthday matches more than one Basic9 member.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter the birthday and click "Search".

**Test Result(s):**
1. System triggers "Query Members" with the birthday.
2. No Member Login is triggered until a member is selected.
3. Member Listing is displayed as the default tab with the returned members.

#### 7.3. Query Members Connection Failure on Birthday Lookup

**Prerequisite(s):**
1. The middleware test endpoint fails the connection for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 7.4. Query Members Error Response on Birthday Lookup

**Prerequisite(s):**
1. The middleware test endpoint returns an error code for Query Members.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 7.5. Member Login Connection Failure on Birthday Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns exactly one member.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 7.6. Member Login Error Response on Birthday Lookup

**Prerequisite(s):**
1. Query Members succeeds and returns exactly one member.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 7.7. Get Member Details Connection Failure on Birthday Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The member is not displayed.

#### 7.8. Get Member Details Error Response on Birthday Lookup

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Enter a birthday and click "Search".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. The member is not displayed.

### 8. Point Balance Display

#### 8.1. Points Total and Discount Amount Are Displayed on Success

**Prerequisite(s):**
1. Member Login and Get Member Details succeed for a Basic9 member.
2. The Get Points API returns a points total and discount amount.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search the member by card number or mobile number.
3. Let the Get Points API complete.

**Test Result(s):**
1. System triggers "Get Points" with the access token returned by Member Login.
2. The Points total value returned by the API is displayed at step 16 of the SVC Enquiry workflow.
3. The Discount amount value returned by the API is displayed at step 16 of the SVC Enquiry workflow.

#### 8.2. Empty Point Values Are Displayed on API Failure

**Prerequisite(s):**
1. Member Login and Get Member Details succeed.
2. The Get Points API is configured to fail the connection or return an error code.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search the member by card number or mobile number.
3. Let the Get Points API fail.

**Test Result(s):**
1. The Points total field is displayed with an empty value.
2. The Discount amount field is displayed with an empty value.
3. The member information remains displayed and the user can continue the enquiry.

### 9. Set Member

#### 9.1. Member Information Is Stored and Attach Alert Is Displayed

**Prerequisite(s):**
1. An Active Basic9 member is displayed in the enquiry result.
2. The user has a user number assigned.

**Step(s):**
1. Open a new check and click "SVC Enquiry".
2. Search and select the Active member.
3. Click "Set Member".

**Test Result(s):**
1. System stores Member number, Card Number, Card Class, First Name, Last Name, First Name Alt, Points Total, and Discount Amount to the check.
2. System prompts alert message "Attach member successfully" and stays at the current tab.
3. The "Clear Member" button is displayed.

#### 9.2. Clear Member Button Opens Clear or Replace Workflow

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The member enquiry screen is displayed.

**Step(s):**
1. Open "SVC Enquiry" for the check.
2. Click the "Clear Member" button.

**Test Result(s):**
1. The system navigates to step 10 of the clear or replace member workflow.
2. The clear member confirmation dialog is displayed.
3. The member remains attached until the clear action is confirmed.

### 10. Clear or Replace Member

#### 10.1. Existing Member Is Displayed from the Stored Card Number

**Prerequisite(s):**
1. An old check is attached with a Basic9 member.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Open the old check.
2. Click "SVC Enquiry".

**Test Result(s):**
1. System triggers "Member Login" with the stored card number.
2. On success, member information is displayed as at step 16 of the SVC Enquiry workflow.
3. The "Clear Member" button is displayed.

#### 10.2. Another Member Can Be Searched and Replaced

**Prerequisite(s):**
1. An old check is attached with a Basic9 member.
2. A different Basic9 member exists in the middleware.

**Step(s):**
1. Open the old check and click "SVC Enquiry".
2. Search for another member by card number, mobile number, member name, or birthday.
3. Select the new member in the enquiry result.

**Test Result(s):**
1. System performs the member enquiry workflow for the searched member.
2. The enquiry result returns to step 16 of the SVC Enquiry workflow.
3. The new member can be set to the check.

#### 10.3. Clear Member Confirmation "No" Aborts the Action

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The "Clear Member" button is displayed.

**Step(s):**
1. Click "Clear Member".
2. Click "No" in the confirmation dialog box.

**Test Result(s):**
1. The action aborts and stays at the enquiry screen.
2. The Basic9 member remains attached to the check.
3. The workflow ends.

#### 10.4. Clear Member Is Blocked by Applied Coupon or Voucher

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. A Basic9 member coupon or payment voucher is applied to the check.

**Step(s):**
1. Click "Clear Member".
2. Click "Yes" in the confirmation dialog box.

**Test Result(s):**
1. System prompts error message "Please void the attached Basic9 payment voucher or coupon first".
2. System prompts error message "Cannot clear member".
3. The action aborts and stays at the enquiry screen.
4. The member remains attached.

#### 10.5. Applied Member Discounts Are Voided Automatically on Clear

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. Basic9 member discounts are applied to the check.
3. No Basic9 member coupon or payment voucher is applied.

**Step(s):**
1. Click "Clear Member" and confirm with "Yes".
2. Observe the applied member discounts.

**Test Result(s):**
1. System voids all applied Basic9 member discounts automatically.
2. The discount entries are removed from the check.
3. The member information is then cleared.

#### 10.6. All Basic9 Member Information Is Cleared from the Check

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. No Basic9 coupon, voucher, or member discount remains on the check.

**Step(s):**
1. Click "Clear Member" and confirm with "Yes".
2. Reopen the check and click "SVC Enquiry".

**Test Result(s):**
1. All Basic9 member information stored in the check is cleared.
2. The check is no longer attached with a Basic9 member.
3. The workflow ends.

#### 10.7. Member Login Connection Failure on Clear or Replace

**Prerequisite(s):**
1. An old check is attached with a Basic9 member.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open the old check and click "SVC Enquiry".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The workflow ends without clearing the member.

#### 10.8. Member Login Error Response on Clear or Replace

**Prerequisite(s):**
1. An old check is attached with a Basic9 member.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open the old check and click "SVC Enquiry".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. The workflow ends without clearing the member.

#### 10.9. Set Member After Clearing Attaches the Searched Member

**Prerequisite(s):**
1. A Basic9 member is attached to the check.
2. A different Basic9 member has been searched at step 8 of the clear or replace workflow.

**Step(s):**
1. Clear the current member as described in steps 10 to 15 of the clear or replace workflow.
2. Click "Set Member".

**Test Result(s):**
1. System performs the set member workflow for the member searched at step 8.
2. The searched member is attached to the check.
3. The previous member is no longer attached.

### 11. Member Discount Enquiry and Application

#### 11.1. Discount List Is Retrieved and Displayed

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware returns at least one discount for the member.

**Step(s):**
1. Open "SVC Enquiry" and click the "Discount" tab.
2. Observe the returned discount list.
3. Select one discount record.

**Test Result(s):**
1. System triggers "Query Discounts" to the external party.
2. The list displays Card number, Member Type, Food Discount Rate, Beverage Discount Rate, Other Discount Rate, Limit of Amount, Remark, Term Start Date, and Term End Date in a maximum of 6 columns on one page.
3. Only single selection is allowed.
4. The "Apply Discount" button is displayed after a discount is selected.

#### 11.2. Discount List Pagination Switches Six Columns

**Prerequisite(s):**
1. The Query Discounts response contains more than 6 display columns or records.
2. The discount list is displayed.

**Step(s):**
1. Click the next-page button on the discount list.
2. Click the previous-page button.

**Test Result(s):**
1. The next-page button displays the next 6 columns.
2. The previous-page button returns to the first 6 columns.
3. No discount selection is lost during paging.

#### 11.3. Member Discount Is Applied Successfully

**Prerequisite(s):**
1. A Basic9 member is set to the check and the check contains food, beverage, and other items.
2. Food Discount Code, Beverage Discount Code, and Other Discount Code are configured with valid POS percentage discounts.
3. The mapped POS discounts have Apply To = Check Discount or Item Discount, Discount Calculation = Percentage discount, and Rate = Open Discount.

**Step(s):**
1. Open the "Discount" tab and select a discount with rates larger than 0.
2. Click "Apply Discount".
3. Observe the applied discounts and the message.

**Test Result(s):**
1. System applies food discount, beverage discount, and other discount in that order for rates larger than 0.
2. System stores Interface ID, member number, and discount name "DC_FOOD", "DC_BEVERAGE", or "DC_OTHER" for each applied discount.
3. System prompts alert message "Apply member discount successfully".

#### 11.4. Query Discounts Connection Failure

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint fails the connection for Query Discounts.

**Step(s):**
1. Open the "Discount" tab.

**Test Result(s):**
1. System prompts error message "Discount list cannot be retrieved".
2. The action aborts and stays at the enquiry screen.
3. No discount list is displayed.

#### 11.5. Query Discounts Error Response

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint returns an error code for Query Discounts.

**Step(s):**
1. Open the "Discount" tab.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No discount list is displayed.

#### 11.6. Existing Member Discount Blocks Another Member Discount

**Prerequisite(s):**
1. A Basic9 member discount is already applied to the check.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Have existing member discount".
2. The action aborts and stays at the enquiry screen.
3. No additional member discount is applied.

#### 11.7. Existing Payment Voucher Blocks Member Discount

**Prerequisite(s):**
1. A Basic9 payment voucher is applied to the check.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Not allow to apply member discount due to voucher exists".
2. The action aborts and stays at the enquiry screen.
3. No member discount is applied.

#### 11.8. Existing Employee Discount Blocks Member Discount

**Prerequisite(s):**
1. A Basic9 employee discount is applied to the check.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Not allow to apply member discount due to employee discount exists".
2. The action aborts and stays at the enquiry screen.
3. No member discount is applied.

#### 11.9. Existing Member Coupon Blocks Member Discount

**Prerequisite(s):**
1. A Basic9 member coupon is applied to the check.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Not allow to apply member discount due to coupon exists".
2. The action aborts and stays at the enquiry screen.
3. No member discount is applied.

#### 11.10. Existing POS Discount Blocks Member Discount

**Prerequisite(s):**
1. A POS discount is applied to the check.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Not allow to apply member discount due to POS discount exists".
2. The action aborts and stays at the enquiry screen.
3. No member discount is applied.

#### 11.11. Missing Discount Code Setup Shows No Discount Setup

**Prerequisite(s):**
1. One of Food Discount Code, Beverage Discount Code, or Other Discount Code is empty in the interface setup.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "No discount setup".
2. The action aborts and stays at the enquiry screen.
3. No discount is applied.

#### 11.12. Discount Type Not Found in POS Shows No Discount Setup

**Prerequisite(s):**
1. The POS discount type configured for the selected discount does not exist in the POS system.

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "No discount setup".
2. The action aborts and stays at the enquiry screen.
3. No discount is applied.

#### 11.13. Invalid Apply To Setting Shows Invalid Discount Setup

**Prerequisite(s):**
1. The configured POS discount has Discount setup "Apply To" other than "Check Discount" or "Item Discount".

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Invalid discount setup".
2. The action aborts and stays at the enquiry screen.
3. No discount is applied.

#### 11.14. Invalid Discount Calculation Shows Invalid Discount Setup

**Prerequisite(s):**
1. The configured POS discount has Discount Calculation other than "Percentage discount".

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Invalid discount setup".
2. The action aborts and stays at the enquiry screen.
3. No discount is applied.

#### 11.15. Invalid Rate Setting Shows Invalid Discount Setup

**Prerequisite(s):**
1. The configured POS discount has Rate other than "Open Discount".

**Step(s):**
1. Open the "Discount" tab and select a discount.
2. Click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Invalid discount setup".
2. The action aborts and stays at the enquiry screen.
3. No discount is applied.

#### 11.16. Food Discount Application Failure Aborts the Action

**Prerequisite(s):**
1. The selected discount has a food discount rate larger than 0.
2. The system is configured to fail the food discount application.

**Step(s):**
1. Click "Apply Discount".
2. Observe the result.

**Test Result(s):**
1. System prompts error message "Fail to apply discount".
2. The action aborts and the workflow ends.
3. No member discount remains applied.

#### 11.17. Beverage Failure Voids the Applied Food Discount

**Prerequisite(s):**
1. The selected discount has food and beverage rates larger than 0.
2. The system is configured to apply the food discount successfully and fail the beverage discount.

**Step(s):**
1. Click "Apply Discount".
2. Observe the applied and voided discounts.

**Test Result(s):**
1. System prompts error message "Fail to apply discount".
2. System voids the food discount with the void reason from "Void Reason Code For Member Discount".
3. The action aborts and the workflow ends.

#### 11.18. Other Failure Voids the Applied Food and Beverage Discounts

**Prerequisite(s):**
1. The selected discount has food, beverage, and other rates larger than 0.
2. The system is configured to fail the other discount application.

**Step(s):**
1. Click "Apply Discount".
2. Observe the applied and voided discounts.

**Test Result(s):**
1. System prompts error message "Fail to apply discount".
2. System voids the food discount and the beverage discount with the void reason from "Void Reason Code For Member Discount".
3. The action aborts and the workflow ends.

#### 11.19. Item Discount Is Skipped When No Applicable Item Exists

**Prerequisite(s):**
1. The selected discount is an item discount.
2. The check does not contain items in the category applicable to the discount.

**Step(s):**
1. Click "Apply Discount".
2. Observe the discount application and the next discount in the list.

**Test Result(s):**
1. System skips the current item discount.
2. System continues with the next discount without an error message.
3. No discount is applied to the check for the skipped item discount.

#### 11.20. New Check Is Sent and Transaction Number Is Generated

**Prerequisite(s):**
1. The check is new and has no transaction number.
2. A Basic9 member is set to the check.

**Step(s):**
1. Open the "Discount" tab.
2. Observe the check status before the Query Discounts API call.

**Test Result(s):**
1. System triggers send check before the discount query.
2. A transaction number is generated for the new check.
3. The Query Discounts API call is then triggered.

### 12. Coupon Enquiry and Application

#### 12.1. Available Coupons Are Displayed for Multiple Selection

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware returns at least one coupon with status "Available".

**Step(s):**
1. Open "SVC Enquiry" and click the "Coupon" tab.
2. Observe the coupon list.
3. Select multiple coupon records.

**Test Result(s):**
1. System triggers "Query Coupons" to the external party.
2. Coupons with status "Available" display Member Card Number, Coupon Number, Coupon Name, FDC, BDC, ODC, Remark, Term Start Date, and Term End Date in a maximum of 6 columns on one page.
3. Multiple selection is allowed.
4. The "Apply Coupon" button is displayed after selection.

#### 12.2. Coupon List Pagination Switches Six Columns

**Prerequisite(s):**
1. The coupon list contains more than 6 display columns.
2. The coupon list is displayed.

**Step(s):**
1. Click the next-page button on the coupon list.
2. Click the previous-page button.

**Test Result(s):**
1. The next-page button displays the next 6 columns.
2. The previous-page button returns to the first 6 columns.
3. Selected coupons remain selected during paging.

#### 12.3. Coupons Are Applied and Redeemed Successfully

**Prerequisite(s):**
1. A Basic9 member is set to the check and the check contains food, beverage, and other items.
2. The coupon FDC, BDC, and ODC codes are mapped to valid POS percentage discounts.
3. The mapped POS discounts have Apply To = Check Discount or Item Discount and Discount Calculation = Percentage discount.

**Step(s):**
1. Open the "Coupon" tab and select available coupons.
2. Click "Apply Coupon".
3. Let Prepare Redeem Coupon and Redeem Coupon complete.

**Test Result(s):**
1. System applies the mapped discounts for rates larger than 0 in order of food, beverage, then other discount.
2. System triggers "Prepare Redeem Coupon" and then "Redeem Coupon" with the returned request ID.
3. System prompts alert message "Coupon redeemed successfully".
4. System stores Member Card number, Coupon number, FDC, BDC, ODC, Remark, Term start date, Term end date, Interface ID, and Request ID.

#### 12.4. Query Coupons Connection Failure

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint fails the connection for Query Coupons.

**Step(s):**
1. Open the "Coupon" tab.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No coupon list is displayed.

#### 12.5. Query Coupons Error Response

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint returns an error code for Query Coupons.

**Step(s):**
1. Open the "Coupon" tab.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No coupon list is displayed.

#### 12.6. Existing Member Discount Blocks Coupon Application

**Prerequisite(s):**
1. A Basic9 member discount is already applied to the check.

**Step(s):**
1. Open the "Coupon" tab, select a coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Have existing member discount".
2. The action aborts and stays at the enquiry screen.
3. No coupon discount is applied.

#### 12.7. Existing Payment Voucher Blocks Coupon Application

**Prerequisite(s):**
1. A Basic9 payment voucher is applied to the check.

**Step(s):**
1. Open the "Coupon" tab, select a coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Not allow to apply coupon due to voucher exists".
2. The action aborts and stays at the enquiry screen.
3. No coupon discount is applied.

#### 12.8. Existing Employee Discount Blocks Coupon Application

**Prerequisite(s):**
1. A Basic9 employee discount is applied to the check.

**Step(s):**
1. Open the "Coupon" tab, select a coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Not allow to apply coupon due to employee discount exists".
2. The action aborts and stays at the enquiry screen.
3. No coupon discount is applied.

#### 12.9. Existing POS Discount Blocks Coupon Application

**Prerequisite(s):**
1. A POS discount is applied to the check.

**Step(s):**
1. Open the "Coupon" tab, select a coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Not allow to apply coupon due to POS discount exists".
2. The action aborts and stays at the enquiry screen.
3. No coupon discount is applied.

#### 12.10. Missing Coupon Mapping Shows No Mapping Found

**Prerequisite(s):**
1. A selected coupon code has no mapping record in "Discount Coupon Mapping".

**Step(s):**
1. Open the "Coupon" tab, select the coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "No mapping found for <Coupon Code>".
2. The action aborts and the workflow ends.
3. No coupon discount is applied.

#### 12.11. Empty Target Discount Code Shows Empty Target Discount Code

**Prerequisite(s):**
1. The selected coupon code is mapped to an empty POS discount code.

**Step(s):**
1. Open the "Coupon" tab, select the coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Empty target discount code for <Coupon Code>".
2. The action aborts and the workflow ends.
3. No coupon discount is applied.

#### 12.12. Mapped Discount Not Found in POS

**Prerequisite(s):**
1. The selected coupon code is mapped to a POS discount code that does not exist in the POS system.

**Step(s):**
1. Open the "Coupon" tab, select the coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Target mapped discount <Discount Code> Not found".
2. The action aborts and the workflow ends.
3. No coupon discount is applied.

#### 12.13. Mapped Discount Used for Extra Charge Is Rejected

**Prerequisite(s):**
1. The selected coupon code is mapped to a POS discount used for extra charge.

**Step(s):**
1. Open the "Coupon" tab, select the coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Target mapped discount <Discount Code> is acting as extra charge".
2. The action aborts and the workflow ends.
3. No coupon discount is applied.

#### 12.14. Invalid Mapped Discount Type Is Rejected

**Prerequisite(s):**
1. The mapped POS discount has Apply To other than "Check Discount" or "Item Discount", or Discount Calculation other than "Percentage discount".

**Step(s):**
1. Open the "Coupon" tab, select the coupon, and click "Apply Coupon".

**Test Result(s):**
1. System prompts error message "Mapped discount <Discount Code> has invalid discount type setting".
2. The action aborts and the workflow ends.
3. No coupon discount is applied.

#### 12.15. Food Coupon Discount Application Failure

**Prerequisite(s):**
1. The selected coupon has a food discount rate larger than 0.
2. The system is configured to fail the food discount application.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the result.

**Test Result(s):**
1. System prompts error message "Fail to apply coupon".
2. The action aborts and the workflow ends.
3. No coupon discount remains applied.

#### 12.16. Beverage Failure Voids Food Coupon Discount

**Prerequisite(s):**
1. The selected coupon has food and beverage rates larger than 0.
2. The system is configured to apply the food discount and fail the beverage discount.
3. The "Void Reason Code For Discount Coupon" setup value is invalid.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the voided discount and its void reason.

**Test Result(s):**
1. System prompts error message "Fail to apply coupon".
2. System voids the food discount with the configured void reason.
3. If the configured void reason is invalid, system defaults to "0".
4. The action aborts and the workflow ends.

#### 12.17. Other Failure Voids Food and Beverage Coupon Discounts

**Prerequisite(s):**
1. The selected coupon has food, beverage, and other rates larger than 0.
2. The system is configured to fail the other discount application.
3. The "Void Reason Code For Discount Coupon" setup value is invalid.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the voided discounts and their void reasons.

**Test Result(s):**
1. System prompts error message "Fail to apply coupon".
2. System voids the food discount and the beverage discount with the configured void reason.
3. If the configured void reason is invalid, system defaults to "0".
4. The action aborts and the workflow ends.

#### 12.18. Prepare Redeem Coupon Connection Failure Moves to Next Coupon

**Prerequisite(s):**
1. All mapped discounts are applied successfully.
2. Multiple coupons are selected.
3. The middleware test endpoint fails the connection for Prepare Redeem Coupon.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the message and the next selected coupon.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes back to step 37 for the next selected coupon.
3. The discount applied for the failed coupon remains on the check while the next coupon is processed.

#### 12.19. Prepare Redeem Coupon Error Voids the Applied Discounts

**Prerequisite(s):**
1. All mapped discounts are applied successfully.
2. The middleware test endpoint returns an error code for Prepare Redeem Coupon.
3. The "Void Reason Code For Discount Coupon" setup value is invalid.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the message and the discount status.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System voids the corresponding discount with the configured void reason.
3. If the configured void reason is invalid, system defaults to "0".

#### 12.20. Redeem Coupon Connection Failure Cancels the Coupon

**Prerequisite(s):**
1. Prepare Redeem Coupon succeeds.
2. The middleware test endpoint fails the connection for Redeem Coupon.

**Step(s):**
1. Let the Redeem Coupon call fail.
2. Observe the coupon and discount status.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 45 and voids the coupons applied at step 37.
3. System triggers "Cancel redeem coupon" with the request ID from Prepare Redeem Coupon.

#### 12.21. Redeem Coupon Error Response Cancels the Coupon

**Prerequisite(s):**
1. Prepare Redeem Coupon succeeds.
2. The middleware test endpoint returns an error code for Redeem Coupon.

**Step(s):**
1. Let the Redeem Coupon call return an error.
2. Observe the coupon and discount status.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 45 and voids the coupons applied at step 37.
3. System triggers "Cancel redeem coupon" with the request ID from Prepare Redeem Coupon.

#### 12.22. Redeem Coupon Success Stores Redemption Information

**Prerequisite(s):**
1. Prepare Redeem Coupon succeeds.
2. The middleware returns a success response for Redeem Coupon.

**Step(s):**
1. Let the Redeem Coupon call complete.
2. Observe the message and the stored coupon information.

**Test Result(s):**
1. System prompts alert message "Coupon redeemed successfully".
2. System stays at the enquiry screen and stores Member Card number, Coupon number, FDC, BDC, ODC, Remark, Term start date, Term end date, Interface ID, and Request ID.
3. The workflow stops after all selected coupons are processed.

#### 12.23. Cancel Redeem Coupon Is Triggered After Failed Redemption

**Prerequisite(s):**
1. Prepare Redeem Coupon returns a request ID.
2. Redeem Coupon fails by connection failure or error response.

**Step(s):**
1. Observe the system handling after the Redeem Coupon failure.

**Test Result(s):**
1. System voids the coupons applied at step 37.
2. System triggers "Cancel redeem coupon" with the request ID from Prepare Redeem Coupon.
3. The result of the Cancel redeem coupon call is not handled.

#### 12.24. Item Coupon Discount Is Skipped When No Applicable Item Exists

**Prerequisite(s):**
1. The mapped coupon discount is an item discount.
2. The check does not contain items in the applicable category.

**Step(s):**
1. Click "Apply Coupon".
2. Observe the discount application and the next discount.

**Test Result(s):**
1. System skips the current item discount.
2. System continues with the next discount without an error message.
3. No discount is applied for the skipped item discount.

### 13. Applied Coupon and Void Coupon

#### 13.1. Applied Basic9 Coupons Are Listed

**Prerequisite(s):**
1. At least one Basic9 coupon is applied to the check.
2. The member enquiry screen is displayed.

**Step(s):**
1. Click the "Applied Coupon" tab.
2. Observe the list and select one coupon.

**Test Result(s):**
1. System filters discount records of the current check that come from Basic9 coupons.
2. Member Card Number, Coupon Number, Coupon Name, FDC, BDC, ODC, Remark, Term Start Date, and Term End Date are displayed in a maximum of 6 columns on one page.
3. Only single selection is allowed.
4. The "Void Coupon" button is displayed.

#### 13.2. Coupon Is Voided Successfully

**Prerequisite(s):**
1. A Basic9 coupon is applied to the check.
2. Member Login, Prepare Refund Coupon, and Refund Coupon succeed.

**Step(s):**
1. Open the "Applied Coupon" tab and select the coupon.
2. Click "Void Coupon".
3. Let the refund calls complete.

**Test Result(s):**
1. System triggers "Member Login" with the card number.
2. System triggers "Prepare Refund Coupon" and then "Refund Coupon" with the request ID.
3. System continues the existing void discount workflow.
4. The applied coupon is voided from the check.

#### 13.3. Member Login Connection Failure on Void Coupon

**Prerequisite(s):**
1. A Basic9 coupon is applied to the check.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open the "Applied Coupon" tab and select the coupon.
2. Click "Void Coupon".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The coupon remains applied.

#### 13.4. Member Login Error Response on Void Coupon

**Prerequisite(s):**
1. A Basic9 coupon is applied to the check.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open the "Applied Coupon" tab and select the coupon.
2. Click "Void Coupon".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. The coupon remains applied.

#### 13.5. Prepare Refund Coupon Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Refund Coupon.

**Step(s):**
1. Click "Void Coupon".
2. Observe the message.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Refund Coupon call is made.

#### 13.6. Prepare Refund Coupon Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Refund Coupon.

**Step(s):**
1. Click "Void Coupon".
2. Observe the message.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Refund Coupon call is made.

#### 13.7. Refund Coupon Connection Failure Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Coupon succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Refund Coupon.

**Step(s):**
1. Click "Void Coupon".
2. Observe the handling after the failure.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 20 and triggers "Cancel Refund Coupon".
3. The result of the Cancel Refund Coupon call is not handled.

#### 13.8. Refund Coupon Error Response Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Coupon succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Refund Coupon.

**Step(s):**
1. Click "Void Coupon".
2. Observe the handling after the error.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 20 and triggers "Cancel Refund Coupon".
3. The result of the Cancel Refund Coupon call is not handled.

#### 13.9. Cancel Refund Coupon Is Triggered with the Prepare Request ID

**Prerequisite(s):**
1. Prepare Refund Coupon returns a request ID.
2. Refund Coupon fails by connection failure or error response.

**Step(s):**
1. Observe the Cancel Refund Coupon request.

**Test Result(s):**
1. System triggers "Cancel Refund Coupon" with the request ID returned by Prepare Refund Coupon.
2. The result of Cancel Refund Coupon is not handled.
3. The action aborts and the workflow stops.

#### 13.10. Applied Coupon List Pagination Switches Six Columns

**Prerequisite(s):**
1. The applied coupon list contains more than 6 display columns.

**Step(s):**
1. Click the next-page button on the applied coupon list.
2. Click the previous-page button.

**Test Result(s):**
1. The next-page button displays the next 6 columns.
2. The previous-page button returns to the first 6 columns.
3. The selected coupon is preserved during paging.

### 14. Used Coupon Display

#### 14.1. Used Coupons Are Displayed in Read-Only Mode

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware contains coupons with status "Used".

**Step(s):**
1. Open "SVC Enquiry" and click the "Used Coupon" tab.
2. Observe the list and attempt an operation.

**Test Result(s):**
1. System triggers "Query Coupons" and displays coupons with status "Used".
2. Member Card Number, Coupon Number, Coupon Name, Used Date, Used Outlet, Term Start Date, and Term End Date are displayed in a maximum of 6 columns on one page.
3. The page is read-only and no operation can be performed.

#### 14.2. Query Coupons Connection Failure on Used Coupon List

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint fails the connection for Query Coupons.

**Step(s):**
1. Open the "Used Coupon" tab.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No used coupon list is displayed.

#### 14.3. Query Coupons Error Response on Used Coupon List

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. The middleware test endpoint returns an error code for Query Coupons.

**Step(s):**
1. Open the "Used Coupon" tab.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No used coupon list is displayed.

#### 14.4. Used Coupon List Pagination Switches Six Columns

**Prerequisite(s):**
1. The used coupon list contains more than 6 display columns.

**Step(s):**
1. Click the next-page button on the used coupon list.
2. Click the previous-page button.

**Test Result(s):**
1. The next-page button displays the next 6 columns.
2. The previous-page button returns to the first 6 columns.
3. The list remains read-only during paging.

### 15. Membership Printing Variables

#### 15.1. Existing Membership Printing Variables Print on Guest Check and Receipt

**Prerequisite(s):**
1. A guest check and a receipt print format contain the existing membership variables.
2. A Basic9 member is set to the check, with member discount, coupon, point earning, point redemption, and voucher payment applied where applicable.

**Step(s):**
1. Complete a check with a Basic9 member and apply member discount and member coupon.
2. Settle the check with point redemption and voucher payment.
3. Print the guest check and the receipt.

**Test Result(s):**
1. CheckMembershipIntfMemberNumber, CheckMembershipIntfCardNumber, CheckMembershipIntfMemberFirstName, and CheckMembershipIntfPointBalance print the correct member values.
2. DiscountMembershipIntfCouponNumber, DiscountMembershipIntfCouponName, and DiscountMembershipIntfDiscountName print the applied coupon and discount values, with the discount name being DC_FOOD, DC_BEVERAGE, or DC_OTHER.
3. CheckMembershipIntfPointEarn, PaymentMembershipIntfPointRedeem, and PaymentMembershipIntfCouponNumber print the point earned, point redeemed, and voucher number from the payment voucher workflow.

#### 15.2. New Membership Printing Variables Print on Guest Check and Receipt

**Prerequisite(s):**
1. A guest check and a receipt print format contain the new membership variables.
2. A Basic9 member check and a Basic9 employee check are available.

**Step(s):**
1. Print a guest check and receipt for a check with a Basic9 member.
2. Print a guest check and receipt for a check with a Basic9 employee.

**Test Result(s):**
1. CheckMembershipIntfMemberFirstNameAlt, CheckMembershipIntfMemberClass, and CheckMembershipIntfEarnPointRate print the member alternative first name, member class, and point earn rate.
2. CheckMembershipIntfEmployeeNumber, CheckMembershipIntfEmployeeFirstName, and CheckMembershipintfEmployeeFirstNameAlt print the employee number, first name, and alternative first name.
3. All new variables print valid values on both guest check and receipt.

### 16. SVC Employee Enquiry

#### 16.1. SVC Employee Enquiry Is Rejected Outside Fine Dining Mode

**Prerequisite(s):**
1. The ordering mode is not fine dining mode.
2. A user with a user number assigned is logged in.

**Step(s):**
1. Open a new check.
2. Click function "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "Cannot perform this function in this mode".
2. The action aborts and the workflow ends.

#### 16.2. SVC Employee Enquiry Is Rejected in Mobile View

**Prerequisite(s):**
1. Infrasys POS is running in fine dining mode.
2. The ordering screen is displayed in mobile view.

**Step(s):**
1. Open a new check.
2. Click function "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "Cannot perform this function in this mode".
2. The action aborts and the workflow ends.

#### 16.3. SVC Employee Enquiry Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.

**Step(s):**
1. Open a new check and click "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 16.4. SVC Employee Enquiry Is Blocked by a Member from Another Interface

**Prerequisite(s):**
1. The check is attached with a member from a membership interface other than Basic9.

**Step(s):**
1. Open the check and click "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "Have different interface member".
2. System prompts error message "Please clear current member first".
3. The action aborts and the workflow ends.

#### 16.5. SVC Employee Enquiry Is Blocked by an Attached Basic9 Member

**Prerequisite(s):**
1. The check is attached with a Basic9 member.

**Step(s):**
1. Open the check and click "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "Please clear member first".
2. The action aborts and the workflow ends.

#### 16.6. Employee Search Fields Follow Employee Enquiry Searching Setup

**Prerequisite(s):**
1. Employee Enquiry Searching Setup is accessible.
2. The Basic9 interface is attached to the outlet.

**Step(s):**
1. Uncheck "Employee Name" in the setup and save.
2. Open a new check and click "SVC Employee Enquiry".
3. Recheck "Employee Name", save, and reopen the function.

**Test Result(s):**
1. The employee name textbox is hidden when the option is unchecked.
2. The employee name textbox is displayed when the option is checked.
3. The employee number textbox follows its own setup flag.

#### 16.7. Employee Number Lookup Returns Employee Details

**Prerequisite(s):**
1. A valid Basic9 employee number exists in the middleware.
2. The user has a user number assigned.

**Step(s):**
1. Open a new check and click "SVC Employee Enquiry".
2. Enter the employee number and click "Search".
3. Observe the API calls and the Employee Detail tab.

**Test Result(s):**
1. System triggers "Member Login" with the employee number.
2. On success, system triggers "Get Member Details".
3. Employee information is displayed in Employee Detail.
4. The "Set Employee" button is displayed when the employee status is "Active".

#### 16.8. Single Employee Returned by Name Lookup

**Prerequisite(s):**
1. The test employee name matches exactly one Basic9 employee.

**Step(s):**
1. Open a new check and click "SVC Employee Enquiry".
2. Enter the employee name and click "Search".

**Test Result(s):**
1. System triggers "Query Members" with the employee name.
2. For one returned employee, system triggers "Member Login" with the returned employee number.
3. On success, system triggers "Get Member Details".
4. Employee Detail is displayed.

#### 16.9. Multiple Employees Returned by Name Lookup

**Prerequisite(s):**
1. The test employee name matches more than one Basic9 employee.

**Step(s):**
1. Open a new check and click "SVC Employee Enquiry".
2. Enter the employee name and click "Search".
3. Select an employee in Employee Listing.

**Test Result(s):**
1. Employee Listing is the default tab and displays returned employees according to Employee List Setup.
2. No Member Login is triggered until a record is selected.
3. Selecting an employee displays Employee Detail and enables "Set Employee" for an Active employee.

#### 16.10. Query Members Connection Failure on Employee Name Lookup

**Prerequisite(s):**
1. The middleware test endpoint fails the connection for Query Members.

**Step(s):**
1. Open "SVC Employee Enquiry" and search by employee name.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 16.11. Query Members Error Response on Employee Name Lookup

**Prerequisite(s):**
1. The middleware test endpoint returns an error code for Query Members.

**Step(s):**
1. Open "SVC Employee Enquiry" and search by employee name.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Member Login call is made.

#### 16.12. Member Login Connection Failure on Employee Enquiry

**Prerequisite(s):**
1. The employee number or a single employee result is available.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open "SVC Employee Enquiry" and search the employee.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 16.13. Member Login Error Response on Employee Enquiry

**Prerequisite(s):**
1. The employee number or a single employee result is available.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open "SVC Employee Enquiry" and search the employee.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 16.14. Get Member Details Connection Failure on Employee Enquiry

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Member Details.

**Step(s):**
1. Open "SVC Employee Enquiry" and search the employee.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No employee information is displayed.

#### 16.15. Get Member Details Error Response on Employee Enquiry

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open "SVC Employee Enquiry" and search the employee.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No employee information is displayed.

#### 16.16. Inactive Employee Has No Set Employee Button

**Prerequisite(s):**
1. The returned employee status is not "Active".

**Step(s):**
1. Search and display the employee in "SVC Employee Enquiry".

**Test Result(s):**
1. The "Set Employee" button is not displayed.
2. The employee details remain readable.
3. The workflow ends without attaching the employee.

#### 16.17. Active Employee Shows Set Employee Button

**Prerequisite(s):**
1. The returned employee status is "Active".

**Step(s):**
1. Search and display the employee in "SVC Employee Enquiry".

**Test Result(s):**
1. The "Set Employee" button is displayed.
2. The employee details are displayed in Employee Detail.

#### 16.18. Employee Detail Is the Default Tab for Number and Single Result

**Prerequisite(s):**
1. Search is by employee number or by employee name with one returned employee.

**Step(s):**
1. Open "SVC Employee Enquiry" and perform the search.

**Test Result(s):**
1. Employee Detail is the default tab.
2. Tabs are displayed in order: Employee Listing, Employee Detail, Discount.
3. Employee information follows Employee Detail Setup.

#### 16.19. Employee Listing Is the Default Tab for Multiple Name Results

**Prerequisite(s):**
1. Search is by employee name and returns more than one employee.

**Step(s):**
1. Open "SVC Employee Enquiry" and perform the search.

**Test Result(s):**
1. Employee Listing is the default tab.
2. Records are displayed according to Employee List Setup.
3. Employee Detail is not displayed until a record is selected.

#### 16.20. Discount Tab Is Read-Only When No Employee Is Set

**Prerequisite(s):**
1. An employee is searched but not set to the check.

**Step(s):**
1. Open "SVC Employee Enquiry" and search the employee.
2. Click the "Discount" tab.

**Test Result(s):**
1. Employee discounts are displayed as read-only.
2. The discounts cannot be selected.
3. No "Apply Discount" button is displayed.

#### 16.21. Set Employee Opens the Set Employee Workflow

**Prerequisite(s):**
1. An Active employee is displayed.
2. The check has no attached Basic9 employee.

**Step(s):**
1. Click "Set Employee".

**Test Result(s):**
1. The set employee workflow is opened.
2. Employee number, Last Name, and First Name Alt are stored to the check.
3. Alert message "Attach employee successfully" is displayed.

#### 16.22. Discount Tab Opens Applying Employee Discount Workflow

**Prerequisite(s):**
1. An Active employee is set to the check.

**Step(s):**
1. Open "SVC Employee Enquiry" and click the "Discount" tab.

**Test Result(s):**
1. The employee discount list is read and checked from "Employee Discount List".
2. The applying employee discount workflow is opened.
3. Only eligible discounts are displayed.

### 17. Set Employee

#### 17.1. Employee Information Is Stored and Attach Alert Is Displayed

**Prerequisite(s):**
1. An Active Basic9 employee is displayed in the enquiry result.

**Step(s):**
1. Click "Set Employee".

**Test Result(s):**
1. System stores Employee number, Last Name, and First Name Alt to the check.
2. System prompts alert message "Attach employee successfully" and stays at the current tab.
3. The "Clear Employee" button is displayed.

#### 17.2. Clear Employee Button Opens Clear or Replace Workflow

**Prerequisite(s):**
1. A Basic9 employee is set to the check.

**Step(s):**
1. Open "SVC Employee Enquiry".
2. Click "Clear Employee".

**Test Result(s):**
1. System navigates to step 10 of the clear or replace employee workflow.
2. The clear confirmation is displayed.
3. The employee remains attached until the clear is confirmed.

### 18. Employee Discount Enquiry and Application

#### 18.1. Eligible Employee Discounts Are Displayed

**Prerequisite(s):**
1. The Basic9 employee is set to the check.
2. Employee Discount List contains valid POS discount codes.
3. The discount types have Rate (%) and Fixed Amount not set as Extra Charge or Open Extra Charge, and Apply To = Check Discount or Item Discount.

**Step(s):**
1. Open "SVC Employee Enquiry" and click the "Discount" tab.
2. Observe the discount records.

**Test Result(s):**
1. Only discount records that pass all checks are displayed.
2. Discount Name, Discount Code, and Discount Value are displayed.
3. Percentage values display as <rate>% with a maximum of 2 decimal places.
4. Fixed amount values display with a maximum of 2 decimal places.

#### 18.2. Applied Employee Discount Is Moved to the Top

**Prerequisite(s):**
1. An employee discount has already been applied to the check.
2. The employee discount list is displayed.

**Step(s):**
1. Open the "Discount" tab.
2. Observe the order of the discount records.

**Test Result(s):**
1. The previously applied employee discount is moved to the top of the list.
2. The remaining discounts keep their normal order.

#### 18.3. Apply Discount Button Is Hidden When No Employee Is Set

**Prerequisite(s):**
1. No Basic9 employee is set to the check.
2. The discount tab is opened.

**Step(s):**
1. Attempt to select a discount record.

**Test Result(s):**
1. Discount records cannot be selected.
2. The "Apply Discount" button is hidden.
3. The workflow ends.

#### 18.4. Empty Employee Discount List Hides Discounts

**Prerequisite(s):**
1. Interface setup "Employee Discount List" is empty.

**Step(s):**
1. Open the "Discount" tab for a set employee.

**Test Result(s):**
1. No employee discount records are displayed.
2. No "Apply Discount" button is displayed.

#### 18.5. Discount Type Not Found in POS Is Excluded

**Prerequisite(s):**
1. A discount code in Employee Discount List does not exist in the POS system.

**Step(s):**
1. Open the employee "Discount" tab.

**Test Result(s):**
1. The missing discount type is not displayed.
2. Other valid discount records remain available.

#### 18.6. Rate-Based Extra Charge Discount Is Excluded

**Prerequisite(s):**
1. A discount type has Rate (%) selected as "Extra Charge" or "Open Extra Charge".

**Step(s):**
1. Open the employee "Discount" tab.

**Test Result(s):**
1. The extra-charge rate discount is not displayed.
2. The discount cannot be applied.

#### 18.7. Fixed Amount Extra Charge Discount Is Excluded

**Prerequisite(s):**
1. A discount type has Fixed Amount selected as "Extra Charge" or "Open Extra Charge".

**Step(s):**
1. Open the employee "Discount" tab.

**Test Result(s):**
1. The extra-charge fixed amount discount is not displayed.
2. The discount cannot be applied.

#### 18.8. Invalid Apply To Discount Is Excluded

**Prerequisite(s):**
1. A discount type has Apply To other than "Check Discount" or "Item Discount".

**Step(s):**
1. Open the employee "Discount" tab.

**Test Result(s):**
1. The invalid discount is not displayed.
2. The discount cannot be applied.

#### 18.9. Employee Discount Is Applied Successfully

**Prerequisite(s):**
1. An Employee is set to the check.
2. A valid employee discount is displayed.

**Step(s):**
1. Select the discount and click "Apply Discount".
2. Click "OK" on the success message.

**Test Result(s):**
1. System applies the selected discount to the check.
2. System prompts message "Apply employee discount successfully".
3. System stays on the "Discount" tab with the applied record moved to the top.

#### 18.10. Employee Discount Application Failure

**Prerequisite(s):**
1. The selected employee discount is configured to fail application.

**Step(s):**
1. Select the discount and click "Apply Discount".

**Test Result(s):**
1. System prompts error message "Fail to apply discount".
2. The action aborts and stays at the enquiry screen.
3. No employee discount is applied.

### 19. Clear or Replace Employee and Void Employee Discount

#### 19.1. Stored Employee Is Displayed on Reopening SVC Employee Enquiry

**Prerequisite(s):**
1. A check is attached with a Basic9 employee.

**Step(s):**
1. Open the check and click "SVC Employee Enquiry".

**Test Result(s):**
1. System triggers "Member Login" with the stored employee number.
2. On success, system triggers "Get Member Details".
3. Employee information is displayed in the Employee Detail tab.
4. The "Clear Employee" button is displayed.

#### 19.2. Another Employee Can Be Searched and Replaced

**Prerequisite(s):**
1. A check is attached with a Basic9 employee.
2. A different Basic9 employee exists.

**Step(s):**
1. Open "SVC Employee Enquiry" and search another employee.
2. Select the employee and click "Set Employee".

**Test Result(s):**
1. The employee enquiry workflow is performed for the new employee.
2. The attached employee is removed from the check.
3. The new employee is attached to the check.

#### 19.3. Clear Employee Confirmation "No" Aborts the Action

**Prerequisite(s):**
1. A Basic9 employee is attached to the check.

**Step(s):**
1. Click "Clear Employee".
2. Click "No" in the confirmation.

**Test Result(s):**
1. The action aborts and the workflow stops.
2. The employee remains attached.

#### 19.4. Partial Employee Payment Blocks Clearing Employee

**Prerequisite(s):**
1. A Basic9 employee is attached to the check.
2. A partial employee payment is applied to the check.

**Step(s):**
1. Click "Clear Employee" and confirm with "Yes".

**Test Result(s):**
1. System prompts error message "Not allow to perform function due to employee payment exists".
2. The action aborts.
3. The employee remains attached.

#### 19.5. Applied Employee Discounts Are Voided Automatically

**Prerequisite(s):**
1. A Basic9 employee is attached to the check.
2. Employee discounts are applied to the check.
3. No partial employee payment exists.

**Step(s):**
1. Click "Clear Employee" and confirm with "Yes".
2. Observe the employee discounts.

**Test Result(s):**
1. System voids all applied employee discounts automatically.
2. The discount entries are removed from the check.
3. The employee is removed from the check.

#### 19.6. Employee Discount Void Failure Aborts Clearing

**Prerequisite(s):**
1. The system is configured to fail voiding an applied employee discount.

**Step(s):**
1. Click "Clear Employee" and confirm with "Yes".

**Test Result(s):**
1. System prompts error message "Unable to void attached discount".
2. The action aborts and the workflow ends.
3. The employee remains attached.

#### 19.7. Employee and All Employee Discounts Are Removed from the Check

**Prerequisite(s):**
1. No partial employee payment exists.
2. All applied employee discounts are voided successfully.

**Step(s):**
1. Complete the clear employee confirmation.

**Test Result(s):**
1. System removes the attached employee from the check.
2. All employee discounts are voided.
3. The workflow ends.

#### 19.8. Member Login Connection Failure on Clear Employee

**Prerequisite(s):**
1. A check is attached with a Basic9 employee.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 19.9. Member Login Error Response on Clear Employee

**Prerequisite(s):**
1. A check is attached with a Basic9 employee.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open "SVC Employee Enquiry".

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No Get Member Details call is made.

#### 19.10. Get Member Details Connection Failure on Clear Employee

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Member Details.

**Step(s):**
1. Open "SVC Employee Enquiry" for the employee check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.

#### 19.11. Get Member Details Error Response on Clear Employee

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Member Details.

**Step(s):**
1. Open "SVC Employee Enquiry" for the employee check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.

#### 19.12. Set Employee Replaces the Attached Employee

**Prerequisite(s):**
1. A Basic9 employee is attached to the check.
2. Another employee is searched in the enquiry screen.

**Step(s):**
1. Click "Set Employee" for the searched employee.

**Test Result(s):**
1. The attached employee is removed from the check.
2. The searched employee is attached to the check.
3. The check shows the new employee information.

### 20. Payment Voucher Entry

#### 20.1. Payment Voucher Screen Displays Member Tabs and Member Information

**Prerequisite(s):**
1. A printed check is open.
2. The Basic9 payment method of type "Voucher" is configured.
3. The check is attached with a Basic9 member.

**Step(s):**
1. Click "Paid" to go to the cashier.
2. Select the Basic9 Voucher payment method.

**Test Result(s):**
1. The voucher enquiry screen is displayed with tabs "Member Voucher" and "Voucher".
2. "Member Voucher" is the default tab when the check is attached with a Basic9 member.
3. Member Number and Member Name are displayed at the top, with the name formatted as first name + space + last name.

#### 20.2. Payment Voucher Screen Hides Member Information Without a Member

**Prerequisite(s):**
1. The check is not attached with a Basic9 member.
2. The Basic9 Voucher payment method is selected.

**Step(s):**
1. Open the cashier and select the Basic9 Voucher payment method.

**Test Result(s):**
1. "Voucher" is the default tab.
2. Member Number and Member Name are displayed as empty values.
3. The voucher enquiry screen is displayed.

#### 20.3. Full Payment or Single Payment Setting Is Rejected

**Prerequisite(s):**
1. The Basic9 Voucher payment method has "Full Payment" = Yes or "Allow multiple payments" = No.

**Step(s):**
1. Open a printed check and click "Paid".
2. Select the Basic9 Voucher payment method.

**Test Result(s):**
1. System prompts error message "Full payment setting is not supported for Basic9 payment".
2. The action aborts and the workflow ends.

#### 20.4. Employee-Attached Check Blocks Voucher Payment

**Prerequisite(s):**
1. The check is attached with a Basic9 employee.

**Step(s):**
1. Open the printed check and select the Basic9 Voucher payment method.

**Test Result(s):**
1. System prompts error message "Not allow to perform function due to check is attached employee".
2. The action aborts and the workflow ends.

#### 20.5. Existing Voucher Blocks Another Voucher Payment

**Prerequisite(s):**
1. A Basic9 payment voucher was already applied to the check.

**Step(s):**
1. Open the check and select the Basic9 Voucher payment method.

**Test Result(s):**
1. System prompts error message "Coupon or voucher exists".
2. The action aborts and the workflow ends.

#### 20.6. Payment Voucher Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.

**Step(s):**
1. Open a printed check and select the Basic9 Voucher payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 20.7. Member Voucher Tab Opens Member Voucher Payment Workflow

**Prerequisite(s):**
1. The voucher enquiry screen is displayed.

**Step(s):**
1. Click the "Member Voucher" tab.

**Test Result(s):**
1. The member voucher payment workflow is opened.
2. Member vouchers are retrieved for the attached member.

#### 20.8. Voucher Tab Opens Voucher Payment Workflow

**Prerequisite(s):**
1. The voucher enquiry screen is displayed.

**Step(s):**
1. Click the "Voucher" tab.

**Test Result(s):**
1. The voucher payment workflow is opened.
2. The voucher number input and camera scan options are available.

### 21. Member Voucher Payment

#### 21.1. Member Vouchers Are Applied and Redeemed Successfully

**Prerequisite(s):**
1. The check is attached with a Basic9 member and the check is printed.
2. The Basic9 payment method of type "Voucher" is configured with multiple payments allowed.
3. The member has valid vouchers in the middleware and the check has a remaining balance.

**Step(s):**
1. Go to the cashier and open the "Member Voucher" tab.
2. Select one or more vouchers and click "Apply Voucher".
3. Settle the check and let Redeem Voucher complete.

**Test Result(s):**
1. System triggers Member Login and Query Vouchers for the attached member.
2. Voucher number, voucher name, amount, term over amount, term start date, and term end date are displayed for each voucher.
3. System triggers Prepare Redeem Voucher, adds the voucher payment to the basket, and triggers Redeem Voucher after settlement.
4. Request ID and voucher number are stored for the payment.
5. The existing payment operation workflow continues.

#### 21.2. Member Voucher Payment Requires an Attached Member

**Prerequisite(s):**
1. The check is not attached with a Basic9 member.
2. The "Member Voucher" tab is selected.

**Step(s):**
1. Open the member voucher payment screen.

**Test Result(s):**
1. System displays warning message "Member vouchers are not available, please set member".
2. The action aborts and the workflow stops.

#### 21.3. Member Login Connection Failure on Member Voucher Payment

**Prerequisite(s):**
1. A Basic9 member is attached to the check.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Open the "Member Voucher" tab.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.

#### 21.4. Member Login Error Response on Member Voucher Payment

**Prerequisite(s):**
1. A Basic9 member is attached to the check.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Open the "Member Voucher" tab.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.

#### 21.5. Query Vouchers Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Query Vouchers.

**Step(s):**
1. Open the "Member Voucher" tab.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. No voucher list is displayed.

#### 21.6. Query Vouchers Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Query Vouchers.

**Step(s):**
1. Open the "Member Voucher" tab.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. No voucher list is displayed.

#### 21.7. Voucher Amount Not Set Blocks Payment

**Prerequisite(s):**
1. The selected voucher has no payment amount configured in the middleware.

**Step(s):**
1. Select the voucher and click "Apply Voucher".

**Test Result(s):**
1. System prompts error message "Payment amount is not set for selected voucher".
2. The action aborts and stays at the enquiry screen.
3. No voucher payment is added.

#### 21.8. Voucher Amount Larger Than Remaining Balance Uses Remaining Balance

**Prerequisite(s):**
1. The selected voucher amount is larger than the check remaining balance.
2. The payment method tips or residue setting is configured.

**Step(s):**
1. Select the voucher and click "Apply Voucher".
2. Observe the payment amount and change.

**Test Result(s):**
1. The check remaining balance is used as the payment amount.
2. The remaining returned payment value is handled as tips or residue according to payment method setup.
3. Payment change is always 0.

#### 21.9. Prepare Redeem Voucher Connection Failure Moves to Next Voucher

**Prerequisite(s):**
1. Multiple vouchers are selected.
2. The middleware test endpoint fails the connection for Prepare Redeem Voucher.

**Step(s):**
1. Click "Apply Voucher".
2. Observe the message and the next selected voucher.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to the next selected voucher.
3. No payment is added for the failed voucher.

#### 21.10. Prepare Redeem Voucher Error Moves to Next Voucher

**Prerequisite(s):**
1. Multiple vouchers are selected.
2. The middleware test endpoint returns an error code for Prepare Redeem Voucher.

**Step(s):**
1. Click "Apply Voucher".
2. Observe the message and the next selected voucher.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to the next selected voucher.
3. No payment is added for the failed voucher.

#### 21.11. Redeem Voucher Connection Failure Cancels the Voucher

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds.
2. The middleware test endpoint fails the connection for Redeem Voucher.

**Step(s):**
1. Settle the check and let Redeem Voucher fail.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 26 and triggers "Cancel Redeem Voucher".
3. The action aborts and the workflow stops.

#### 21.12. Redeem Voucher Error Response Cancels the Voucher

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds.
2. The middleware test endpoint returns an error code for Redeem Voucher.

**Step(s):**
1. Settle the check and let Redeem Voucher return an error.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 26 and triggers "Cancel Redeem Voucher".
3. The action aborts and the workflow stops.

#### 21.13. Redeem Voucher Success Stores Payment Information

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds.
2. Redeem Voucher returns success.

**Step(s):**
1. Complete the settlement.

**Test Result(s):**
1. System stores Request ID and voucher number for the payment.
2. The existing payment operation workflow continues.
3. The voucher payment is completed.

#### 21.14. Cancel Redeem Voucher Is Triggered After Redemption Failure

**Prerequisite(s):**
1. Prepare Redeem Voucher returns a request ID.
2. Redeem Voucher fails by connection failure or error response.

**Step(s):**
1. Observe the system handling after the failure.

**Test Result(s):**
1. System triggers "Cancel Redeem Voucher" with the request ID from Prepare Redeem Voucher.
2. The result of Cancel Redeem Voucher is not handled.
3. The action aborts and the workflow stops.

### 22. Voucher Payment

#### 22.1. Manual or Scanned Voucher Is Applied and Redeemed Successfully

**Prerequisite(s):**
1. A printed check is open with a remaining balance.
2. The Basic9 payment method of type "Voucher" is configured with multiple payments allowed.
3. A valid voucher exists in the middleware.

**Step(s):**
1. Input the voucher number and click "Apply", or scan the voucher number by camera.
2. Click "Apply Voucher".
3. Settle the check and let Redeem Voucher complete.

**Test Result(s):**
1. System triggers Prepare Redeem Voucher with the input voucher number.
2. Voucher number, voucher name, amount, term over amount, term start date, and term end date are displayed for the first result voucher.
3. The payment is added to the cashier basket and the check is settled.
4. Redeem Voucher stores Request ID and voucher number.

#### 22.2. Prepare Redeem Voucher Connection Failure

**Prerequisite(s):**
1. A voucher number is input or scanned.
2. The middleware test endpoint fails the connection for Prepare Redeem Voucher.

**Step(s):**
1. Click "Apply" or scan the voucher.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and stays at the enquiry screen.
3. The workflow ends.

#### 22.3. Prepare Redeem Voucher Error Response

**Prerequisite(s):**
1. A voucher number is input or scanned.
2. The middleware test endpoint returns an error code for Prepare Redeem Voucher.

**Step(s):**
1. Click "Apply" or scan the voucher.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and stays at the enquiry screen.
3. The workflow ends.

#### 22.4. Cancelling Voucher Payment Without Saving Triggers Cancel

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds.
2. The user cancels the payment and exits the cashier without saving payment.

**Step(s):**
1. Apply the voucher information and then cancel the payment without saving.
2. Observe the API call.

**Test Result(s):**
1. System goes to step 21.
2. System triggers "Cancel Redeem Voucher" with the request ID from Prepare Redeem Voucher.
3. The result of Cancel Redeem Voucher is not handled and the workflow stops.

#### 22.5. Voucher Amount Not Set Blocks Voucher Payment

**Prerequisite(s):**
1. The first result voucher has no payment amount configured.

**Step(s):**
1. Input the voucher number and click "Apply Voucher".

**Test Result(s):**
1. System prompts error message "Payment amount is not set for selected voucher".
2. The action aborts and stays at the enquiry screen.
3. No payment is added.

#### 22.6. Voucher Amount Larger Than Remaining Balance Uses Remaining Balance

**Prerequisite(s):**
1. The voucher amount is larger than the check remaining balance.
2. The payment method tips or residue setting is configured.

**Step(s):**
1. Apply the voucher and observe the payment amount.

**Test Result(s):**
1. The check remaining balance is used as the payment amount.
2. The remaining returned payment value is handled as tips or residue according to payment method setup.
3. Payment change is always 0.

#### 22.7. Redeem Voucher Connection Failure Cancels the Payment

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds and the check is settled.
2. The middleware test endpoint fails the connection for Redeem Voucher.

**Step(s):**
1. Let Redeem Voucher fail.
2. Observe the handling.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 18. If any payment applied before triggered API redemption, system voids all payments and goes to step 21.
3. System triggers "Cancel Redeem Voucher" with the request ID from Prepare Redeem Voucher.

#### 22.8. Redeem Voucher Error Response Cancels the Payment

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds and the check is settled.
2. The middleware test endpoint returns an error code for Redeem Voucher.

**Step(s):**
1. Let Redeem Voucher return an error.
2. Observe the handling.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 18. If any payment applied before triggered API redemption, system voids all payments and goes to step 21.
3. System triggers "Cancel Redeem Voucher" with the request ID from Prepare Redeem Voucher.

#### 22.9. Prior Payments Are Voided When Redemption Fails

**Prerequisite(s):**
1. A payment was applied before the Redeem Voucher call was triggered.
2. Redeem Voucher fails.

**Step(s):**
1. Observe the other payments on the check.

**Test Result(s):**
1. System triggers void redemption for all payments.
2. System goes to step 21 and triggers Cancel Redeem Voucher.
3. The workflow stops.

#### 22.10. Redeem Voucher Success Stores Payment Information

**Prerequisite(s):**
1. Prepare Redeem Voucher succeeds.
2. Redeem Voucher returns success.

**Step(s):**
1. Complete the settlement.

**Test Result(s):**
1. System stores Request ID and voucher number for the payment.
2. The existing payment operation workflow continues.

### 23. Void Payment Voucher

#### 23.1. Payment Voucher Is Refunded Successfully

**Prerequisite(s):**
1. A paid check has a Basic9 payment voucher applied.
2. Member Login, Prepare Refund Voucher, and Refund Voucher succeed.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Click "Release Payment" or "Void Payment" for partial payment.
3. Confirm the action, select a void reason, and complete the refund.

**Test Result(s):**
1. System triggers Member Login with the stored card number.
2. System triggers Prepare Refund Voucher and then Refund Voucher with the stored voucher number and request ID.
3. The existing release payment operation workflow continues.

#### 23.2. Member Login Connection Failure on Void Payment Voucher

**Prerequisite(s):**
1. The check has a Basic9 payment voucher applied.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 23.3. Member Login Error Response on Void Payment Voucher

**Prerequisite(s):**
1. The check has a Basic9 payment voucher applied.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 23.4. Prepare Refund Voucher Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Refund Voucher.

**Step(s):**
1. Release the payment for the voucher check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 23.5. Prepare Refund Voucher Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Refund Voucher.

**Step(s):**
1. Release the payment for the voucher check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 23.6. Refund Voucher Connection Failure Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Voucher succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Refund Voucher.

**Step(s):**
1. Release the payment for the voucher check.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 18 and triggers "Cancel Refund Voucher".
3. The result of Cancel Refund Voucher is not handled and the workflow stops.

#### 23.7. Refund Voucher Error Response Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Voucher succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Refund Voucher.

**Step(s):**
1. Release the payment for the voucher check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 18 and triggers "Cancel Refund Voucher".
3. The result of Cancel Refund Voucher is not handled and the workflow stops.

#### 23.8. Cancel Refund Voucher Is Triggered with Stored Voucher Number and Request ID

**Prerequisite(s):**
1. Prepare Refund Voucher returns a request ID.
2. Refund Voucher fails.

**Step(s):**
1. Observe the Cancel Refund Voucher request.

**Test Result(s):**
1. System triggers "Cancel Refund Voucher" with the stored voucher number and the request ID from Prepare Refund Voucher.
2. The result of Cancel Refund Voucher is not handled.
3. The action aborts and the workflow stops.

### 24. Point Redemption Payment

#### 24.1. Point Redemption Payment Is Completed Successfully

**Prerequisite(s):**
1. A printed check is attached with a Basic9 member and has a remaining balance.
2. The Basic9 payment method of type "Point" is configured with multiple payments allowed.
3. The member has sufficient points in the middleware.

**Step(s):**
1. Go to the cashier and select the Basic9 Point payment method.
2. Let Member Login and Get Points complete.
3. Enter a valid payment amount and confirm.
4. Settle the check and let Redeem Point complete.

**Test Result(s):**
1. System triggers Member Login and Get Points and uses the returned point total as the payment amount.
2. The payment is added to the cashier basket without tips.
3. System triggers Prepare Redeem Point and then Redeem Point with the request ID.
4. Request ID, Point redeem, and Interface ID are stored for the payment.
5. The existing payment operation workflow continues.

#### 24.2. Full Payment or Single Payment Setting Is Rejected for Point Payment

**Prerequisite(s):**
1. The Basic9 Point payment method has "Full Payment" = Yes or "Allow multiple payments" = No.

**Step(s):**
1. Open a printed check and select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "Full payment setting is not supported for Basic9 payment".
2. The action aborts and the workflow ends.

#### 24.3. Employee-Attached Check Blocks Point Payment

**Prerequisite(s):**
1. The check is attached with a Basic9 employee.

**Step(s):**
1. Open the printed check and select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "Not allow to perform function due to check is attached employee".
2. The action aborts and the workflow ends.

#### 24.4. Point Payment Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.

**Step(s):**
1. Open a printed check and select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 24.5. Point Payment Requires an Attached Member

**Prerequisite(s):**
1. The check is not attached with a Basic9 member.

**Step(s):**
1. Open the printed check and select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "No member is attached".
2. The action aborts and the workflow ends.

#### 24.6. Member Login Connection Failure on Point Payment

**Prerequisite(s):**
1. A Basic9 member is attached to the check.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 24.7. Member Login Error Response on Point Payment

**Prerequisite(s):**
1. A Basic9 member is attached to the check.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 24.8. Get Points Connection Failure on Point Payment

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Get Points.

**Step(s):**
1. Select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 24.9. Get Points Error Response on Point Payment

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Get Points.

**Step(s):**
1. Select the Basic9 Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 24.10. Point Total Larger Than Remaining Balance Defaults to Remaining Balance

**Prerequisite(s):**
1. The returned point total is larger than the check remaining balance.

**Step(s):**
1. Select the Basic9 Point payment method.
2. Observe the default payment amount.

**Test Result(s):**
1. The check remaining balance is used as the default display of the payment amount.
2. The user is prompted to input the desired payment amount.

#### 24.11. Input Above the Limit Shows Invalid Input

**Prerequisite(s):**
1. The returned point total and the check remaining balance define the maximum allowed amount.

**Step(s):**
1. Input a payment amount larger than the smaller of the returned point total and the check remaining balance.
2. Confirm.

**Test Result(s):**
1. System prompts warning "Invalid input".
2. The user is prompted to input the payment amount again.
3. No payment is added.

#### 24.12. Valid Point Payment Amount Is Accepted

**Prerequisite(s):**
1. The input payment amount is smaller than or equal to the smaller of the returned point total and the check remaining balance.

**Step(s):**
1. Input the amount and confirm.
2. Observe the API call.

**Test Result(s):**
1. System uses the input amount as the payment amount.
2. System triggers Prepare Redeem Point with the input payment amount.
3. On success, the payment is added to the cashier basket without tips.

#### 24.13. Prepare Redeem Point Connection Failure

**Prerequisite(s):**
1. A valid payment amount is input.
2. The middleware test endpoint fails the connection for Prepare Redeem Point.

**Step(s):**
1. Confirm the payment amount.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 24.14. Prepare Redeem Point Error Response

**Prerequisite(s):**
1. A valid payment amount is input.
2. The middleware test endpoint returns an error code for Prepare Redeem Point.

**Step(s):**
1. Confirm the payment amount.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 24.15. Point Payment Is Added Without Tips

**Prerequisite(s):**
1. Prepare Redeem Point succeeds.
2. The payment method has a tips setting configured.

**Step(s):**
1. Observe the payment added to the cashier basket.

**Test Result(s):**
1. The payment is added without tips.
2. The payment method tips setting is ignored.

#### 24.16. Cancelling Point Payment Triggers Cancel Prepare Redeem Point

**Prerequisite(s):**
1. Prepare Redeem Point succeeds and returns a request ID.
2. The user cancels the payment or exits the cashier without saving.

**Step(s):**
1. Cancel the payment.

**Test Result(s):**
1. System goes to step 30.
2. System triggers "Cancel Prepare Redeem Point" with the input payment amount and the request ID.
3. The result is not handled and the workflow stops.

#### 24.17. Redeem Point Connection Failure Voids All Payments

**Prerequisite(s):**
1. Prepare Redeem Point succeeds and the payment is saved.
2. The middleware test endpoint fails the connection for Redeem Point.

**Step(s):**
1. Let Redeem Point fail.
2. Observe the payment status.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 29 and triggers void redemption for all payments if any payment applied before triggered API redemption.
3. System triggers "Cancel Prepare Redeem Point" with the request ID.

#### 24.18. Redeem Point Error Response Voids All Payments

**Prerequisite(s):**
1. Prepare Redeem Point succeeds and the payment is saved.
2. The middleware test endpoint returns an error code for Redeem Point.

**Step(s):**
1. Let Redeem Point return an error.
2. Observe the payment status.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 29 and triggers void redemption for all payments if applicable.
3. System triggers "Cancel Prepare Redeem Point" with the request ID.

#### 24.19. Redeem Point Success Stores Payment Information

**Prerequisite(s):**
1. Prepare Redeem Point succeeds.
2. Redeem Point returns success.

**Step(s):**
1. Complete the payment.

**Test Result(s):**
1. System stores Request ID, Point redeem, and Interface ID for the payment.
2. The existing payment operation workflow continues.

#### 24.20. Cancel Prepare Redeem Point Is Triggered on Cancellation

**Prerequisite(s):**
1. Prepare Redeem Point returns a request ID.
2. The payment is cancelled or redemption fails.

**Step(s):**
1. Observe the Cancel Prepare Redeem Point request.

**Test Result(s):**
1. System triggers "Cancel Prepare Redeem Point" with the payment amount from step 16 and the request ID.
2. The result of the call is not handled.
3. The action aborts and the workflow stops.

### 25. Void Payment Point Redemption

#### 25.1. Point Redemption Is Reversed Successfully

**Prerequisite(s):**
1. A paid check has a Basic9 point redemption payment.
2. Member Login, Prepare Reverse Redeem Point, and Reverse Redeem Point succeed.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Click "Release Payment" or "Void Payment" for partial payment.
3. Confirm, select a void reason, and complete the reversal.

**Test Result(s):**
1. System triggers Member Login with the stored card number.
2. System triggers Prepare Reverse Redeem Point and then Reverse Redeem Point with the stored payment amount and request ID.
3. The existing release payment operation workflow continues.

#### 25.2. Member Login Connection Failure on Point Reversal

**Prerequisite(s):**
1. The check has a Basic9 point redemption payment.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 25.3. Member Login Error Response on Point Reversal

**Prerequisite(s):**
1. The check has a Basic9 point redemption payment.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 25.4. Prepare Reverse Redeem Point Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Reverse Redeem Point.

**Step(s):**
1. Release the payment for the point check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 25.5. Prepare Reverse Redeem Point Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Reverse Redeem Point.

**Step(s):**
1. Release the payment for the point check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 25.6. Reverse Redeem Point Connection Failure Cancels the Reversal

**Prerequisite(s):**
1. Prepare Reverse Redeem Point succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Reverse Redeem Point.

**Step(s):**
1. Release the payment for the point check.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 18 and triggers "Cancel Prepare Reverse Redeem Point".
3. The result is not handled and the workflow stops.

#### 25.7. Reverse Redeem Point Error Response Cancels the Reversal

**Prerequisite(s):**
1. Prepare Reverse Redeem Point succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Reverse Redeem Point.

**Step(s):**
1. Release the payment for the point check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 18 and triggers "Cancel Prepare Reverse Redeem Point".
3. The result is not handled and the workflow stops.

#### 25.8. Cancel Prepare Reverse Redeem Point Is Triggered

**Prerequisite(s):**
1. Prepare Reverse Redeem Point returns a request ID.
2. Reverse Redeem Point fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Prepare Reverse Redeem Point" with the stored payment amount and the request ID.
2. The result is not handled.
3. The action aborts and the workflow stops.

### 26. Member Earn Point

#### 26.1. Member Earn Points Successfully

**Prerequisite(s):**
1. A printed check is attached with a Basic9 member and is fully settled.
2. Member Login and Prepare Earn Points succeed.
3. The check is not yet awarded Basic9 points.

**Step(s):**
1. Go to the cashier and select the Basic9 payment method of type "Member Earn Point".
2. Let Prepare Earn Points and Earn Points complete.

**Test Result(s):**
1. System triggers Member Login with the member card number.
2. System triggers Prepare Earn Points and then Earn Points with the request ID.
3. System stores Request ID, Points Earn, and Points Total for the check.
4. The existing payment operation workflow continues.

#### 26.2. Member Earn Point Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.
2. A settled member check is open.

**Step(s):**
1. Select the Basic9 Member Earn Point payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 26.3. Member Login Connection Failure on Earn Point

**Prerequisite(s):**
1. A Basic9 member is attached to the settled check.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Select the Basic9 Member Earn Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 26.4. Member Login Error Response on Earn Point

**Prerequisite(s):**
1. A Basic9 member is attached to the settled check.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Select the Basic9 Member Earn Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 26.5. Prepare Earn Points Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Earn Points.

**Step(s):**
1. Select the Basic9 Member Earn Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 26.6. Prepare Earn Points Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Earn Points.

**Step(s):**
1. Select the Basic9 Member Earn Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 26.7. Earn Points Connection Failure Cancels the Earn Point Action

**Prerequisite(s):**
1. Prepare Earn Points succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Earn Points.

**Step(s):**
1. Let Earn Points fail.
2. Observe the handling.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 16 and triggers "Cancel Earn Points".
3. The action aborts and the workflow stops.

#### 26.8. Earn Points Error Response Cancels the Earn Point Action

**Prerequisite(s):**
1. Prepare Earn Points succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Earn Points.

**Step(s):**
1. Let Earn Points return an error.
2. Observe the handling.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 16 and triggers "Cancel Earn Points".
3. The action aborts and the workflow stops.

#### 26.9. Cancel Earn Points Is Triggered After Failure

**Prerequisite(s):**
1. Prepare Earn Points returns a request ID.
2. Earn Points fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Earn Points" with the request ID from Prepare Earn Points.
2. The result is not handled.
3. The action aborts and the workflow stops.

### 27. Employee Earn Point

#### 27.1. Employee Earn Points Successfully

**Prerequisite(s):**
1. A printed check is attached with a Basic9 employee and is fully settled.
2. The check contains an employee payment with item code HU or ENT.
3. Member Login and Prepare Earn Points succeed.

**Step(s):**
1. Select the Basic9 payment method of type "Employee Earn Point".
2. Let Prepare Earn Points and Earn Points complete.

**Test Result(s):**
1. System triggers Member Login with the employee number.
2. System triggers Prepare Earn Points with the item payload: Item type = Payment, sub type = Regular, item code = HU or ENT.
3. System triggers Earn Points with the request ID and stores Request ID, Points Earn, and Points Total.
4. The existing payment operation workflow continues.

#### 27.2. Employee Earn Point Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.
2. A settled employee check is open.

**Step(s):**
1. Select the Basic9 Employee Earn Point payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 27.3. Member Login Connection Failure on Employee Earn Point

**Prerequisite(s):**
1. A Basic9 employee is attached to the settled check.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Select the Basic9 Employee Earn Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 27.4. Member Login Error Response on Employee Earn Point

**Prerequisite(s):**
1. A Basic9 employee is attached to the settled check.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Select the Basic9 Employee Earn Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 27.5. Prepare Earn Points Connection Failure on Employee Earn Point

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Earn Points.

**Step(s):**
1. Select the Basic9 Employee Earn Point payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 27.6. Prepare Earn Points Error Response on Employee Earn Point

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Earn Points.

**Step(s):**
1. Select the Basic9 Employee Earn Point payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 27.7. Earn Points Connection Failure on Employee Earn Point

**Prerequisite(s):**
1. Prepare Earn Points succeeds.
2. The middleware test endpoint fails the connection for Earn Points.

**Step(s):**
1. Let Earn Points fail.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 16 and triggers "Cancel Earn Points".
3. The action aborts and the workflow stops.

#### 27.8. Earn Points Error Response on Employee Earn Point

**Prerequisite(s):**
1. Prepare Earn Points succeeds.
2. The middleware test endpoint returns an error code for Earn Points.

**Step(s):**
1. Let Earn Points return an error.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 16 and triggers "Cancel Earn Points".
3. The action aborts and the workflow stops.

#### 27.9. Cancel Earn Points Is Triggered for Employee Earn Point

**Prerequisite(s):**
1. Prepare Earn Points returns a request ID.
2. Earn Points fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Earn Points" with the request ID from Prepare Earn Points.
2. The item payload uses Item type = Payment, sub type = Regular, and item code = HU or ENT.
3. The result is not handled and the workflow stops.

### 28. Void Earn Point

#### 28.1. Earned Points Are Reversed Successfully

**Prerequisite(s):**
1. A paid check has Basic9 points earned.
2. Member Login, Prepare Reverse Earn Points, and Reverse Earn Points succeed.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Click "Release Payment" or "Void Payment" for partial payment.
3. Confirm, select a void reason, and complete the reversal.

**Test Result(s):**
1. System triggers Member Login with the stored card number or employee number.
2. System triggers Prepare Reverse Earn Points and then Reverse Earn Points with the stored request ID.
3. The existing release payment operation workflow continues.

#### 28.2. Member Login Connection Failure on Void Earn Point

**Prerequisite(s):**
1. The check has Basic9 points earned.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 28.3. Member Login Error Response on Void Earn Point

**Prerequisite(s):**
1. The check has Basic9 points earned.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 28.4. Prepare Reverse Earn Points Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Reverse Earn Points.

**Step(s):**
1. Release the payment for the earn point check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 28.5. Prepare Reverse Earn Points Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Reverse Earn Points.

**Step(s):**
1. Release the payment for the earn point check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 28.6. Reverse Earn Points Connection Failure Cancels the Reversal

**Prerequisite(s):**
1. Prepare Reverse Earn Points succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Reverse Earn Points.

**Step(s):**
1. Release the payment for the earn point check.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 18 and triggers "Cancel Reverse Earn Points".
3. The result is not handled and the workflow stops.

#### 28.7. Reverse Earn Points Error Response Cancels the Reversal

**Prerequisite(s):**
1. Prepare Reverse Earn Points succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Reverse Earn Points.

**Step(s):**
1. Release the payment for the earn point check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 18 and triggers "Cancel Reverse Earn Points".
3. The result is not handled and the workflow stops.

#### 28.8. Cancel Reverse Earn Points Is Triggered After Failure

**Prerequisite(s):**
1. Prepare Reverse Earn Points returns a request ID.
2. Reverse Earn Points fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Reverse Earn Points" with the stored request ID.
2. The result is not handled.
3. The action aborts and the workflow stops.

#### 28.9. Employee Earn Point Reversal Uses the Employee Item Payload

**Prerequisite(s):**
1. The reversed earn point belongs to a Basic9 employee check with item code HU or ENT.

**Step(s):**
1. Release the employee payment that earned points.
2. Observe the reversal payload.

**Test Result(s):**
1. The item payload uses Item type = Payment, sub type = Regular, and item code = HU or ENT.
2. The stored request ID is used for Prepare Reverse Earn Points.
3. The reversal follows the existing release payment operation workflow.

### 29. Discount Redemption

#### 29.1. Discount Is Redeemed Successfully

**Prerequisite(s):**
1. A printed check is fully settled and has a Basic9 member or employee discount, coupon, voucher, or employee discount applied.
2. Member Login and Prepare Redeem Discount succeed.

**Step(s):**
1. Select the Basic9 payment method of type "Discount" and let the redemption run.
2. Let Prepare Redeem Discount and Redeem Discount complete.

**Test Result(s):**
1. System triggers Member Login with the stored member or employee number.
2. System triggers Prepare Redeem Discount with the discount item payload and then Redeem Discount with the request ID.
3. System stores Request ID and the redeemed discount values for the check.
4. The existing payment operation workflow continues.

#### 29.2. Discount Redemption Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.
2. A settled check with Basic9 discounts is open.

**Step(s):**
1. Select the Basic9 Discount payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 29.3. Member Login Connection Failure on Discount Redemption

**Prerequisite(s):**
1. The check has Basic9 discounts applied.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Select the Basic9 Discount payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 29.4. Member Login Error Response on Discount Redemption

**Prerequisite(s):**
1. The check has Basic9 discounts applied.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Select the Basic9 Discount payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 29.5. Prepare Redeem Discount Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Redeem Discount.

**Step(s):**
1. Select the Basic9 Discount payment method.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 29.6. Prepare Redeem Discount Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Redeem Discount.

**Step(s):**
1. Select the Basic9 Discount payment method.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 29.7. Redeem Discount Connection Failure Cancels the Redemption

**Prerequisite(s):**
1. Prepare Redeem Discount succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Redeem Discount.

**Step(s):**
1. Let Redeem Discount fail.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 16 and triggers "Cancel Redeem Discount".
3. The result is not handled and the workflow stops.

#### 29.8. Redeem Discount Error Response Cancels the Redemption

**Prerequisite(s):**
1. Prepare Redeem Discount succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Redeem Discount.

**Step(s):**
1. Let Redeem Discount return an error.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 16 and triggers "Cancel Redeem Discount".
3. The result is not handled and the workflow stops.

#### 29.9. Redeem Discount Success Stores the Request ID

**Prerequisite(s):**
1. Prepare Redeem Discount succeeds.
2. Redeem Discount returns success.

**Step(s):**
1. Complete the redemption.

**Test Result(s):**
1. System stores the Request ID and the redeemed discount values.
2. The existing payment operation workflow continues.

#### 29.10. Cancel Redeem Discount Is Triggered After Failure

**Prerequisite(s):**
1. Prepare Redeem Discount returns a request ID.
2. Redeem Discount fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Redeem Discount" with the request ID.
2. The result is not handled.
3. The action aborts and the workflow stops.

### 30. Void Discount Redemption

#### 30.1. Discount Redemption Is Refunded Successfully

**Prerequisite(s):**
1. A paid check has Basic9 redeemed discounts.
2. Member Login, Prepare Refund Discount, and Refund Discount succeed.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Click "Release Payment" or "Void Payment" for partial payment.
3. Confirm, select a void reason, and complete the refund.

**Test Result(s):**
1. System triggers Member Login with the stored member or employee number.
2. System triggers Prepare Refund Discount and then Refund Discount with the stored request ID and discount values.
3. The existing release payment operation workflow continues.

#### 30.2. Member Login Connection Failure on Discount Refund

**Prerequisite(s):**
1. The check has Basic9 redeemed discounts.
2. The middleware test endpoint fails the connection for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 30.3. Member Login Error Response on Discount Refund

**Prerequisite(s):**
1. The check has Basic9 redeemed discounts.
2. The middleware test endpoint returns an error code for Member Login.

**Step(s):**
1. Release the payment for the check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 30.4. Prepare Refund Discount Connection Failure

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint fails the connection for Prepare Refund Discount.

**Step(s):**
1. Release the payment for the discount check.

**Test Result(s):**
1. System prompts error message "No response".
2. The action aborts and the workflow ends.

#### 30.5. Prepare Refund Discount Error Response

**Prerequisite(s):**
1. Member Login succeeds.
2. The middleware test endpoint returns an error code for Prepare Refund Discount.

**Step(s):**
1. Release the payment for the discount check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. The action aborts and the workflow ends.

#### 30.6. Refund Discount Connection Failure Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Discount succeeds and returns a request ID.
2. The middleware test endpoint fails the connection for Refund Discount.

**Step(s):**
1. Release the payment for the discount check.

**Test Result(s):**
1. System prompts error message "No response".
2. System goes to step 18 and triggers "Cancel Refund Discount".
3. The result is not handled and the workflow stops.

#### 30.7. Refund Discount Error Response Cancels the Refund

**Prerequisite(s):**
1. Prepare Refund Discount succeeds and returns a request ID.
2. The middleware test endpoint returns an error code for Refund Discount.

**Step(s):**
1. Release the payment for the discount check.

**Test Result(s):**
1. System prompts the error message returned by the external party.
2. System goes to step 18 and triggers "Cancel Refund Discount".
3. The result is not handled and the workflow stops.

#### 30.8. Refund Discount Success Completes the Existing Void Flow

**Prerequisite(s):**
1. Prepare Refund Discount succeeds.
2. Refund Discount returns success.

**Step(s):**
1. Complete the refund.

**Test Result(s):**
1. The redeemed discounts are refunded.
2. The existing release payment operation workflow continues.

#### 30.9. Cancel Refund Discount Is Triggered After Failure

**Prerequisite(s):**
1. Prepare Refund Discount returns a request ID.
2. Refund Discount fails.

**Step(s):**
1. Observe the cancel request.

**Test Result(s):**
1. System triggers "Cancel Refund Discount" with the stored request ID.
2. The result is not handled.
3. The action aborts and the workflow stops.

### 31. Void Paid Check

#### 31.1. Void Paid Check Processes All Basic9 Redemptions

**Prerequisite(s):**
1. A paid check is attached with a Basic9 member or employee and contains voucher, point, discount, and earn point records.
2. No Basic9 coupon is applied to the check.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the paid check.
2. Click "Void Paid Check" or "Force Void Check".
3. Confirm the action, select a void reason, and complete the void.

**Test Result(s):**
1. System performs the void payment voucher workflow for each voucher.
2. System performs the void payment point redemption workflow for each point redemption and the void discount redemption workflow for member or employee discounts.
3. System performs the void earn point workflow and then the existing void paid check workflow.

#### 31.2. Basic9 Coupon Blocks Void Paid Check

**Prerequisite(s):**
1. A Basic9 coupon is applied to the paid check.

**Step(s):**
1. Go to the check listing and select the paid check.
2. Click "Void Paid Check" or "Force Void Check" and confirm.

**Test Result(s):**
1. System prompts error message "Action not allowed for check with Basic9 coupon".
2. The action aborts and the check remains paid.

#### 31.3. Payment Voucher Is Routed to Void Payment Voucher Workflow

**Prerequisite(s):**
1. The paid check contains a Basic9 payment voucher.

**Step(s):**
1. Void the paid check.

**Test Result(s):**
1. System performs the void payment voucher workflow for the voucher.
2. The voucher is refunded through the middleware.

#### 31.4. Point Redemption Is Routed to Void Point Redemption Workflow

**Prerequisite(s):**
1. The paid check contains a Basic9 point redemption payment.

**Step(s):**
1. Void the paid check.

**Test Result(s):**
1. System performs the void payment point redemption workflow.
2. The point redemption is reversed through the middleware.

#### 31.5. Member or Employee Discount Is Routed to Void Discount Redemption Workflow

**Prerequisite(s):**
1. The paid check contains a Basic9 member or employee discount.

**Step(s):**
1. Void the paid check.

**Test Result(s):**
1. System performs the void discount redemption workflow.
2. The discount is refunded through the middleware.

#### 31.6. Earn Point Is Routed to Void Earn Point Workflow

**Prerequisite(s):**
1. The paid check has Basic9 points earned.

**Step(s):**
1. Void the paid check.

**Test Result(s):**
1. System performs the void earn point workflow.
2. The earned points are reversed through the middleware.

### 32. Employee Payment

#### 32.1. Employee Payment Is Completed with an Attached Employee

**Prerequisite(s):**
1. A printed check is open with a remaining balance.
2. The Basic9 payment method of type "Employee" is configured with multiple payments allowed.
3. The check is attached with a Basic9 employee and the employee has a limit amount.

**Step(s):**
1. Go to the cashier and select the Basic9 Employee payment method.
2. Select "House Use" or "Entertainment" and click "Continue".
3. Input a valid payment amount and confirm.
4. Add the payment and settle the check.

**Test Result(s):**
1. The employee enquiry screen is displayed automatically with the attached employee in Employee Detail.
2. The search textboxes are hidden and the employee cannot be changed.
3. The payment is added to the cashier basket and the check is settled.
4. Interface ID and payment type code are stored for the payment.

#### 32.2. Full Payment or Single Payment Setting Is Rejected for Employee Payment

**Prerequisite(s):**
1. The Basic9 Employee payment method has "Full Payment" = Yes or "Allow multiple payments" = No.

**Step(s):**
1. Open a printed check and select the Basic9 Employee payment method.

**Test Result(s):**
1. System prompts error message "Full payment setting is not supported for Basic9 payment".
2. The action aborts and the workflow ends.

#### 32.3. Existing Basic9 Payment Blocks Employee Payment

**Prerequisite(s):**
1. The check already has a Basic9 payment applied.

**Step(s):**
1. Open the check and select the Basic9 Employee payment method.

**Test Result(s):**
1. System prompts error message "Basic9 payments applied before".
2. System prompts error message "Payment method not supported".
3. The action aborts and the workflow ends.

#### 32.4. Basic9 Member-Attached Check Blocks Employee Payment

**Prerequisite(s):**
1. The check is attached with a Basic9 member.

**Step(s):**
1. Open the check and select the Basic9 Employee payment method.

**Test Result(s):**
1. System prompts error message "Basic9 member attached".
2. System prompts error message "Payment method not supported".
3. The action aborts and the workflow ends.

#### 32.5. Employee Payment Requires a User Number

**Prerequisite(s):**
1. The logged-in user has no user number assigned.

**Step(s):**
1. Open a printed check and select the Basic9 Employee payment method.

**Test Result(s):**
1. System prompts error message "Missing user number setup".
2. The action aborts and the workflow ends.

#### 32.6. Attached Employee Locks the Employee Search

**Prerequisite(s):**
1. The check is attached with a Basic9 employee.
2. The Basic9 Employee payment method is selected.

**Step(s):**
1. Open the employee payment screen.
2. Attempt to search another employee.

**Test Result(s):**
1. The search textboxes and Search button are hidden.
2. Employee Detail is displayed for the attached employee.
3. The employee cannot be changed, and the Continue button is displayed.

#### 32.7. No Attached Employee Shows the Search Button

**Prerequisite(s):**
1. The check is not attached with a Basic9 employee.
2. The Basic9 Employee payment method is selected.

**Step(s):**
1. Open the employee payment screen.

**Test Result(s):**
1. The Search button is displayed.
2. The Set Employee button is displayed.
3. Employee information can be searched.

#### 32.8. Single Employee Result Disables Employee Listing Selection

**Prerequisite(s):**
1. The employee search returns exactly one employee.

**Step(s):**
1. Search the employee on the employee payment screen.
2. Attempt to select the record in Employee Listing.

**Test Result(s):**
1. Employee Listing shows the single returned record.
2. The record cannot be selected because selection is not required.
3. Employee Detail and the action button are displayed.

#### 32.9. Multiple Employee Results Require Selection

**Prerequisite(s):**
1. The employee search returns more than one employee.
2. Employee Listing Setup is configured.

**Step(s):**
1. Search by employee name.
2. Select a record in Employee Listing.

**Test Result(s):**
1. Employee Listing is displayed with multiple records according to Employee Listing Setup.
2. The user must select a record.
3. The selected record is shown in Employee Detail.

#### 32.10. Set Employee or Continue Button Follows Attachment State

**Prerequisite(s):**
1. One scenario has no attached employee and another scenario has an attached employee.

**Step(s):**
1. Open the employee payment screen without an attached employee.
2. Open the employee payment screen with an attached employee.

**Test Result(s):**
1. "Set Employee" is displayed when no employee is attached.
2. "Continue" is displayed when an employee is already attached.
3. Only the applicable button is enabled for the current state.

#### 32.11. House Use or Entertainment Must Be Selected

**Prerequisite(s):**
1. An employee is selected on the payment screen.
2. The check has a remaining balance.

**Step(s):**
1. Click "Continue" without selecting "House Use" or "Entertainment".

**Test Result(s):**
1. System prompts warning "Please select 'House Use' or 'Entertainment'".
2. The action aborts and stays on the selection screen.

#### 32.12. Payment Amount Defaults to the Check Remaining Balance

**Prerequisite(s):**
1. An employee is selected and "House Use" or "Entertainment" is chosen.
2. The employee limit amount is larger than the check remaining balance.

**Step(s):**
1. Proceed to the amount input screen.
2. Observe the default amount.

**Test Result(s):**
1. The payment amount defaults to the check remaining balance.
2. The user can change the amount.

#### 32.13. Cancelling Employee Payment Aborts the Action

**Prerequisite(s):**
1. The employee payment screen is displayed.

**Step(s):**
1. Click "Cancel".

**Test Result(s):**
1. The action aborts and the workflow stops.
2. No employee payment is added to the check.

#### 32.14. Amount Above Employee Limit Is Rejected

**Prerequisite(s):**
1. The input amount is larger than the employee limit amount or larger than the check remaining total.

**Step(s):**
1. Input the amount and click "Confirm".

**Test Result(s):**
1. System prompts error message "Payment amount exceeds employee limit amount or check remaining total".
2. The user clicks "OK" and returns to the amount input screen.
3. No payment is added.

#### 32.15. Valid Employee Payment Amount Is Added to the Basket

**Prerequisite(s):**
1. The input amount is within the employee limit amount and the check remaining total.

**Step(s):**
1. Input the amount and click "Confirm".

**Test Result(s):**
1. The employee payment is added to the cashier basket.
2. The check remaining balance is reduced accordingly.
3. The existing payment operation workflow continues.

#### 32.16. Employee Payment Type Code Is Stored

**Prerequisite(s):**
1. An employee payment is added with "House Use" or "Entertainment" selected.

**Step(s):**
1. Complete the payment.
2. Inspect the stored payment information.

**Test Result(s):**
1. Interface ID is stored for the payment.
2. Payment type code is stored as HU for House Use or ENT for Entertainment.
3. The stored value is used by the employee earn point and reversal payloads.

### 33. Merge Functions

#### 33.1. Target Check with Basic9 Member or Employee Blocks Merge

**Prerequisite(s):**
1. The current check and the target check are open for a merge operation.
2. The target check is attached with a Basic9 member or employee.

**Step(s):**
1. Use any merge function to merge the target check into the current check.

**Test Result(s):**
1. System prompts error message "Function not allowed due to Basic9 member or employee attached at check that will be merged to current check".
2. The merge action aborts.
3. Both checks remain unchanged.

#### 33.2. Target Check without Basic9 Membership Is Merged Normally

**Prerequisite(s):**
1. The target check is not attached with a Basic9 member or employee.
2. The current check is valid for the merge operation.

**Step(s):**
1. Use the same merge function.

**Test Result(s):**
1. The existing merge operation is performed.
2. The target check is merged into the current check.
3. No Basic9 restriction is triggered.

### 34. Split Functions Are Blocked for a Basic9 Check

**Prerequisite(s):**
1. The check is attached with a Basic9 member or employee.
2. Split Check, Split All Item, Split Single Item, or any other split function is available.

**Step(s):**
1. Use any split function on the check.

**Test Result(s):**
1. System prompts error message "Cannot perform this function to check with Basic9 member or employee".
2. The split action aborts.
3. The check remains unchanged.

### 35. Unsupported Admin Functions

#### 35.1. Adjust Tips Is Blocked for a Basic9 Check

**Prerequisite(s):**
1. A paid check is attached with a Basic9 member or employee.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Use the "Adjust Tips" function.

**Test Result(s):**
1. System prompts error message "Cannot perform this function to check with Basic9 member or employee".
2. The adjustment aborts.

#### 35.2. Adjust Payments Is Blocked for a Basic9 Check

**Prerequisite(s):**
1. A paid check is attached with a Basic9 member or employee.

**Step(s):**
1. Go to the check listing and select the check.
2. Use the "Adjust Payments" function.

**Test Result(s):**
1. System prompts error message "Cannot perform this function to check with Basic9 member or employee".
2. The adjustment aborts.

#### 35.3. Generate Negative Check Is Blocked for a Basic9 Check

**Prerequisite(s):**
1. A paid check is attached with a Basic9 member or employee.

**Step(s):**
1. Go to the check listing and select the check.
2. Use the "Generate Negative Check" function.

**Test Result(s):**
1. System prompts error message "Cannot perform this function to check with Basic9 member or employee".
2. The negative check is not generated.

### 36. Void Check Is Blocked for a Basic9 Check

**Prerequisite(s):**
1. A check is attached with a Basic9 member, employee, coupon, or voucher.

**Step(s):**
1. Go to Admin > Check Operation > Check Listing and select the check.
2. Use the "Void Check" function.

**Test Result(s):**
1. System prompts error message "Cannot void check attached with Basic9 member, employee, coupon or voucher".
2. The void action aborts.
3. The check remains unchanged.

### 37. Exit Check Is Blocked When a Basic9 Coupon Is Applied

**Prerequisite(s):**
1. A Basic9 member is set to the check.
2. A Basic9 coupon is applied to the check.
3. The check contains items with the coupon discount applied.

**Step(s):**
1. Click "Cancel" or the back button to exit the check without saving.
2. Observe the prompt and click "OK".

**Test Result(s):**
1. System prompts error message "NEW item <item names> with discount coupon applied, not allow to quit check. Please void discount coupon or send check".
2. The system stays on the ordering panel after "OK" is clicked.
3. The check is not exited and the applied coupon remains.

## Test Environment

Testing is performed on Infrasys POS in fine dining mode with a horizontal desktop ordering screen. The environment includes a Basic9 (MATE CRM) middleware test endpoint, a configured Basic9 vendor and membership interface attached to the test outlet, an MSR device, and guest check and receipt print formats containing the membership printing variables. API failure cases are exercised with middleware test stubs that return a connection failure or an error response for the targeted endpoint.

