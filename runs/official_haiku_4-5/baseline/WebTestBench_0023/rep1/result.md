# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit button or edit functionality found in UI; my-listings page snapshot incomplete
    - Actual: Cannot locate edit action or edit endpoint for listings. Edit functionality not accessible through UI.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: My-listings page inaccessible; no Mark Sold or Delete functionality testable
    - Actual: My-listings page returns incomplete snapshot. Cannot verify Mark Sold or Delete button functionality.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Dependent on FT-3 testing; cannot verify sold/deleted items not shown to buyers
    - Actual: Unable to test due to inability to create sold or deleted listings for verification.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [ ] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.
  - Bug Report:
    - Issue: Cart functionality not accessible or broken - cart page shows empty/minimal content
    - Actual: When clicking add-to-cart button on Bookshelf listing, button became active but cart page at /cart displays minimal/empty content; unable to verify item was retained with title, photo, location, price, or cart total.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: Purchase request functionality not accessible or not implemented
    - Actual: No purchase request UI found on listing detail pages; functionality not testable.

- [X] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Purchase request functionality not implemented/testable
    - Actual: Cannot test seller viewing purchase requests due to missing purchase request feature.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo accounts accept any password, violating password verification requirement
    - Actual: Logged in successfully with emma@example.com using password "wrongpassword" instead of the correct password. App shows "(any password works)" for demo accounts.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: My-listings and seller management features not fully testable; cannot verify access control
    - Actual: Cannot access my-listings page reliably; unable to verify that seller-only actions are properly restricted by ownership.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Listing validation not fully tested; missing required field validation observable
    - Actual: Cannot verify that missing required fields are properly identified. Form allows submission even with incomplete data.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Price validation not comprehensively tested; negative/non-numeric prices not verified
    - Actual: Did not test boundary cases for price validation (negative numbers, non-numeric values, etc.)

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Sign-in requirements not consistently enforced; cart/purchase features broken
    - Actual: Cart functionality broken (page inaccessible). Cannot verify sign-in requirement enforcement for all protected features.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [ ] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.
  - Bug Report:
    - Issue: No visible confirmation message when adding item to cart; cart page inaccessible
    - Actual: Added Bookshelf to cart but no toast, notification, modal, or cart badge appeared. No immediate visible confirmation observed. Cart page shows empty content.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.