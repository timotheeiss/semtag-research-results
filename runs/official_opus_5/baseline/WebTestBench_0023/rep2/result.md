# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos; only canned sample images can be added
    - Actual: The Create Listing form has no file input (document.querySelectorAll('input[type=file]').length === 0). The "Add Photo" button is labelled "Click to add sample images (up to 4)" and inserts fixed Unsplash URLs (photo-1555041469..., photo-1506439773649...). All other submitted data (title, description, $85, Furniture, Good, Seattle WA, upcycled) was saved and displayed correctly on /product/prod-1787836903965.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: On /my-listings the owned listing exposes only "View", "Mark Sold" and "Delete" buttons (full button list enumerated from DOM); the product page for an owned listing offers only "Manage Your Listings". There is no way to change title, description, photos, price, category, condition, location or upcycled status after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No usable maximum-price filter
    - Actual: The Price Range control renders only one thumb (role=slider, aria-label "Minimum", 0–500). Dragging it always changes the minimum ($0→$150→$420→$470 in repeated attempts); the upper bound stays pinned at $500, so buyers cannot cap results by maximum price. Category (Furniture→4), condition (Furniture+New→2), upcycled-only (→6), min price ($150 → only ≥$150), distance (3 miles → 3 items) and sorting (Newest/Price low-high/Distance) all worked and combined correctly.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category chosen on /categories does not filter browse results
    - Actual: On a fresh load of /categories (which lists e.g. "Furniture 3 items"), clicking Furniture navigates to /?category=furniture but the browse page shows all 10 available listings ("10 items available") with no category checkbox checked. The category URL parameter is ignored. (Clicking a category tile on the home page's own "Shop by Category" section does filter correctly, e.g. Home Decor → 2 items.)

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view received purchase requests/messages
    - Actual: Buyer "Buy Tester" sent a message on Sofia's listing prod-9 (toast "Message sent to seller!") and earlier a cart-wide request reported "Purchase requests sent to sellers!". Signed in as sofia@example.com, /my-listings shows only her 3 listings with View/Mark Sold/Delete and no requests or messages section; the only other account page, "My Profile" (/profile), renders a 404 "Oops! Page not found". No UI anywhere surfaces the requested item or buyer message, and the cart confirmation is shown unconditionally.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified at login; any password authenticates a registered account
    - Actual: Registered test.seller1@example.com with password "seller123", logged out, then signed in with "wrongpass999" — login succeeded (redirected to home, header shows Sell/user menu instead of Log in/Sign up). Login page even states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected routes render a blank page for signed-out users instead of redirecting or prompting sign-in
    - Actual: Signed out, /create-listing, /cart and /my-listings all render an empty document (document.body.innerText === "", no header, no toast, no redirect — URL stays put). In-app actions do work correctly: Add to Cart redirects to /login with toast "Please log in to add items to your cart", and Message Seller shows a "Log in to message" dialog.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.