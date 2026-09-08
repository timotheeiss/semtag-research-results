# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality visible or accessible in the UI for existing listings.
    - Actual: My Listings page shows only View, Mark Sold, and Delete buttons for each listing. Product detail page has no Edit button. Direct URL /edit/prod-1 returns 404 error. No UI path found to edit a listing's title, description, photos, price, category, condition, location, or upcycled status.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Unable to fully test due to page loading issues and session management.
    - Actual: My Listings page showed Mark Sold and Delete buttons for existing listings (prod-1, prod-5), but could not interact with them due to repeated page loading failures when accessing my-listings URL.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot test due to FT-3 not being testable and application page loading issues.
    - Actual: Depends on FT-3 (mark as sold/delete) functionality. Unable to create sold/deleted listings to test if they disappear from browse.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: Purchase request submitted without message input dialog and without visible confirmation.
    - Actual: Clicked 'Send Purchase Request' button in cart. Cart immediately emptied (0 items), but no message input dialog was shown, no message was collected from user, and no visible confirmation message was displayed. User received no feedback that the request was sent.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing page shows all items instead of filtering by selected category.
    - Actual: Clicked on 'Furniture' category from categories page (URL: ?category=furniture). Page navigated to browse results but showed all 10 items instead of only the 3 furniture items. Categories page showed Furniture had 3 items count, but browse results ignored the category filter.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Cannot test due to FT-9 failure (purchase request not working as specified) and session issues.
    - Actual: FT-9 showed that purchase requests are submitted but without message input or confirmation. Cannot test if sellers can view these requests due to page loading failures and auth session loss.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo accounts accept any password, allowing login with incorrect passwords.
    - Actual: Logged in with emma@example.com and password 'wrongpassword' successfully. The application redirected to home page and authenticated user is logged in (Sell button and Cart button visible). Application accepted incorrect password, when it should have rejected it.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Cannot test due to create-listing form not loading properly when authenticated.
    - Actual: Created listing successfully earlier with all required fields (FT-1 test). Attempted to test validation by accessing create-listing form, but form did not load (semantic snapshot empty, no form elements found). Unable to verify validation of missing required fields.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Cannot test due to create-listing form not loading properly.
    - Actual: Same as CS-14 - created listing successfully with valid price ($150) in FT-1, but cannot test invalid price validation due to form loading failures.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated access to create-listing page is not properly blocked.
    - Actual: Navigated to /create-listing while logged out. Page loaded (no redirect to login, no error) but with empty content. Should have either redirected to login page or shown clear sign-in prompt. Also attempted access while authenticated but form failed to load.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [ ] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.
  - Bug Report:
    - Issue: Listing does not display all required content fields.
    - Actual: Displays: photos, title ($65), category (Clothing), condition (Good), location (San Francisco, CA), seller (Marcus Chen), description, listing date (2024-11-18). Missing: Availability status (should show 'Available' or 'Sold'), Upcycled status (should show if product is upcycled or not).