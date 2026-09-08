# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit-listing functionality exists
    - Actual: The My Listings page only offers View, Mark Sold, and Delete actions per listing (confirmed via semantic_snapshot: no edit/update action). The product detail page for the owner only shows a 'Manage Your Listings' link, no edit control. Directly navigating to a guessed edit route (/edit-listing/{id}) returns a 404 'Page not found'. Sellers have no way to edit title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not apply the category filter to browse results
    - Actual: Clicking 'Furniture' (3 items) on the Categories page navigated to '/?category=furniture' but the browse page showed all 10 items ('10 items available'), not just the 3 Furniture items, and the Furniture checkbox in the filters sidebar remained unchecked (false). The category query param is not being read/applied by the browse page's filter state.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing UI exists to view purchase requests or buyer messages
    - Actual: Bob (buyer) successfully added Alice's 'QA Final Test Bicycle' to cart and sent a purchase request via the cart's 'Send Purchase Request' button, receiving toast 'Purchase requests sent to sellers!' (confirming the second half of this item works: cart request reported sent). However, when Alice (seller/owner) logged back in, there is no way to view the request or any buyer message: the header has no notifications/messages icon, 'My Profile' route returns 404, 'My Listings' page shows no request indicator, and the owner's own product detail page shows no requests/messages section at all — just title/description/seller info and a 'Manage Your Listings' link.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [X] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Inconsistent/missing sign-in enforcement for direct navigation to protected routes
    - Actual: Action-triggered attempts are handled well while logged out: clicking 'Add to Cart' redirects to /login with toast 'Please log in to add items to your cart', and 'Message Seller' shows a dialog with a disabled 'Log in to message' button. However, directly navigating (URL) to protected routes /create-listing, /cart, and /my-listings while unauthenticated renders a completely blank page (no header content, no form, no message, no redirect to /login) — confirmed via DOM inspection showing empty body innerText and empty semantic_snapshot for each of the three routes. This is not a clear sign-in prompt or redirect as required.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.