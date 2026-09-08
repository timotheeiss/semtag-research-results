# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own photos
    - Actual: The Photos control is a button labelled "Add Photo" / "Click to add sample images (up to 4)" that inserts a canned stock image; the page contains no input[type=file] (document.querySelectorAll('input[type=file]').length === 0), so a seller cannot upload their own product photo. All other submitted data was retained: new listing "Upcycled Crate Side Table" displays Upcycled badge, Furniture, Good, $60, Brooklyn NY, Listed 2026-08-27, description and seller Emma Green.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: /my-listings offers only View, Mark Sold and Delete per listing (buttons on page: New Listing, View, Mark Sold, Delete); the owner's product-detail page shows only a "Manage Your Listings" link. There is no way to edit title, description, photos, price, category, condition, location or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control
    - Actual: The "Price Range" filter renders a single slider thumb (aria-label "Minimum", range 0-500). Only the minimum price can be changed ($0-$500 → $100-$500); there is no control to lower the maximum price, so a buyer cannot combine a max-price criterion. Category, condition, distance, upcycled filters and newest/price/distance sorts each worked (e.g. Furniture+New+Upcycled → 2 matching items; price-low sort ordered $35→$320).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection does not filter browse results
    - Actual: Clicking "Home Decor" (labelled "2 items") on /categories navigated to /?category=home-decor, but browse showed all 10 listings and the count read "10 items available"; the Home Decor filter toggle remained false (semantic_observe value=false).

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests / buyer messages
    - Actual: As buyer Quinn Tester I sent a direct request on prod-2 ("QA-REQUEST-777...", toast "Message sent to seller!") and a cart request for prod-2+prod-9 (toast "Purchase requests sent to sellers!"). Signing in as the seller marcus@example.com, /my-listings shows only the listings with View/Mark Sold/Delete and no requests/messages section; prod-2's detail page as owner contains no request or buyer message (body text does not contain "QA-REQUEST-777"); the only other account page, /profile, renders the 404 "Return to Home" page. The cart flow reports requests as sent with no recorded, viewable request for any cart item.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified during authentication
    - Actual: Signing in as emma@example.com with the arbitrary password "definitely-wrong-password-xyz" succeeded: redirected to / and header changed to signed-in state (Sell menu). Login page even states "any password works".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Gated areas blocked without any sign-in prompt or redirect; message dialog silently non-functional
    - Actual: Signed out: /create-listing and /cart render a completely blank page (document.body.innerText === "", no header, no prompt, no redirect). On a product page the "Message Seller" dialog still opens for an anonymous visitor and clicking Send does nothing at all — dialog stays open, no confirmation, no error, no redirect to login. Only "Add to Cart" behaved correctly by redirecting to /login.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.