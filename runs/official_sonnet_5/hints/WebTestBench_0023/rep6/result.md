# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for existing listings
    - Actual: On the My Listings page, each owned listing only exposes 'View', 'Mark Sold', and 'Delete' actions (confirmed via both semantic hints and full accessibility snapshot). The product detail page for an owned listing shows only a 'Manage Your Listings' link back to that page. No edit/update control or route is exposed anywhere in the UI, so sellers cannot edit title, description, photos, price, category, condition, or location of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Filter combination logic incomplete: Price Range and Max Distance sliders do not function as true min/max range filters
    - Actual: Category+condition combos, upcycled toggle, and sort (price low-to-high) all combined correctly and updated result counts/order as expected. However the 'Price Range' control (labeled '$0 - $500') exposes only a single adjustable 'Minimum' thumb (confirmed via DOM inspection: 2 duplicate sliders both aria-label='Minimum', valuemax hardcoded at 500) — there is no way to set a maximum price below $500, so results cannot be filtered by both a minimum AND maximum price as the checklist requires. The 'Max Distance' slider was moved through many values (50→26→25→24→23→12→1-range attempts) via keyboard and drag while 5 upcycled listings (from cities San Francisco/Brooklyn/Portland, presumably far from the signed-in seller's location) remained visible unchanged at every distance value tested, indicating the distance filter does not actually narrow the result set. DOM inspection also found the distance control duplicated as 2 identical unlabeled sliders (valuemax=50), matching the same duplication bug seen on the price slider.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the Categories page does not filter browse results
    - Actual: On /categories, 'Furniture' was shown with a count of '3 items'. Clicking that category card navigated to '/?category=furniture', but the Browse page rendered all 10 unfiltered listings ('10 items available') and the 'Furniture' checkbox in the sidebar filter panel remained unchecked (state=false) — the category query parameter was placed in the URL but never applied to the actual product filter, so the result count (10) does not match the category's advertised count (3) and non-Furniture items (e.g. Yoga Mat, Denim Jacket, Headphones, Book Set) are still shown.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view exists for buyer purchase requests or messages
    - Actual: As buyer Emma Green, sent a 'Message Seller' text and a cart 'Send Purchase Request' for prod-2 (owned by Marcus Chen); both showed buyer-side confirmation toasts. Logged in as Marcus Chen (the seller) and checked My Listings and the product detail page for prod-2 — no message, purchase-request list, notification badge, or any UI element anywhere in the app surfaces the buyer's message or the purchase request to the seller. There is no inbox/requests page in the navigation (only Browse, Categories, About, Sell, Cart, user menu).


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted for a registered (non-demo) account
    - Actual: Logged in with email tim.qa.tester@example.com (registered with password 'password1') using password 'wrongpassword'. Submission redirected to home page and header displayed signed-in state (Sell link, cart icon, user menu), indicating an authenticated session was created despite the incorrect password.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated access to protected routes renders a blank page instead of a sign-in prompt or redirect
    - Actual: While logged out, navigating directly to /create-listing and to /cart both rendered a completely blank page (document.body.innerText === '', no header, no content, no error). No sign-in prompt, no redirect to /login, and no visible message was shown to the unauthenticated user.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.