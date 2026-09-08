# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality for existing listings
    - Actual: On /my-listings, the owned listing 'Test Upcycled Chair' only exposes View, Mark Sold, and Delete actions - no Edit control. The product detail page for the same owned listing only shows a 'Manage Your Listings' button (which returns to /my-listings), with no edit option. Clicking the listing title/image does not open an edit form either. There is no discoverable way to change title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

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
    - Actual: Categories page showed 'Furniture 3 items'. Clicking the Furniture category card navigated to /?category=furniture, but the Browse section still displayed all 10 items (heading '10 items available') and the Furniture category checkbox in the filter sidebar was unchecked - the category filter was not applied to the results, and the shown count did not match the categories page's count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view of purchase requests / buyer messages
    - Actual: As buyer, sent a 'Message Seller' purchase message on prod-1 (toast 'Message sent to seller!') and used cart 'Send Purchase Request' for prod-2 (toast 'Purchase requests sent to sellers!', cart cleared to 0 items). Logged in as Marcus Chen (seller of prod-2). Neither the user menu (only 'My Profile'/'My Listings'), My Listings page, nor the owned product detail page (prod-2, only shows 'Manage Your Listings') expose any way to view received purchase requests or buyer messages. There is no requests/inbox feature in the app, so the seller cannot view the requested item and buyer message as required.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password accepted at login
    - Actual: Logging in with email timothee.qa.test@example.com and an incorrect password ('wrongpassword', account was registered with 'password123') redirected to home and created an authenticated session (user menu opened showing 'Timothee Issenmann' / timothee.qa.test@example.com, with Sell/Cart/My Listings/Log out visible). Password is not actually verified.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Inconsistent/incomplete auth gating on protected routes
    - Actual: In-app action-triggered auth checks work correctly (e.g., clicking 'Add to Cart' while logged out redirects to /login). However, directly navigating to protected routes while unauthenticated (/cart, /create-listing) does NOT redirect to login or show a clear sign-in prompt - it renders a completely blank page (only notification regions in DOM, no header/nav/content at all). Console shows a React warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating the redirect logic is broken/incomplete for direct route access. This leaves unauthenticated users on a dead blank page instead of a clear sign-in prompt or working redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.