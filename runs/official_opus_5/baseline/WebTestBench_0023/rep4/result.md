# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Seller cannot attach their own product photos
    - Actual: The Photos control is a button labelled "Add Photo" with helper text "Click to add sample images (up to 4)"; it inserts a fixed Unsplash stock image (photo-1555041469-a586c61ea9bc) and the page contains no input[type=file] (0 file inputs), so no user-supplied photo can be attached. The rest of FT-1 works: listing "Restored Oak Dining Chair" ($80, Furniture, Good, Brooklyn NY, upcycled, description) was created ("Listing created successfully!") and its detail page retained/displayed all submitted values plus Listed 2026-08-27.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: On /my-listings each owned listing exposes only View, Mark Sold and Delete (full button list: New Listing, View/Mark Sold/Delete per item). The product detail page for an owned listing offers only "Manage Your Listings". There is no way to change title, description, photos, price, category, condition, location or upcycled status after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter; price range slider exposes only a minimum thumb
    - Actual: The "Price Range" control renders a single Radix thumb with aria-label="Minimum" (aria-valuemin=0, aria-valuemax=500); the label "$0 - $500" only ever changes its lower bound (e.g. "$220 - $500"), and there is no second thumb or input to cap the maximum price, so buyers cannot filter by maximum price. All other criteria worked and combined correctly (Furniture+New+min $220 → only the $320 bookshelf; upcycled-only → 4 upcycled items; Max Distance 1 mi → 0 items; sorts by newest, price low/high and distance all ordered correctly).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category chosen on the /categories page does not filter browse results
    - Actual: /categories listed "Furniture 3 items"; clicking it navigated to /?category=furniture but the browse view still showed all 9 available listings ("9 items available", including clothing/books/electronics) and no category checkbox was checked, even after waiting. (By contrast the home page's own "Shop by Category" tile for Clothing correctly filtered to 1 item, prod-2.)

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no view of received purchase requests/messages; cart request reports success with nothing recorded anywhere
    - Actual: Emma (buyer) sent a request on Marcus's prod-2 with message "PURCHASE REQUEST TEST 4242: ..." ("Message sent to seller!"). Signed in as Marcus, nothing shows the request: /my-listings lists only his items with View/Mark Sold/Delete (no request/message text at all), his own product page shows only "Manage Your Listings", the user menu offers only My Profile / My Listings / Log out, and "My Profile" (/profile) renders a 404 "Oops! Page not found". Likewise the cart's "Send Purchase Request" immediately reports "Purchase requests sent to sellers!" and empties the cart, but no per-item request is retrievable by any seller.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never verified; any password authenticates a registered account
    - Actual: Signing in as emma@example.com with the arbitrary wrong password "definitelyWrongPassword" succeeded: toast "Welcome back!", redirected to home, header showed signed-in state (Sell/avatar). The login page itself states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected routes render a blank page instead of a sign-in prompt or redirect
    - Actual: Signed out, /create-listing and /cart both render an empty page (document.body innerText is "", only the toast container in #root) — no header, no message, no redirect. Access is blocked but with no clear sign-in prompt. (Other paths are handled well: Add to Cart from a product page redirects to /login with toast "Please log in to add items to your cart", and the Message Seller dialog shows a disabled "Log in to message" button.)


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.