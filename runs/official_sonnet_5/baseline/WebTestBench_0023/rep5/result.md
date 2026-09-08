# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for listings
    - Actual: On My Listings, each owned listing card only exposes 'View', 'Mark Sold', and 'Delete' buttons. The listing detail page for an owned item only shows a 'Manage Your Listings' button (which returns to My Listings). No 'Edit' button/link/icon exists anywhere in the DOM (checked via full button/link text and href scan) and there is no route to modify an existing listing's title, description, photos, price, category, condition, location, or upcycled status.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing link does not apply the filter
    - Actual: On /categories page, "Furniture" category card correctly displayed "3 items". Clicking it navigated to /?category=furniture, but the Browse page still displayed all "10 items available" (unfiltered) and the "Furniture" checkbox in the sidebar filter panel was NOT checked. The category browsing link/URL query param has no effect on the actual results shown - the count and item list did not match the selected category.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing UI exists to view received purchase requests or buyer messages
    - Actual: Logged in as buyer and sent a "Message Seller" text to Marcus Chen (seller of Vintage Denim Jacket, prod-2) and also sent a cart purchase request for Reclaimed Wood Bookshelf (prod-10). Then logged in as Marcus Chen (the seller who received the message) and checked all reachable seller-facing surfaces: header icons (only Sell button and Cart icon present, no messages/notifications icon), the "Marcus Chen" account dropdown menu (only "My Profile", "My Listings", "Log out"), the My Listings page (each listing card shows only View/Mark Sold/Delete buttons with no message/request indicator or count), and My Profile (route /profile returns a 404 "Page not found"). There is no inbox, messages, or purchase-requests view anywhere for the seller to see the buyer's message or the purchase request that were sent.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified on login for registered accounts
    - Actual: Retested cleanly (no intervening reload): registered Tim Tester (tim.tester.qa@example.com / secret6, in-app), logged out via in-app menu, then on /login submitted email tim.tester.qa@example.com with an incorrect password 'wrongpassword'. Login succeeded: toast 'Welcome back!' and the app navigated to the signed-in home view as 'Tim Tester'. Any password is accepted for a registered account, not just the correct one, so incorrect passwords are not rejected.

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