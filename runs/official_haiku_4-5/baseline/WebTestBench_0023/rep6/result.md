# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit button found for listings
    - Actual: On my-listings page, only "View", "Mark Sold", and "Delete" buttons are visible. No "Edit" button or option to edit listing title, description, photos, price, category, condition, location, or upcycled status.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot verify sold/deleted listings are excluded from buyer browse
    - Actual: Unable to test because edit listing feature is not available (FT-2 FAIL), preventing creation of a sold listing to verify it no longer appears to buyers. Delete functionality exists but was not fully exercised to verify removal from browse results.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [ ] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.
  - Bug Report:
    - Issue: Item not retained in cart after adding
    - Actual: Clicked add to cart button for "Yoga Mat & Blocks Set" which showed as [active] after click, but cart page shows "0 items" and "Your cart is empty". Item was not persisted to cart.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing filter not applied when navigating from category page
    - Actual: URL shows ?category=books but displays all 10 items instead of filtering to 1 book item (Complete Harry Potter Book Set). Result count shows "10 items available" instead of "1 items available".

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Cannot verify seller can view purchase requests and buyer messages
    - Actual: Unable to test seller's ability to view purchase requests and buyer messages. The feature either requires additional UI navigation not discovered or is not fully implemented. Could not reliably access and test the seller's message/request view interface.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [X] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Cannot test form validation due to create-listing page not loading
    - Actual: Create-listing form fails to load properly when accessing /create-listing while logged in. Page shows incomplete snapshot and no form elements. Unable to test required field validation and error messages for missing title, description, photo, price, category, condition, or location.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Cannot test price validation due to create-listing form not loading
    - Actual: Create-listing form fails to load properly when accessing /create-listing while logged in (see CS-14). Unable to test that negative prices, non-numeric values, and invalid prices are rejected. Cannot verify that invalid prices do not create listings.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [ ] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.
  - Bug Report:
    - Issue: No visible confirmation or cart update when adding item
    - Actual: Clicked add to cart but saw no notification/confirmation message. Cart count in header was not updated. Item does not appear in cart after viewing cart page.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.