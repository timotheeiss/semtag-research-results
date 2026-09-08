# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own photos — no file upload exists; "Add Photo" inserts random canned stock images
    - Actual: The create-listing form has no input[type=file] (0 found). The Photos control is labelled "Click to add sample images (up to 4)" and each "Add Photo" click appends a fixed Unsplash URL (photo-1555041469..., photo-1506439773...) unrelated to the seller's item. All other data was retained correctly: created listing prod-1787864057461 shows title "Upcycled Crate Nightstand", $80, Furniture, Good, Brooklyn NY, Upcycled badge, description, seller Tess Buyer, Listed 2026-08-27.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: My Listings offers only View / Mark Sold / Delete for the owned listing (accessibility snapshot of /my-listings), and the owner's product detail page offers only "Manage Your Listings". There is no Edit control or edit form anywhere, so title, description, photos, price, category, condition, location and upcycled status cannot be changed after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Maximum-price filter is not usable — the "price range" slider renders only one (minimum) thumb; the maximum is fixed at $500
    - Actual: [data-semtag-id='browse.filters.price'] contains a single role=slider thumb (aria-valuemin 0, aria-valuemax 500); moving it only raises the minimum (label "$10 - $500", "$250 - $500") and the upper bound can never be changed, so buyers cannot set a maximum price. Other criteria worked and combined correctly: Furniture + condition New + min $250 → 1 item (prod-10 $320); distance 11 mi → 8 items (both 15-mi San Francisco items excluded); distance 1 mi → 0 items; upcycled-only → 5 items, all upcycled; sort price-low gave $45,$125,$175,$285,$320 and sort distance gave Brooklyn items before Portland items.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category chosen on the Categories page is not applied to browse results
    - Actual: Categories page listed Home Decor "2 items"; clicking it navigated to /?category=home-decor but the browse grid showed all 10 listings (prod-1..prod-10 across every category) with count "10 items available", and every category checkbox in the filter sidebar remained unchecked — the category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests or buyer messages; the cart "requests sent" confirmation is unverifiable
    - Actual: Bob Buyer sent a message on prod-1 ("QA-TEST-MESSAGE: ...") and a cart purchase request for prod-1 (Emma Green) + prod-9 (Sofia Martinez), each confirmed by toast. Signing in as Emma Green: My Listings shows only her two listings with View/Mark Sold/Delete and no request/message area; her own product page shows only "Manage Your Listings" and no buyer message (page text does not contain the message); "My Profile" leads to a 404 page, and /requests, /messages, /inbox are all 404. The cart reported "Purchase requests sent to sellers!" although no request is retrievable anywhere.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified — any password authenticates a registered account
    - Actual: Signing in as emma@example.com with the password "definitely-not-her-password" succeeded: toast "Welcome back!", redirect to /, header user menu present (authenticated session). The login page itself states "Demo accounts: ... (any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Gated pages render a blank screen instead of a sign-in prompt or redirect; header cart button is a silent no-op when signed out
    - Actual: Signed out: /create-listing and /cart render an empty page (body text length 0, no prompt, no redirect, URL unchanged after 1.5s); clicking the header cart icon on / did nothing (stayed on /, no prompt). Correct behaviour only for the product page: "Add to Cart" redirected to /login and the message dialog showed a disabled "Log in to message" button.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.