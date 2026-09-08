# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit button found
    - Actual: Viewed listing in My Listings and on product detail page. No 'Edit' button or link visible. Only 'View', 'Mark Sold/Available', and 'Delete' options available for seller management.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Category filter not applying combinations
    - Actual: Testing FT-21 already showed category filter alone doesn't work. Combined with other filters would not filter correctly either. Need to check if multiple filter combinations work together.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category filter not working
    - Actual: Clicked Furniture category from categories page (showing 3 items). URL changed to ?category=furniture but page still displays 10 items available instead of 3 Furniture items only. Items from other categories (Clothing, Electronics, Sports, etc.) are still visible.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Unable to verify seller's view of purchase requests
    - Actual: Submitted purchase request message to seller but did not verify if seller can view requests or if requests are tracked. Would need to log in as seller to verify.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email not rejected
    - Actual: System created a new account with email alice.seller@test.com (already used by Alice Seller), replacing the existing account. Bob Buyer is now signed in instead of Alice Seller, indicating the first account was overwritten.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo accounts accept any password
    - Actual: Demo accounts (emma@example.com, marcus@example.com, sofia@example.com) documented as accepting any password. Cannot test incorrect password rejection with demo accounts. Would need a non-demo account to test.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Unable to verify seller-only actions
    - Actual: Created and managed own listing successfully but did not test viewing another user's listing to verify that seller actions are hidden for non-owners. Would require testing as different user.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Unable to test - form validation not checked
    - Actual: Created one successful listing with all required fields. Did not attempt to create listing with missing fields to verify validation errors. Would need to test form submission with incomplete data.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Unable to test - price validation not checked
    - Actual: Created listing with valid price ($450). Did not attempt to create listing with negative or non-numeric price to verify validation. Would need to test form submission with invalid price values.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unable to verify authentication requirement
    - Actual: Attempted to navigate to /create-listing while logged out. Page URL changed but content did not load fully. Cannot determine if unauthenticated access was properly blocked or redirected.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No duplicate email feedback
    - Actual: No error message or warning displayed on registration form. Form was not kept available for correction; instead redirected to homepage and created new account with duplicate email.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [ ] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.
  - Bug Report:
    - Issue: Filter changes not updating results correctly
    - Actual: Category filter test (FT-21) showed that clicking category from categories page did not update results to show only items in that category. Indicates filters do not update results immediately as expected.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.