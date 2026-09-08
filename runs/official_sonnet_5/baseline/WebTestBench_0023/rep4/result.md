# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality for existing listings
    - Actual: The My Listings page only exposes "View", "Mark Sold", and "Delete" actions per listing card; there is no "Edit" button or any UI path to modify an existing listing's title, description, photos, price, category, condition, location, or upcycled status. The product detail page (owner view) likewise only offers a "Manage Your Listings" link, no edit option.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing page does not filter results by chosen category
    - Actual: On /categories, "Furniture" tile correctly displayed "3 items". Clicking it navigated to /?category=furniture, but the Browse page ignored the category query param: it showed "10 items available" (all listings, including Clothing, Electronics, Books, etc.) and the "Furniture" checkbox in the Filters sidebar remained unchecked. Expected: browse results should show only the 3 available Furniture listings with a matching result count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller inbox to view purchase requests/messages
    - Actual: After a buyer (Emma) sent a purchase request via cart ("Purchase requests sent to sellers!") and a direct message via "Message Seller" ("Message sent to seller!"), there is no UI anywhere for the seller to view the requested item or buyer message: the header has no notifications/messages icon (only search and mobile-menu icons), the user dropdown only offers "My Profile" (which 404s), "My Listings", and "Log out", and My Listings shows no request/message indicator per listing.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email registration not rejected
    - Actual: Registering with the exact same email (qatester1@example.com) and with a case-only variation (QATester1@example.com) as an already-registered account both succeeded with "Account created successfully!" and signed the user in as a new/different account, instead of being rejected.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted for demo accounts
    - Actual: Login with emma@example.com and an incorrect password ("definitely_wrong_password_123") succeeded, showing "Welcome back!" and authenticating as "Emma Green". Login page itself even discloses "(any password works)" for demo accounts, confirming password is not verified for these registered accounts.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No duplicate-account feedback shown
    - Actual: Attempting to register with an already-used email (exact and case-variant) shows a success toast "Account created successfully!" and navigates away from the registration form instead of showing duplicate-account error feedback.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.