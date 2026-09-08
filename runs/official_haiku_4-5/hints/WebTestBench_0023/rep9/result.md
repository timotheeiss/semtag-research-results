# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Listing created without required photo
    - Actual: Successfully created listing without attaching any photos. Requirement FT-1 specifies "by attaching one or more of their own product photos" but validation did not enforce this. Constraint CS-14 also specifies photos are required.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality found for owned listings
    - Actual: Viewed own listing on product detail page and my-listings page. No Edit button or edit action was available. Only actions shown were View, Mark Sold, and Delete. Edit functionality required by FT-2 is not exposed in the UI.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [ ] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.
  - Bug Report:
    - Issue: No visible confirmation message for purchase request submission
    - Actual: Clicked \"Send Purchase Request\" button from cart with 2 items. Cart was cleared without showing any confirmation message. Requirement states \"receives visible confirmation that it was submitted\" - no such confirmation was visible. Also unclear if message dialog was required (requirement says non-empty purchase-request message).

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category filter not applied from category page
    - Actual: Clicked Furniture category on categories page. URL changed to ?category=furniture but browse results still show 10 items (all categories) instead of 3 Furniture items. No category filter appears active.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller cannot view purchase requests
    - Actual: Logged in as seller (Marcus Chen) and viewed owned product (prod-6: Complete Harry Potter Book Set). No purchase request information or messages section visible on the product detail page or in my-listings. No way found to view buyer messages or purchase requests from seller perspective.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted
    - Actual: Login with emma@example.com and incorrect password 'wrongpassword' was accepted. User is now authenticated (Sell and Cart buttons visible in header).

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Listing created without required fields - no validation
    - Actual: Created listing with title, description, price, category, condition, and location, but WITHOUT photos. Listing was accepted and saved. Requirement specifies listing cannot be created without \"at least one photo, a title, description, valid price, category, condition, and location\" - photo validation did not work.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No visible feedback for duplicate email during registration
    - Actual: Attempted registration with existing email (emma@example.com). Form submission was rejected (remained on register page), but no error message was displayed to the user. Requirement specifies \"immediate, visible duplicate-account feedback\" - this feedback was not visible.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [ ] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.
  - Bug Report:
    - Issue: Missing content fields in listing display
    - Actual: Viewed product detail page for Kids Wooden Train Set. Displayed: category (Toys), condition (Fair), title, price ($35), description, seller name (Sofia Martinez). NOT displayed: photos, location, listing date, availability status, upcycled status. Requirement specifies listing details should include \"photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status\".