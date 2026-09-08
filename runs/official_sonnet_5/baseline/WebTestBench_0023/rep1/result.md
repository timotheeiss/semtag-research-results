# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for listings
    - Actual: On My Listings, each owned listing only exposes View, Mark Sold, and Delete actions — no Edit control. The product detail page for an owned listing only shows a "Manage Your Listings" button, no edit option. Attempting to navigate directly to /edit-listing/{id} returns a 404 "Page not found". There is no way to edit title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not filter Browse results
    - Actual: On /categories page, "Home Decor" showed "3 items". Clicking it navigated to /?category=home-decor (URL query param correctly set), but the Browse page displayed "11 items available" showing the full unfiltered grid (all 11 listings across all categories, e.g. Yoga Mat & Blocks Set (Sports), Vintage Denim Jacket (Clothing), Sony Headphones (Electronics), Kids Wooden Train Set (Toys), etc.), and the "Home Decor" checkbox in the Categories filter sidebar was not checked/active. Expected: only the 3 Home Decor category listings should be shown with a matching count, per the category page's own stated count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller cannot view buyer's purchase request or message anywhere in the UI
    - Actual: As buyer Blake Buyer, added seller Sasha Seller's listing "QA Test Vase FT22" to cart, sent a message via Message Seller ("Message sent to seller!" confirmed), and clicked "Send Purchase Request" in cart (toast "Purchase requests sent to sellers!" shown, cart cleared to 0 items). Logged out and logged back in as Sasha Seller (the listing owner). Checked: (1) the notification/bell icon in header - clicking it produced no dropdown/panel, just navigated to home with no visible change; (2) user menu - only contains "My Profile" and "My Listings", no Messages/Requests/Notifications option; (3) My Listings page - the listing still shows plain "Active" status with only View/Mark Sold/Delete actions, no indicator of a pending purchase request or unread message; (4) the listing's own detail page (as owner) - only shows "Manage Your Listings" button, no request/message content or badge anywhere. There is no UI surface in the application where a seller can see that a purchase request was submitted or read the buyer's message.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted, granting a signed-in session
    - Actual: Registered account casey.pwtest.qa@example.com with password "correctpass1". Logging in with the same email but an incorrect password "wrongpassword" was accepted: toast "Welcome back!" appeared and the header showed the user as signed in ("Casey Password"), instead of being rejected.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Missing required fields not identified to seller
    - Actual: Submitting the Create Listing form completely empty (no photo, title, description, price, category, condition, location) did not create a listing (correct), but no error message, inline field highlighting, or toast was shown anywhere identifying which fields were missing. Only the Title field silently received focus.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Non-numeric price accepted and listing created with invalid price
    - Actual: Negative price (-25) was correctly rejected (no listing created, stayed on form). However, submitting a non-numeric price value ("abc" in the price field) was accepted: the app navigated to My Listings with toast "Listing created successfully!" and the new listing "QA Test Chair 2" was created and displayed with price "$NaN" instead of being rejected.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Direct navigation to protected routes while unauthenticated shows a blank page instead of a redirect or sign-in prompt
    - Actual: In-app interactive actions correctly guard authentication: clicking "Add to Cart" on a product detail page while logged out shows toast "Please log in to add items to your cart" and redirects to /login (good); clicking "Message Seller" while logged out shows the message dialog but properly restricted per earlier testing. However, directly navigating (full page load) to protected routes while unauthenticated produces a completely blank page with no content, no redirect to /login, and no sign-in prompt: /create-listing renders blank, /cart renders blank, /my-listings renders blank. Only the notification region divs are present in the DOM; the entire app shell (header, content) fails to render, giving the user no indication of why the page is empty or how to sign in.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.