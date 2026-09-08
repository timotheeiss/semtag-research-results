# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No listing edit capability exists
    - Actual: My Listings offers only View, Mark Sold and Delete for an owned listing — no Edit control anywhere (product detail for owner only offers 'Manage Your Listings'). Direct route /edit-listing/&lt;id&gt; returns the 404 page. Sellers cannot change title, description, photos, price, category, condition, location or upcycled status after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter; price slider exposes only a minimum thumb
    - Actual: 'Price Range' slider contains a single thumb (aria-label='Minimum', 0-500); the upper bound is fixed at $500 (label always '$X - $500'), so a buyer cannot set a maximum price. Other filters work and combine correctly (Furniture + Like New + Upcycled-only + min $250 → 1 result, prod-1 $285 Furniture/Like New/Upcycled; distance ≤2 mi → only the 0-mi local listing) and all three sorts order results correctly (price asc 35→320, price desc 320→35, distance groups Brooklyn→Austin→Portland→SF).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection on /categories does not filter browse results
    - Actual: Categories page correctly showed 'Furniture 4 items'. Clicking it navigated to /?category=furniture, but the browse grid still listed all 11 available listings (clothing, electronics, books, sports, toys included), the count read '11 items available' and the Furniture filter checkbox remained unchecked — the category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view of purchase requests exists
    - Actual: Buyer marcus sent a purchase-request message on emma's prod-1 ('QA-REQ-77...') and a cart request was earlier reported as 'Purchase requests sent to sellers!'. Signed in as the seller (emma) there is nowhere to view them: the user menu offers only My Profile, My Listings, Log out; My Listings shows only listing cards/actions and no requests; 'My Profile' (/profile) renders the 404 page, and /requests, /messages, /purchase-requests, /seller/requests, /inbox, /orders all 404. The requested item and buyer message can never be viewed by the seller.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified; any password authenticates a registered account
    - Actual: Account qa.seller@example.com was registered with password 'sellerpass1'. Signing in with 'totallywrong9' succeeded: redirected to home with an authenticated session (header shows avatar alt='QA Seller' and Sell/cart controls). Login page itself states '(any password works)'.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank screen instead of a sign-in prompt or redirect
    - Actual: Good: unauthenticated 'Add to Cart' redirects to /login, and the message dialog's send button is disabled and labelled 'Log in to message'. But visiting /cart, /create-listing or /my-listings while signed out renders a completely empty page (document body has no content beyond the toast containers) — no sign-in prompt, no message and no redirect to /login.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.