# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Cannot test listing creation due to broken registration/authentication system
    - Actual: Registration form accepts input but does not create accounts (verified by attempting login with registered credentials). Clicking "Start Selling" navigates to registration page rather than a listing creation form. The /create-listing endpoint exists but returns empty page. Cannot verify FT-1 requirements due to these blocking issues.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Cannot test listing editing due to broken registration/authentication and no listing creation
    - Actual: Prerequisite test FT-1 cannot be completed due to app issues. Without ability to create listings, cannot test editing functionality.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Cannot test listing deletion/status change due to broken registration and no listing access
    - Actual: Blocked by FT-1 failure. Cannot access listings or seller management features.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot test sold/deleted listing visibility due to blocking issues with FT-3
    - Actual: Blocked by FT-3 failure.

- [ ] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.
  - Bug Report:
    - Issue: Account registration not created or not persisted
    - Actual: Registration form accepted input and redirected to home, but subsequent login attempt with same credentials failed, indicating account was never created or stored.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [ ] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.
  - Bug Report:
    - Issue: Search functionality does not filter listings by keyword
    - Actual: Typed "jacket" and "bookshelf" in search input, but product list remained unchanged with all 10 items still showing. Search input value does not appear to be stored or sent to filter backend.

- [ ] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.
  - Bug Report:
    - Issue: Cannot test cart functionality due to broken session management and authentication
    - Actual: Attempted to add items to cart. Authentication redirect works (Add to Cart redirects to login), but after logging in, session does not persist. Subsequent cart operations redirect back to login. Cart page (/cart) exists but appears empty or is not loading properly.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: Cannot test purchase requests due to session management issues and product page messaging feature unclear
    - Actual: Message Seller button is visible on product pages, but clicking it when not logged in (or with unstable session) cannot be tested due to authentication issues. FT-9 prerequisites blocked.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Clicking category from categories page does not filter browse results to that category
    - Actual: Navigated to /categories, saw Furniture category showing 3 items. Clicked on Furniture category button, which navigated to /?category=furniture but the browse results still showed all 10 items, not filtered to the 3 Furniture items. No category filter was applied in the UI.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Blocked by FT-9 failure - cannot test purchase request confirmation
    - Actual: Depends on FT-9 working. Cannot access purchase request features.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Cannot test duplicate email rejection due to broken registration system
    - Actual: Registration system does not create accounts. Attempted registration completes without error but account cannot be verified to exist or to reject duplicates. Cannot proceed with testing duplicate email rejection.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Cannot test wrong password rejection - demo accounts accept any password, cannot create new account for testing
    - Actual: Demo accounts (emma@example.com, marcus@example.com, sofia@example.com) accept any password by design. Custom account registration is broken. Cannot test that incorrect passwords are properly rejected for legitimate accounts.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Cannot test seller action visibility due to inability to create own listings or access seller dashboard
    - Actual: Blocked by FT-1 and authentication issues. Cannot access own listings to verify seller management buttons are present, nor can verify they're absent from other users' listings due to session instability.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Cannot test form validation for listing creation due to missing/broken listing form
    - Actual: No accessible listing creation form found. /create-listing endpoint returns empty page. Attempted registration form does not create accounts. Cannot verify required field validation.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Cannot test price validation due to missing listing creation form
    - Actual: Blocked by CS-14 failure. No accessible listing form to test price validation logic.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: Cannot test duplicate email feedback due to broken registration system
    - Actual: Blocked by registration system failure. Cannot register accounts to test duplicate email feedback or form persistence after rejection.

- [ ] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.
  - Bug Report:
    - Issue: Search results do not update as buyer types or clears query
    - Actual: Entered search terms in the search box, but no immediate filtering occurred. The browse count and product list remained static. No separate search submission button exists, so real-time updates should happen, but they don't.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [ ] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.
  - Bug Report:
    - Issue: Cannot test cart updates due to broken session management and empty cart page
    - Actual: Cart page exists but returns no content. Session management prevents testing cart item additions and updates. Cannot verify real-time cart count and total updates.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.