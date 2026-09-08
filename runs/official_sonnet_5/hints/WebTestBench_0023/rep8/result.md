# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for listings
    - Actual: On My Listings, each owned listing only exposes 'View', 'Mark Sold', and 'Delete' actions. The listing detail page for an owned listing only shows a 'Manage Your Listings' link back to My Listings — no Edit button, form, or route exists anywhere in the UI to modify title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not filter browse results
    - Actual: Clicking 'Furniture' (stated as 3 items) or 'Books' (stated as 1 item) on the Categories page navigates to '/?category=furniture' or '/?category=books' respectively, but the resulting browse page shows all 10 items with 'Furniture'/'Books' category checkbox left unchecked and count unchanged at '10 items available' — the category filter is not actually applied to the results.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing UI exists to view a buyer's purchase-request message
    - Actual: After a buyer sends a purchase request/message to seller Sofia Martinez (regarding prod-4 Sony Headphones), logging in as Sofia shows no way to view it: her user menu only offers 'My Profile' and 'My Listings' (no Messages/Inbox/Requests link); her My Listings page lists all 3 of her listings (Sony Headphones, Yoga Mat, Kids Train Set) with only View/Mark Sold/Delete buttons and no message/request indicators; and her Profile page is broken, rendering a 404 'Page not found' screen instead of any profile/inbox content. There is no UI anywhere for a seller to view the requested item or buyer message.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted for login
    - Actual: Logged in as timothee.tester.qa@example.com using an incorrect password ('wrongpassword999' vs registered 'password123'). Login succeeded, redirected to home page with an authenticated session (Sell/Cart/user-menu visible in header) instead of being rejected.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected routes render blank instead of redirect/prompt when accessed directly while signed out
    - Actual: While signed out, clicking 'Add to Cart' correctly redirected to /login, and 'Message Seller' correctly showed a disabled 'Log in to message' button — both good. However, directly navigating to /create-listing or /cart while unauthenticated renders a completely blank page (no header, no content, no sign-in prompt, no redirect to /login) even after waiting; the user is left stuck with no feedback.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.