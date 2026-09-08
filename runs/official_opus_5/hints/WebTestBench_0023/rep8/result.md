# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: My Listings offers only View, Mark Sold/Mark Available and Delete — no Edit control. The owner's product page shows only "Manage Your Listings" (ids: product.manage-listings) with no edit action, and no edit route exists (/edit-listing/prod-1, /listing/prod-1/edit, /edit/prod-1, /my-listings/prod-1/edit all render the 404 page). Title, description, photos, price, category, condition, location and upcycled status of an existing listing cannot be changed.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control exists
    - Actual: The price-range slider renders only ONE thumb (aria-label="Minimum", min=0 max=500); the upper bound is fixed at $500 and cannot be changed (label always reads "$X - $500"). Category, condition, min-price, distance, upcycled-only and all three sorts work correctly and combine (e.g. Furniture+New+Upcycled+min $210 → only the $320 Reclaimed Wood Bookshelf), but a maximum price cannot be specified as required.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not filter browse results
    - Actual: Categories page correctly shows per-category available counts (Furniture 3). Clicking Furniture navigates to /?category=furniture, but the browse list shows all 8 available listings from every category (Sports, Electronics, Toys, Home Decor included) and the count reads "8 items available"; the Furniture filter checkbox remains unchecked (value=false).

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller has no view of received purchase requests; request confirmation is unverifiable/unbacked
    - Actual: Emma sent a purchase-request message ("QA-REQUEST-42…") on Marcus's prod-2 and also used cart "Send Purchase Request" (toast "Purchase requests sent to sellers!", cart emptied). Signing in as Marcus, no request is visible anywhere: My Listings shows only listings (View/Mark Sold/Delete), the owner product page shows only "Manage Your Listings", and the "My Profile" menu item leads to a 404 page. No requested-item or buyer-message view exists.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified; any password authenticates a registered account
    - Actual: Signing in as emma@example.com with password "definitely-wrong-password-xyz" succeeded: redirected to home page with signed-in header (Sell link shown). Login page itself states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank screen instead of a sign-in prompt or redirect
    - Actual: Good: Add to Cart while signed out redirects to /login, and Message Seller's send button is disabled reading "Log in to message". But visiting /cart, /create-listing or /my-listings while unauthenticated leaves the URL unchanged and renders an entirely empty page (body innerText length 0, only the toast containers in the DOM) — no redirect and no sign-in prompt, even after waiting.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.