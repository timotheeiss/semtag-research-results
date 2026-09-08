# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for listings
    - Actual: On My Listings, each owned listing only exposes View, Mark Sold, and Delete actions. The View button and "Manage Your Listings" link on the product detail page only navigate back to My Listings; no Edit button, form, or route was found anywhere to modify an existing listing's title, description, photos, price, category, condition, location, or upcycled status.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing from /categories does not apply the category filter to results
    - Actual: On /categories, "Furniture" card correctly displayed "3 items". Clicking it navigated to /?category=furniture, but the Browse All Items section showed "10 items available" (all items, unfiltered) instead of the expected 3 Furniture items, and the Furniture checkbox in the sidebar Filters panel was NOT checked. The category selection from the Categories page has no effect on the displayed results/count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests or buyer messages
    - Actual: As buyer (Emma), sent a Message Seller note and a cart "Send Purchase Request" to Marcus Chen's "Vintage Denim Jacket" listing (both confirmed sent via toasts). Logged in as Marcus Chen (the seller) and checked all reachable views: header icons, profile dropdown (My Profile, My Listings, Log out), My Listings page, and each listing's actions (View/Mark Sold/Delete) - none show received purchase requests or buyer messages. The "My Profile" link itself returns a 404 "Page not found". No requested item or buyer message is viewable by the seller anywhere in the app.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted, allowing sign-in without the correct password
    - Actual: Logged in as emma@example.com using password "wrongpassword123" (never set by this account). App redirected to home page with toast "Welcome back!" and header showed authenticated user "Emma Green" with Sell button and profile menu, i.e. a signed-in session was created despite the wrong password.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: No missing-field feedback shown to seller
    - Actual: Clicking "Create Listing" with all fields empty (no photo, title, description, price, category, condition, location) did not submit and did not navigate away, but produced no toast, inline error text, or any element with error/alert styling identifying which fields were missing - only the title textbox silently gained focus.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated direct access to protected routes shows a blank page instead of a sign-in prompt or redirect
    - Actual: While signed out: clicking "Add to Cart" correctly redirected to /login with a toast. Clicking "Message Seller" opened a dialog with a disabled "Log in to message" button (acceptable clear prompt). However, directly visiting /create-listing or /cart while signed out rendered a completely blank page (no content, no redirect, no login prompt - document.body.innerText was empty), leaving the user with no indication that sign-in is required.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [ ] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.
  - Bug Report:
    - Issue: Availability status is not explicitly/consistently displayed on active listings
    - Actual: Product detail page for an active listing (Reclaimed Wood Bookshelf) includes photo, "Upcycled" badge, category (Furniture), condition (New), title, price ($320), location (Portland, OR), listed date (Listed 2024-11-23), description, and seller (James Wilson) - all present. However, full page text (document.body.innerText) contains no explicit availability label (e.g., "Available"/"In Stock") anywhere; availability is only implied by the presence of the "Add to Cart"/"Message Seller" buttons. This is inconsistent with sold listings, which DO show an explicit "This item is no longer available" text (confirmed in FT-4 testing). Active listings should also explicitly state their availability status rather than leaving it implicit.