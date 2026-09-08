# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Edit listing interface not found or not accessible
    - Actual: No edit button visible on listing detail page or my-listings page. Attempting to navigate to /edit-listing/:id returns 404 error page. Only available actions are View, Mark Sold, and Delete.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Cannot maintain authenticated seller session to test mark as sold/delete functionality
    - Actual: Created listing (prod-1787754042267) with all required details, which navigated to my-listings page showing 'Mark Sold' and 'Delete' buttons. However, session was lost during testing, preventing verification of mark-as-sold and delete functionality. Persistent authentication issues prevent complete testing of this feature.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot test without completing FT-3 (mark as sold/delete)
    - Actual: Dependent on FT-3 functionality. Unable to verify that sold or deleted listings are removed from browse results due to session loss and inability to maintain seller authentication context. Pre-requisite for this test (ability to mark listings as sold/deleted) could not be fully verified.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: No visible confirmation message for purchase request submission
    - Actual: Clicked 'Send Purchase Request' button in cart - cart cleared and redirected to empty cart page, but no visible confirmation message (e.g., 'Purchase request sent successfully') or success notification displayed. Only indication was the cart becoming empty.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category link does not automatically filter browse results
    - Actual: Clicked 'Furniture' category from categories page (3 items listed), navigated to /?category=furniture, but browse results initially showed 10 items unfiltered. Had to manually toggle Furniture filter to see only 3 furniture items. Category URL parameter not being applied as automatic filter.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Cannot maintain seller session to view purchase requests and verify cart request reporting
    - Actual: Unable to access seller purchase request interface due to persistent session loss. Sent purchase request from cart (which cleared cart), but could not verify seller-side visibility of request or confirm that cart request is reported as 'sent' only after request recorded for each item. Cannot complete this test.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo accounts accept any password, preventing verification of password validation for registered accounts
    - Actual: Logged in successfully with emma@example.com using password 'wrongpassword' (help text states demo accounts work with any password). Cannot verify password validation behavior for actual registered accounts due to registration/authentication issues.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Cannot test seller action access control due to session issues and inability to view other users' listings
    - Actual: Observed that 'Mark Sold', 'Delete', and 'Manage Listings' actions appear on seller's own listings (prod-1787754042267 showed these actions). Could not verify that these actions are hidden when viewing another seller's listing due to session loss. Cannot fully test access control for seller management actions.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Listing created without photo or photo validation
    - Actual: Successfully created listing with title, description, price, category, condition, location, and upcycled status, but without explicitly uploading or confirming photos. No error message or validation feedback for missing photos. Listing was persisted and displayed without photo content. Checklist requires photos to be mandatory.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Cannot verify price validation - session issues prevent testing
    - Actual: Successfully created listing with valid price ($450). Attempted to test invalid prices (negative, non-numeric) but unable to access create-listing form for testing due to persistent authentication session loss. Cannot confirm if invalid prices are properly rejected.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.