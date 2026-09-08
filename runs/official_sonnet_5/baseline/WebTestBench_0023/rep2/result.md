# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality available for listings
    - Actual: On My Listings, the only actions available per listing are 'View', 'Mark Sold', and 'Delete' — there is no 'Edit' button or link. The owned listing's detail page only shows a 'Manage Your Listings' button, with no inline edit controls. No route or UI element exists to edit title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category page link does not apply filter on Browse page
    - Actual: On /categories, "Furniture" card correctly shows "3 items". Clicking it navigates to /?category=furniture, but the Browse page ignores the query param: heading shows "10 items available" (all items), the Furniture checkbox in the Filters sidebar is unchecked, and the full unfiltered list of 10 items (including Clothing, Electronics, Sports, etc.) is displayed instead of only the 3 Furniture items.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller cannot view/manage purchase requests
    - Actual: After buyer (Sofia) sent a purchase request for Marcus's "Vintage Denim Jacket - Size M" (confirmed via toast "Purchase requests sent to sellers!" and cart clearing), logged in as seller Marcus Chen and checked My Listings, the item's product detail page, and header icons (cart/notification icons) — no purchase request, notification badge, or request-management UI was found anywhere. The listing still just shows generic "Active" status with View/Mark Sold/Delete actions, with no indication a purchase request was received. There appears to be no seller-side feature to view or respond to purchase requests at all.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email not rejected for case-only variation
    - Actual: Registering with 'Timothee.QA.Test26@Example.com' (case-variant of already-registered 'timothee.qa.test26@example.com') succeeded: toast said 'Account created successfully!' and a new account 'Duplicate Tester' was created and signed in, instead of being rejected as a duplicate.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password not verified on login
    - Actual: Registered account 'qa.persist.check77@example.com' with password 'PersistPass123'. Logging in with an incorrect password 'WrongPass000' succeeded: toast 'Welcome back!' shown and user was signed in as 'QA Persist Test', instead of being rejected.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Missing required fields not identified to seller (except photo)
    - Actual: Submitting the Create Listing form with all fields empty produced no visible feedback at all (no toast, no field error text), just silently failed. Submitting with title and location empty (but photo/description/price/category/condition filled) also produced no toast or visible error message, only silent focus on the Title field. Only the missing-photo case shows a clear toast ('Please add at least one image'); other missing required fields are not identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.