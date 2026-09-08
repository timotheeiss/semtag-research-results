# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: No photo upload — sellers cannot attach their own photos
    - Actual: The create-listing photo control has no file input at all (0 input[type=file] in the form); the "Add Photo" button only appends canned Unsplash stock images and the helper text reads "Click to add sample images (up to 4)". All other data was retained correctly: the created listing "Restored Oak Dining Chair" shows $120, Furniture, Good, Brooklyn NY, the submitted description, Upcycled badge, seller Testy Buyer, Listed 2026-08-28, and 2 photos.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Listing edit capability absent
    - Actual: No edit affordance exists anywhere for an owned listing. /my-listings offers only "View", "Mark Sold" and "Delete" per item, and the owner's product detail page exposes only "Manage Your Listings". There is no way to change title, description, photos, price, category, condition, location or upcycled status after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Maximum-price filter control is missing/inoperable
    - Actual: The "Price Range" control renders only ONE Radix thumb (aria-label="Minimum", valuenow 0, min 0, max 500); there is no second/maximum thumb and no other max-price input in the filters panel (panel text: Special/Categories/Condition/Price Range $0-$500/Max Distance). Minimum can be raised (PageUp -> "$100 - $500", 5 items all >= $100) but the maximum is permanently pinned at $500, so buyers cannot apply a maximum-price criterion. Category (Furniture -> 3 correct items), condition (+New -> prod-10, prod-3), upcycled-only (5 upcycled items), distance (3 miles -> only the 2 Brooklyn listings), and sorting by newest/price-low ($45,$125,$175,$285,$320)/distance all worked correctly.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection does not filter browse results
    - Actual: Clicking "Home Decor" (listed as "2 items") on /categories navigates to /?category=home-decor, but the browse grid still shows all 10 listings and the count still reads "10 items available"; the Home Decor filter toggle remains false. A direct fresh load of /?category=home-decor gives the same unfiltered result, so the category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no view of received purchase requests/messages
    - Actual: After Testy Buyer sent a message on prod-1 and a cart purchase request for prod-1+prod-9, signing in as the seller Emma Green shows no requests anywhere: /my-listings only lists her two items with View/Mark Sold/Delete and no request or message section, and the "My Profile" menu entry navigates to a 404 "Oops! Page not found" page. The cart action reported "Purchase requests sent to sellers!" and cleared the cart, but no request is recorded or viewable for any cart item.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password not verified during authentication
    - Actual: Logging in as emma@example.com with an arbitrary wrong password ("totally-wrong-password-xyz") succeeded and redirected to the signed-in home page. Login page itself states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected routes render a blank page instead of a sign-in prompt or redirect
    - Actual: While signed out, /cart, /create-listing and /my-listings all render an entirely empty page (document.body.innerText === "") with no sign-in prompt, message, or redirect to /login. Only the in-page controls behave correctly: Add to Cart redirects to /login and Message Seller shows a disabled "Log in to message" button.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.