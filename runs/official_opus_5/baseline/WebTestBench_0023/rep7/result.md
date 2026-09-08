# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: My Listings offers only View / Mark Sold / Delete for the owner's listing; the owner's product page offers only "Manage Your Listings". No edit form or route exists (/edit-listing/:id returns the 404 page), so title, description, photos, price, category, condition, location and upcycled status cannot be changed after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category chosen on /categories page is not applied to browse results
    - Actual: Clicking "Furniture" (3 items) on /categories navigated to /?category=furniture, but the browse page showed all 10 listings, count "10 items available", and no category checkbox was checked (even after waiting). The home page "Shop by Category" tiles do work (Clothing → 1 item, count "1 items available").

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view received purchase requests
    - Actual: Marcus sent a product-page message ("QA-REQUEST-42...") and a cart-wide "Send Purchase Request" for Emma's items (toasts: "Message sent to seller!" / "Purchase requests sent to sellers!" with the cart emptied immediately). Signing in as Emma, no requested item or buyer message is visible anywhere: My Listings shows only listing cards with View/Mark Sold/Delete, her own product pages show only "Manage Your Listings", the user-menu "My Profile" link renders the 404 page, and /requests is also 404. The cart request is reported as sent with no verifiable per-item record.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not validated; any password authenticates a registered account
    - Actual: Signed in as emma@example.com using the arbitrary password "totally-wrong-xyz": redirected to / and header changed to show "Sell" (authenticated). Login page even states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected seller routes render a blank page instead of a sign-in prompt or redirect
    - Actual: Good: unauthenticated "Add to Cart" redirects to /login with toast "Please log in to add items to your cart"; "Message Seller" dialog shows "Log in to message"; cart trigger is hidden. But visiting /create-listing or /my-listings while signed out renders a completely empty page (document.body.innerText === "") with no sign-in prompt and no redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.