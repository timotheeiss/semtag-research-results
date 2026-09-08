# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Edit functionality not implemented
    - Actual: No edit button available on my-listings page or product detail page. Attempting to navigate to /edit/{product-id} route returns 404 not-found error. Sellers cannot edit listing details.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category page browse results not filtered
    - Actual: Clicked Furniture from categories page. URL changed to /?category=furniture but results still show all 10 items (expected 3). Count shows "10 items available" instead of "3 items" as shown on category page. Filter checkbox method works (tested in FT-6) but category page navigation doesn't filter results properly.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller interface for viewing purchase requests not verified
    - Actual: Buyer submitted purchase request from cart (request accepted and cart cleared). However, unable to verify from seller perspective: no interface found for sellers to view purchase requests, requested items, or buyer messages. Incomplete implementation or insufficient UI for this requirement.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email not rejected or unclear error handling
    - Actual: Attempted registration with already-registered email 'alice.smith@test.com'. Form accepted submission and navigated to home page without error message. No clear rejection visible.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo accounts accept any password, preventing proper password validation testing
    - Actual: Demo accounts (emma@example.com, marcus@example.com, sofia@example.com) accept any password. Attempted login with correct email but wrong password ('wrongpassword123') succeeded and created authenticated session. This violates password validation requirement.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Missing required fields not identified or validated
    - Actual: Attempted to create listing without any required fields (no title, description, price, category, condition, location, or photos). Form was submitted without showing validation errors or identifying which required information was missing.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Negative/non-numeric prices not properly validated
    - Actual: Attempted to create listing with negative price (-50). Form submitted without showing validation error rejecting negative price. No user feedback provided about invalid price.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No visible duplicate email feedback during registration
    - Actual: Attempted registration with duplicate email 'alice.smith@test.com'. Form accepted submission and redirected to home without showing error message or feedback. No indication that duplicate email was rejected or that existing account was preserved.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.