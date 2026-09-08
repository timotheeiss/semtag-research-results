# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos; only canned sample images can be added
    - Actual: The Create a Listing form has no file input (document.querySelectorAll('input[type=file]').length === 0). The "Add Photo" control is labelled "Click to add sample images (up to 4)" and inserts a fixed Unsplash stock URL (photo-1555041469-a586c61ea9bc) unrelated to the seller's item. All other submitted data was retained correctly on /product/prod-1787863509767 (title QA Test Lamp, description, $42.5, Home Decor, Good, Denver CO, Upcycled badge, seller Tester QA).

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: On /my-listings the only per-listing controls are View, Mark Sold and Delete — no Edit button anywhere (button list: Sell, New Listing, View, Mark Sold, Delete). The owner's product page only offers "Manage Your Listings". Route /edit-listing/prod-1787863509767 returns the 404 page. Title, description, photos, price, category, condition, location and upcycled status cannot be changed after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter exists
    - Actual: The "Price Range" control is a single-thumb Radix slider with aria-label="Minimum" (0–500); only the lower bound can be set (setting it to 250 kept $320 and dropped $175). There is no second thumb or max-price input, so buyers cannot cap price. Category + condition + upcycled-only + distance filters and Newest/Price/Distance sorts all worked correctly and combined (e.g. upcycled+Furniture+New → exactly the 2 matching items; ≤4 mi → prod-1/5/7 in ascending distance order).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection does not filter browse results
    - Actual: Clicking "Furniture" (listed as "3 items") on /categories navigated to /?category=furniture, but the browse page showed all 10 listings with count "10 items available" and no Furniture filter checkbox checked (aria-checked all false). The category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view received purchase requests; cart request confirmation is not backed by any recorded request
    - Actual: Marcus sent a request on Emma's prod-1 with text "QA-REQUEST-123..." (toast "Message sent to seller!") and separately sent a cart request for 2 items (toast "Purchase requests sent to sellers!"). Signing in as Emma, neither /my-listings nor the owned product page /product/prod-1 shows any request, buyer name or message (page text contains no "QA-REQUEST-123"); the account menu only has My Profile (404), My Listings and Log out. No inbox/requests UI exists, so the "requests sent" confirmation cannot be verified as recorded.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never validated; any password authenticates a registered account
    - Actual: Signed in as emma@example.com using the incorrect password "wrongpassword999": redirected to / and header switched to the authenticated state (Sell + account menu). Login page itself states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.