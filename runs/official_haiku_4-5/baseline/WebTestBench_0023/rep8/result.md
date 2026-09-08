# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Edit functionality not found. No edit button visible on my-listings page or product detail page; /edit-listing/{id} returns 404.
    - Actual: Attempted to edit listing through UI buttons and direct URL navigation; no edit interface found.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Mark sold/delete functionality depends on edit features that are not implemented
    - Actual: No edit button found on my-listings page or product detail page. /edit-listing/{id} URL returns 404. Cannot access seller management actions for mark sold/delete without edit functionality.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot verify without FT-3 implementation
    - Actual: FT-3 failed (mark sold/delete not available). Cannot test if sold/deleted listings disappear from buyer browse results without ability to mark items sold or delete them.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing does not filter results correctly
    - Actual: Clicked Furniture from categories page (3 items). URL changed to /?category=furniture, but results still show "10 items available" with all 10 items displayed instead of only the 3 furniture items.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Cannot verify seller-side purchase request viewing due to account/listing access limitations
    - Actual: Submitted purchase request for Sony headphones from buyer account. Unable to verify if seller Sofia Martinez can view the request due to account limitations and time constraints. Feature may or may not be implemented on seller side.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Cannot verify duplicate email handling due to account persistence issues
    - Actual: Attempted to register with seller@test.com. Account appeared to create but didn't persist in system. Unable to register duplicate email to test rejection. Demo accounts cannot be used for this test.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Cannot verify password validation due to demo account behavior
    - Actual: Demo accounts (emma@example.com, etc.) accept any password as specified on login form: "(any password works)". Cannot test if incorrect passwords are rejected for real user accounts since demo is the only available test data.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Not tested due to time constraints and page rendering issues
    - Actual: Attempted to test by creating listing, but create-listing page had rendering issues preventing form interaction. Could not submit listing with missing required fields to verify validation error handling.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Not tested due to time constraints and page rendering issues
    - Actual: Could not test price validation (negative/non-numeric values) due to create-listing page rendering issues. Successfully created one listing with valid price ($250), but could not test invalid price rejection.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: Cannot verify immediate duplicate email feedback due to demo account system
    - Actual: Demo accounts cannot be duplicated for testing. Created seller@test.com account during testing, but account did not persist. Unable to register same email twice to test whether immediate duplicate-account feedback and form retention occurs.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.