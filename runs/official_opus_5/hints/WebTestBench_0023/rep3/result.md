# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos; only preset sample images can be added
    - Actual: The Photos section only offers an 'Add Photo' button labelled 'Click to add sample images (up to 4)'. There is no file input anywhere on /create-listing (document.querySelectorAll('input[type=file]').length === 0), so the photo attached to the created listing is a hard-coded Unsplash URL (photo-1555041469-a586c61ea9bc). All other submitted data (title 'QA Upcycled Crate Shelf', description, $120, Furniture, Good, Austin TX, Upcycled) was retained and displayed on the created listing.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No listing-edit capability exists
    - Actual: For an owned listing, /my-listings offers only View, Mark Sold and Delete, and the owner's product detail page offers only a 'Manage Your Listings' link. No Edit control, edit form or edit route is exposed anywhere, so title, description, photos, price, category, condition, location and upcycled status of an existing listing cannot be changed.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter: the price range slider exposes only a 'Minimum' thumb
    - Actual: The Price Range control renders a single Radix thumb with aria-label='Minimum' (aria-valuemin=0, aria-valuemax=500); the displayed range '$0 - $500' has no second/max thumb, so a buyer cannot cap the maximum price. Other filters combine correctly (Furniture + New + Upcycled-only → exactly prod-10 $320 and prod-3 $175; min price $100 + distance ≤12mi → 5 items all ≥$100 and ≤12mi) and sorts by price low→high ($35…$320) and distance (2.5→12mi) are correct.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the Categories page does not filter browse results
    - Actual: Clicking 'Home Decor' (card states '2 items') navigated to /?category=home-decor, but the browse grid still showed all 10 listings, the count read '10 items available', and every category filter toggle remained aria-checked=false — the category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests or buyer messages
    - Actual: Buyer marcus@example.com sent a message 'QA-TEST-MSG-42…' on Emma's prod-1 and then a cart purchase request for prod-1 + prod-5 (toast 'Purchase requests sent to sellers!'). Signing in as the seller emma@example.com, /my-listings shows only listing rows (title, price, status, View/Mark Sold/Delete) with no requests or messages, the owner's product detail page shows no request/message section (page text does not contain 'QA-TEST-MSG-42'), and the only other seller destination, 'My Profile' (/profile), renders the 404 'Page not found' screen. The cart request is therefore reported as sent with no recorded request the seller can view.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never verified; any password authenticates a registered account
    - Actual: Account qa.buyer@example.com was registered with password 'qapass123'. Logging in with 'TotallyWrongPassword999' succeeded: toast 'Welcome back!', redirect to home, header shows the signed-in user menu. The login page itself advertises 'Demo accounts: ... (any password works)'.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank page instead of a sign-in prompt or redirect
    - Actual: While signed out, /cart, /my-listings and /create-listing all render an empty document (document.body.innerText === "") with the URL unchanged — no sign-in prompt, no redirect, no message. In-app actions are handled better: Add to Cart redirects to /login and Message Seller opens a dialog showing 'Log in to message'.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.