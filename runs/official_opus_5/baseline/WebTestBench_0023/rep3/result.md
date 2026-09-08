# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own photos — only canned sample images
    - Actual: The Photos control is an "Add Photo" button labelled "Click to add sample images (up to 4)"; it inserts fixed Unsplash stock URLs and the page contains no input[type=file], so a seller's own product photos can never be attached. All other submitted data was retained: created listing /product/prod-1787846485484 shows title "QA Test Lamp", the description, $42.5, Home Decor, Good, Portland, OR, Upcycled badge, seller Emma Green, Listed 2026-08-27, with the 2 added images.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: My Listings offers only View / Mark Sold / Delete per listing (no Edit), and the product page for an owned listing shows only "Manage Your Listings". There is no UI anywhere to change a listing's title, description, photos, price, category, condition, location, or upcycled status after creation.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control
    - Actual: The "Price Range" slider renders only a single thumb (aria-label="Minimum", 0–500); the displayed range is always "$X - $500" and the upper bound cannot be changed, so buyers cannot set a maximum price. Other criteria combined correctly: Furniture + condition New + Upcycled-only + min $200 → 1 item (Reclaimed Wood Bookshelf $320); distance ≤6 miles → 5 items; sorts Newest/Price Low-High/High-Low/Distance all reorder correctly.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection does not filter browse results
    - Actual: The /categories page shows correct per-category counts (Furniture 3, Clothing 1, …, Other 0, matching the 9 available listings), but clicking "Furniture" navigates to /?category=furniture where the browse grid still lists all 9 available items across every category and reports "9 items available"; no category filter checkbox is applied. Same behaviour for the home page category tiles.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests or buyer messages
    - Actual: As Marcus I sent a message request on Emma's prod-1 ("Message sent to seller!") and a cart request for prod-1 + Sofia's prod-4 ("Purchase requests sent to sellers!"). Signing in as Emma, no request/message data is exposed anywhere: My Listings shows only listing cards with View/Mark Sold/Delete, the owned product page shows only "Manage Your Listings", the header has no notifications control, and /profile, /requests, /messages, /inbox, /purchase-requests, /dashboard, /notifications, /orders all render 404. The cart request confirmation is therefore shown with no verifiable per-item request record.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified at login
    - Actual: Signing in as emma@example.com with the wrong password "totally-wrong-password" succeeded: toast "Welcome back!", redirected to home, authenticated header (Sell + account menu). The login page even states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated cart page renders blank with no sign-in prompt or redirect
    - Actual: Add to Cart and Message Seller are correctly gated (redirect to /login with toast "Please log in to add items to your cart"; message dialog shows disabled "Log in to message"), and the Sell control is hidden when logged out. But visiting /cart while signed out renders an empty page (document body has no content at all, URL stays /cart) — no sign-in prompt, message, or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.