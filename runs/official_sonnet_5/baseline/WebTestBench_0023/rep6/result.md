# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality for existing listings
    - Actual: On My Listings, each owned listing card only exposes 'View', 'Mark Sold', and 'Delete' actions (no Edit/Manage button that opens an editable form). The listing detail page for an owned item only shows a 'Manage Your Listings' button, which returns to the same View/Mark Sold/Delete list. There is no way found to modify title, description, photos, price, category, condition, or location of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from /categories page does not filter browse results
    - Actual: On /categories, 'Furniture' button showed '2 items'. Clicking it navigated to /?category=furniture, but the Browse page displayed '8 items available' (the full unfiltered set) including clearly non-furniture items (Yoga Mat & Blocks Set, Vintage Denim Jacket, Sony Headphones, Kids Wooden Train Set, Complete Harry Potter Book Set). The Furniture checkbox in the sidebar filter panel was also left unchecked. The category query param is not being applied to filter results.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view of purchase requests or buyer messages
    - Actual: As buyer Marcus, sent a purchase request via cart checkout (toast 'Purchase requests sent to sellers!') and a direct message via 'Message Seller' dialog (toast 'Message sent to seller!') for prod-9 owned by Sofia. Logged in as Sofia (the seller) and checked My Listings, the product detail page (owner view shows only 'Manage Your Listings'), and the header menu/icons - there is no requests inbox, message list, or any indicator/count showing the purchase request or buyer's message anywhere in the seller's UI. The confirmation toasts on the buyer side are the only evidence a request/message was sent; the seller has no way to view the requested item alongside the buyer's message.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email (case-only variation) not rejected
    - Actual: Registering a second account with 'TimQA.Tester@example.com' (case variant of already-registered timqa.tester@example.com) succeeded with 'Account created successfully!' toast and created a distinct signed-in account 'Tim Duplicate', instead of being rejected as a duplicate.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted
    - Actual: Logging in as emma@example.com with an incorrect password ('definitelyWrongPassword999') succeeded, showing 'Welcome back!' toast and signing in as Emma Green, instead of being rejected.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Inconsistent/incomplete blocking of unauthenticated actions
    - Actual: While logged out, unauthenticated action blocking is inconsistent across the app: (1) Header cart icon click: does nothing (no toast, no redirect, no dialog). (2) Quick 'add to cart' icon on a browse card: does nothing (no toast/redirect). (3) Product detail page 'Add to Cart' button: correctly redirects to /login. (4) Product detail page 'Message Seller' button: opens the message dialog but shows a disabled 'Log in to message' button instead of redirecting to /login (different UX than Add to Cart, though it does block sending). (5) Direct navigation to /cart while logged out: renders a completely blank page (only notification regions, no header/nav/content, no redirect to /login and no message). (6) Direct navigation to /create-listing while logged out: also renders a completely blank page with no redirect or message. While no unauthenticated purchase/listing action ultimately succeeds, the blocking mechanism is inconsistent (silent no-ops, redirects, disabled buttons, and blank unrecoverable pages all occur for different entry points to the same restriction), which is poor and inconsistent UX/constraint enforcement.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No duplicate-account feedback shown
    - Actual: Submitting registration with an already-used email (case variant) produced a success toast and navigated away from the registration form instead of showing duplicate-account error feedback.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.