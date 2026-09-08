# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: No way to attach the seller's own photos; only canned stock images can be added
    - Actual: The create-listing form has no file input at all (document.querySelectorAll('input[type=file]').length === 0). The "Add Photo" control only appends pre-set Unsplash sample images (helper text: "Click to add sample images (up to 4)"), so a seller cannot supply their own product photos. All other parts of the item worked: a listing created with title "QA Upcycled Crate Shelf", description, price 42.50, category Furniture, condition Good, location Brooklyn NY and upcycled=true was saved and its detail page displayed all of that (price rendered "$42.5").

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Listing editing is not implemented
    - Actual: No edit affordance exists anywhere for an owned listing. My Listings offers only View, Mark Sold and Delete (no "Edit" string appears on the page at all), the product detail page for an owned listing offers only "Manage Your Listings", and the direct route /edit-listing/<id> renders the 404 "Oops! Page not found" page. Title, description, photos, price, category, condition, location and upcycled status therefore cannot be changed after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter (price slider has a single min-only thumb); distance is not measured from the buyer's location
    - Actual: The price filter renders only ONE thumb (aria-valuemin=0, aria-valuemax=500); moving it sets the LOWER bound only (label became "$250 - $500"), so a buyer cannot set a maximum price — the upper bound is permanently $500. Distance also ignores the signed-in buyer's location: as Quinn Tester (Austin, TX), setting the distance filter to 3 miles returned only the two Brooklyn, NY items and excluded all three Austin, TX items, and "Sort: Distance" ordered Brooklyn first and Austin second. Category, condition, upcycled-only and price-minimum filters do combine correctly (Furniture+New+Upcycled+min $250 → only the $320 Reclaimed Wood Bookshelf), and price sorting is correct ($35→$320).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category tile on the Categories page does not apply the category filter
    - Actual: Clicking "Home Decor 2 items" on /categories navigated to /?category=home-decor but browse showed all 10 listings and the count read "10 items available"; every category checkbox in the filter panel remained unchecked, i.e. the ?category= query parameter is ignored. (By contrast, the category shortcut on the home page itself works: it produced "2 items available" with exactly the Macrame Wall Hanging and Brass Floor Lamp.)

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no view of purchase requests; request confirmations are not backed by any recorded request
    - Actual: Buyer sofia@example.com sent a message request on Marcus Chen's prod-2 containing "QA-REQUEST-MARKER..." (toast "Message sent to seller!"), and a cart request for two of Marcus's items reported "Purchase requests sent to sellers!". Signing in as marcus@example.com afterwards, no request is visible anywhere: /my-listings shows only the listing cards (no request/message/inquiry text, marker absent), the owned product detail page shows only "Manage Your Listings", and the user-menu "My Profile" link navigates to /profile which renders the 404 "Oops! Page not found" page. The cart flow also emptied the cart and reported success immediately with no per-item request record to verify.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified; any password authenticates a registered account
    - Actual: Signing in as emma@example.com with the arbitrary password "definitely-the-wrong-password-zzz" succeeded: redirected to / and header switched to the signed-in state (Sell link shown). The login page itself states "Demo accounts ... (any password works)", confirming passwords are never checked.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank page instead of a sign-in prompt or redirect
    - Actual: Gated actions on the product page behave correctly (unauthenticated "Add to Cart" redirects to /login; "Message Seller" shows "Log in to message"). But visiting the protected pages while signed out renders a completely empty page with no message and no redirect: /cart, /create-listing and /my-listings all return document.body.innerText.length === 0 and an accessibility snapshot containing only the notification regions. Access is blocked, but there is no clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.