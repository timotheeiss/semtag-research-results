# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit listing functionality exists
    - Actual: On /my-listings, each owned listing (e.g. 'Mid-Century Modern Armchair') only offers View, Mark Sold, and Delete buttons — no Edit option. Clicking View opens the product detail page which, for the owner, shows only a 'Manage Your Listings' button that navigates back to /my-listings; no edit form, modal, or route was found anywhere to modify title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category page navigation does not apply category filter to results
    - Actual: On /categories, 'Furniture' category card correctly displayed count '2 items' (matching the actual furniture listings). Clicking it navigated to /?category=furniture, but the Browse page ignored the category query param: it displayed 'Browse All Items' with '8 items available' (the full unfiltered catalog) and the Furniture checkbox in the sidebar filter panel was NOT checked/applied. Expected: navigating from a category card should filter results to that category (2 furniture items) with a matching count, consistent with what the category card itself displayed.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view for received purchase requests or buyer messages
    - Actual: As a buyer (Emma), sending a cart purchase request to seller James Wilson (prod-10) produced a buyer-side confirmation toast 'Purchase requests sent to sellers!', and sending a direct message via 'Message Seller' produced 'Message sent to seller!'. However, no UI surface exists for the seller to view these: the account dropdown menu offers only 'My Profile' (navigates to /profile which renders a 404 'Page not found'), 'My Listings' (shows only View/Mark Sold/Delete per listing with no indication of requests or messages received), and there is no notifications/inbox icon in the nav. Sellers therefore have no way to view purchase requests or buyer messages sent to them.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password not verified on login
    - Actual: Registered a fresh account (qa.buyer.test2@example.com) with password "correctpass1". After logging out, signing in with the same email but an incorrect password "wrongpassword999" succeeded: toast "Welcome back!" appeared and nav showed the authenticated "QA Buyer" session. Also observed same behavior with demo account emma@example.com and a wrong password (login page itself notes "(any password works)"). Incorrect passwords are not rejected.

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