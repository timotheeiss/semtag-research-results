# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos
    - Actual: The create-listing form's only photo control is "Add Photo" labelled "Click to add sample images (up to 4)"; it appends a hard-coded Unsplash stock image (photo-1555041469-a586c61ea9bc) and there is no file input in the page (document.querySelectorAll('input[type=file]').length === 0), so no user-supplied photo can be attached. All other submitted data was retained: created listing shows title "QA Upcycled Crate Shelf", description, $120, Furniture, Good, Brooklyn NY, Upcycled badge, seller QA Tester, Listed 2026-08-27.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: My Listings offers only View, Mark Sold and Delete for the owned listing (full button list: New Listing, View, Mark Sold, Delete); the owned product detail page offers only "Manage Your Listings". There is no edit form or control anywhere to change title, description, photos, price, category, condition, location or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control; price slider exposes only a minimum thumb
    - Actual: The price filter renders a single Radix thumb with aria-label="Minimum" (aria-valuemin 0, valuemax 500); no second/maximum thumb exists in the DOM, so the upper bound is permanently $500 and cannot be lowered even though the label claims a "$0 - $500" range. Other criteria worked and combined correctly: Furniture + condition New + min $200 → only Reclaimed Wood Bookshelf $320 (count 1); distance 10 mi excluded the 15 mi San Francisco items, 1 mi left only the 0-mi local listing; upcycled-only → 6 items all badged Upcycled; sorts price-low ($45→$320), price-high, distance (0 mi → Brooklyn → Portland) and newest all ordered correctly.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the categories page does not filter browse results
    - Actual: Categories page listed "Home Decor 2 items"; clicking it navigated to /?category=home-decor but the browse section showed all 11 listings (count "11 items available") including Furniture, Clothing, Electronics, Books, Sports and Toys items. The Home Decor filter toggle remained value:false, i.e. the category query parameter is ignored.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-side view of purchase requests / buyer messages
    - Actual: As buyer Sofia I sent a message request on Emma's prod-1 ("QA-REQUEST-ARMCHAIR...") and a cart purchase request for Emma's prod-5 (toast "Purchase requests sent to sellers!"). Signing in as Emma, My Listings shows only her two listings with View/Mark Sold/Delete and no requests section; her own product detail page shows no requests either; the "My Profile" menu entry navigates to /profile which renders a 404 "Oops! Page not found". The requested item and buyer message are nowhere visible to the seller, so the "requests sent" report cannot be confirmed as recorded.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified during authentication
    - Actual: Account qa.tester@example.com was registered with password "Test1234". Signing in with "WrongPass999" succeeded: redirected to home page and header switched to signed-in state (Sell / cart / user menu). Login page even advertises "Demo accounts ... (any password works)". Any non-empty password is accepted for any existing email.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Guarded pages render a blank screen instead of a sign-in prompt or redirect
    - Actual: Positive cases work in-app: unauthenticated Add to Cart redirected to /login and Message Seller showed a disabled "Log in to message" button. However, visiting /create-listing, /cart or /my-listings while signed out leaves the URL unchanged and renders an empty page (document.body.innerText === "", #root contains only the toaster and an empty section) — no header, no redirect and no sign-in prompt, so the block gives the user no clear feedback or way forward.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.