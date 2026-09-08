# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for owned listings
    - Actual: My Listings page offers only View, Mark Sold and Delete for the owned listing; the owned product detail page offers only "Manage Your Listings"; the account menu has only My Profile / My Listings / Log out. There is no way to edit title, description, photos, price, category, condition, location or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Maximum-price filter has no usable control (price range slider renders only one "Minimum" thumb)
    - Actual: Category (Furniture→3 items), condition (+New→2 items), upcycled-only (→5 upcycled items), distance (1 mile→0 items) and sorts (Newest, Price Low→High: 45,125,175,285,320; Distance: Brooklyn items first) all work and every result matched the active criteria. However the "Price Range" control renders a single thumb with aria-label "Minimum" (aria-valuemin 0/valuemax 500); there is no second (maximum) handle in the DOM, so a buyer cannot set a maximum price. Clicking the track only moves the minimum (label became "$250 - $500"); the max value only changed ($10 - $120) as an unintended side effect of a drag.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the Categories page does not filter browse results
    - Actual: Categories page showed "Home Decor 2 items"; clicking it went to /?category=home-decor but browse showed all 10 listings ("10 items available") with no category filter applied. (For contrast, the home page's "Shop by Category" tile for Furniture correctly filtered to 3 items and checked the Furniture box.)

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view of received purchase requests
    - Actual: QA Buyer sent a direct message request for prod-1 ("Message sent to seller!") and a cart request for prod-1+prod-2 ("Purchase requests sent to sellers!", cart emptied immediately). Logging in as the seller emma@example.com, My Listings shows only her two listings with View/Mark Sold/Delete and no requests/messages section; the account menu only has My Profile, My Listings, Log out; the header has only search, Sell, cart and avatar. The seller cannot view the requested item or the buyer's message anywhere, and the cart request reported success without any verifiable record.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified; any password authenticates a registered account
    - Actual: Account qa.buyer@example.com was registered with password "BuyerPass1". Signing in with "WrongPass999" succeeded: toast "Welcome back!", redirected to /, header shows Sell and the QA Buyer avatar (authenticated session created). The login page itself states "(any password works)".

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