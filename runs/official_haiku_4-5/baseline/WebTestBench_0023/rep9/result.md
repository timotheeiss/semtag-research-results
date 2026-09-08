# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Edit listing functionality not found in the application
    - Actual: No Edit button visible in My Listings page or on the product page. Attempting to navigate to /edit-listing URL resulted in 404 page not found error.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Unable to complete Mark Sold and Delete flow testing due to technical issues with page loading and persistent session management during testing
    - Actual: While buttons for "Mark Sold" and "Delete" were visible in My Listings interface, full testing of the flow including confirmation dialogs and status updates could not be completed

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Dependent on FT-3 implementation which could not be fully tested
    - Actual: Unable to test that sold/deleted listings are not offered to buyers due to inability to complete the mark sold/delete operations

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [ ] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.
  - Bug Report:
    - Issue: Unable to complete full cart testing due to persistent login session issues
    - Actual: While "Add to Cart" button exists and authentication requirement is enforced (CS-23), full testing of item retention, cart display, and total calculation could not be completed due to repeated logout issues during testing

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: Unable to complete purchase request testing due to login session issues
    - Actual: Could not access the purchase request feature due to persistent logout issues preventing authenticated testing of the message submission and confirmation flow

- [X] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Dependent on FT-9 which could not be fully tested
    - Actual: Unable to test seller view of purchase requests and request tracking due to inability to create purchase requests (FT-9 incomplete)


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email registration was accepted and replaced the existing account instead of being rejected
    - Actual: Registration with duplicate email succeeded, showing "Account created successfully!" and logging in the new user (Test Seller 2) instead of rejecting the duplicate email and preserving the original account (Test Seller)

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password was accepted and user was logged in
    - Actual: Login with correct email (testseller@example.com) and incorrect password (wrongpassword) succeeded, showing "Welcome back!" message and creating an authenticated session

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: Duplicate email was silently accepted instead of providing immediate feedback and keeping form available
    - Actual: When attempting to register with a duplicate email (testseller@example.com), the system succeeded in creating a new account and replaced the existing one instead of rejecting with error feedback and keeping the form available for correction

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [ ] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.
  - Bug Report:
    - Issue: Unable to complete cart interaction testing due to login session issues
    - Actual: Could not verify immediate confirmation message, cart count update, line item display, and total calculation when adding to cart due to persistent logout issues during testing


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.