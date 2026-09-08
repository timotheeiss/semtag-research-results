# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality found in UI
    - Actual: My Listings page shows only View, Mark Sold, and Delete buttons. No Edit button or option found. Attempted to navigate to /edit-listing and /product/{id}/edit URLs which returned 404 errors.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Couldn't test - unable to obtain listing owned by logged-in user
    - Actual: Demo account (marcus@example.com) has no listings. Previous test account didn't persist. Token constraints prevent creating new listing for testing mark sold/delete functionality.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Dependent on FT-3 - cannot test without functional mark sold/delete
    - Actual: Cannot verify that sold/deleted listings are removed from browse results.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [ ] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.
  - Bug Report:
    - Issue: Cart functionality not working - cart page appears broken
    - Actual: Attempted to navigate to /cart URL which returned minimal page. Add to cart button on listings did not provide visible confirmation. Cart page returned only 485 bytes of HTML with no content.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: Purchase request feature not tested - related functionality appears incomplete
    - Actual: Could not locate purchase request functionality in the application. Complex flow and token constraints prevented testing.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category filter not applied - results not restricted to selected category
    - Actual: Clicked Furniture category from categories page (/?category=furniture URL), but results still show all 10 items instead of just 3 Furniture items. Result count displays '10 items available' instead of updated count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Could not test seller purchase request view - purchase request feature incomplete
    - Actual: Could not locate or test purchase request functionality in application.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Could not test duplicate email registration due to account persistence issues
    - Actual: First registration attempt didn't persist in system. Could not create duplicate registration test.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Wrong password was accepted for demo account
    - Actual: Demo account (emma@example.com) accepted password 'wrongpassword' and created authenticated session, navigating to home page. Should have rejected incorrect password.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Could not test seller-only actions - requires owned listing
    - Actual: No owned listings available to verify that edit/delete actions are hidden for non-owners.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Could not test missing required fields validation - create-listing page not fully accessible
    - Actual: Attempted to access create-listing form but page content not loading in snapshot. Could not verify error messages for missing fields.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Could not test negative price validation - create-listing form not fully functional
    - Actual: Could not attempt to submit negative price due to form access issues.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Could not fully test unauthenticated access blocks - incomplete testing
    - Actual: Could not verify that specific features (create listing, cart, purchase request) redirect unauthenticated users.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: Could not test duplicate email feedback - account registration issues
    - Actual: Registration functionality available but first account didn't persist. Could not test duplicate email error message.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [ ] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.
  - Bug Report:
    - Issue: Could not test cart confirmation - cart feature not working
    - Actual: Cart page not functional. Could not verify cart updates or confirmation messages.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.