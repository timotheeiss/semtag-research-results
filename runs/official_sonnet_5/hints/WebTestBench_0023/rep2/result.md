# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit/update functionality for existing listings
    - Actual: After creating a listing, checked both the 'My Listings' page (which only offers View, Mark Sold, and Delete actions per item — no Edit) and the listing's own detail page as its owner (which only shows a 'Manage Your Listings' link — no Edit button). A DOM-wide search for any button/link containing 'edit' text or aria-label on both pages returned no matches. There is no discoverable way for a seller to edit the title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Maximum price cannot be independently filtered
    - Actual: Category, condition, upcycled-only, and distance filters all combine correctly with sort (newest/price/distance) and every result satisfied the active criteria in each test (e.g., Furniture+New+min$250 → exactly prod-10 $320; Upcycled-only → exactly the 5 'Upcycled...' titled items, correctly sorted ascending by price). However, the 'Price Range' filter control (labeled '$0 - $500') is implemented with only a single interactive slider thumb whose aria-label is 'Minimum' (confirmed via DOM inspection: only one role=slider element inside the price filter container, and Tab from it moves focus to the next control, not a second thumb). There is no way to set a maximum price below the fixed $500 ceiling, so buyers cannot filter listings by a maximum price as required.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not filter browse results
    - Actual: On /categories, 'Furniture' showed '3 items'. Clicking the Furniture category card navigated to '/?category=furniture', but the resulting Browse page showed all 10 items unfiltered (count 'browse.count' = '10 items available'), and the Furniture checkbox in the filters panel was NOT checked (value=false). The category query parameter is not applied to the listing filter, so browse results are not restricted to the chosen category and the shown count does not match the category's actual item count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller has no way to view submitted purchase requests or buyer messages anywhere in the app
    - Actual: Full cross-user flow executed in one continuous tab (no reloads): (1) As seller 'Sam Seller', created listing 'CS13 Test Stool' ($20, Seattle WA). (2) Logged out, registered new buyer 'Betty Buyer'. (3) As Betty, opened the listing, sent a distinctive Message Seller text ('FT22-UNIQUE-MSG: Hi Sam, I'd like to buy this stool...') which showed 'Message sent to seller!' confirmation, then added the item to cart and clicked 'Send Purchase Request' on the cart page, which showed toast 'Purchase requests sent to sellers!' and correctly emptied the cart (0 items) - confirming the cart-request-sent confirmation only fires after the request is recorded. (4) Logged out, logged back in as Sam Seller (in-page navigation only, no reloads). Checked every plausible seller-facing surface: /my-listings (only shows 'View/Mark Sold/Delete' per listing, no request/message count or badge), the listing's own product detail page as owner (only shows 'Manage Your Listings' link, no requests section), the header (only theme toggle, Sell, Cart, and user-avatar buttons - no notifications/inbox icon), and 'My Profile' (routes to a broken 404 'Page not found'). Nowhere in the app could the seller see that a purchase request was submitted for their listing, nor view Betty's buyer message ('FT22-UNIQUE-MSG...'). The seller-side viewing capability required by this checklist item does not exist in the UI.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Wrong password is accepted for a registered account
    - Actual: Registered account qa.tester.2026@example.com / password123, logged out, then submitted login with the correct email but wrong password 'wrongpassword' (all via in-page navigation, no reloads). The app navigated to '/' and the header immediately showed the signed-in state (Sell, Cart, user-menu) instead of remaining logged out — the incorrect password was accepted and created an authenticated session. (An earlier attempt appeared to fail only because a subsequent full page reload had reset the app's in-memory auth state, masking the actual successful login.)

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated protected-route access not consistently blocked
    - Actual: While logged out: clicking 'Add to Cart' on a product correctly redirects to /login, and clicking 'Message Seller' correctly opens a dialog with a disabled 'Log in to message' button (clear sign-in prompts). However, directly navigating to /cart or /create-listing while unauthenticated renders a completely blank page (no elements, no redirect, no sign-in message) even after waiting 2s — it does not redirect to /login or show any sign-in prompt, unlike the public /categories route which renders correctly on direct navigation.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.