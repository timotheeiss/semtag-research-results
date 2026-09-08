# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: No way to attach the seller's own photos
    - Actual: The create-listing form has no file input (document.querySelectorAll('input[type=file]').length === 0). The only photo control is "Add Photo" labelled "Click to add sample images (up to 4)", which appends fixed Unsplash stock URLs. All other submitted data (title "QA Upcycled Crate Shelf", description, price $142.5, Furniture, Good, Denver CO, Upcycled) was retained and displayed correctly on /product/prod-1787898817199.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No listing-edit capability exists
    - Actual: /my-listings offers only View, Mark Sold and Delete for an owned listing (no Edit); the owner's product page offers only "Manage Your Listings". App router (src/App.tsx) defines only Index, ProductDetail, Login, Register, CreateListing, MyListings, Cart, About, Categories, NotFound — there is no edit-listing route, so title/description/photos/price/category/condition/location/upcycled cannot be changed after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control
    - Actual: The "Price Range" control has only a single slider thumb (aria-label="Minimum", min 0 / max 500); the upper bound is hard-fixed at $500 and cannot be changed, so buyers cannot set a maximum price. Category, condition, distance, upcycled-only and all three sorts (Newest First, Price Low→High/High→Low, Distance) worked correctly and combined correctly (min $100 + Furniture + Like New + Upcycled → exactly Mid-Century Modern Armchair $285).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from /categories does not filter browse results
    - Actual: The /categories page correctly lists "Furniture — 3 items". Clicking it navigates to /?category=furniture, but the browse grid shows all 10 available listings (Clothing, Electronics, Books, Sports, Toys included) and the header reads "10 items available" instead of 3; the Furniture filter checkbox remains unchecked. The ?category query param is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no view of received purchase requests
    - Actual: Emma sent a purchase-request message ("FT22-CHECK: Emma requesting the Harry Potter set…") on Marcus Chen's prod-6 and got "Message sent to seller!". Signed in as Marcus, neither /my-listings (only View/Mark Sold/Delete per listing) nor the prod-6 product page (only "Manage Your Listings") shows any request, buyer name or message; the "My Profile" menu item leads to /profile which renders the 404 page. Separately, the cart's "Send Purchase Request" immediately reported "Purchase requests sent to sellers!" and emptied the cart, but no per-item request can be observed anywhere.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified at login
    - Actual: Registered tina.tester@example.com with password "secret123", logged out, then signed in with "totallyWrongPassword" — login succeeded, redirected to / and header shows the signed-in state (Sell button + user menu). Login page even states "(any password works)".

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