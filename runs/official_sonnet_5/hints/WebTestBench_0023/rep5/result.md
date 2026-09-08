# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit/update action exists for an existing listing
    - Actual: On My Listings, the only seller-management actions available per listing are "View", "Mark Sold", and "Delete" (confirmed via semantic_snapshot action group and full accessibility snapshot). The owned listing's detail page only offers a "Manage Your Listings" link, no edit control. There is no UI path to modify title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter control exists; the "Price Range" slider only has a Minimum thumb
    - Actual: Combining Category=Furniture + Condition=New correctly returned 2/2 matching items. Setting the price slider to $250 (min) correctly excluded the $175 item, leaving only the $320 item ≥$250. However, DOM inspection of the Price Range control (data-semtag-id=browse.filters.price) shows only ONE slider thumb, aria-label="Minimum" — there is no second "Maximum" thumb in the DOM or in tab order (Tab from the minimum thumb moves directly to "Clear all filters"). The displayed "$X - $500" label is misleading since $500 (the upper bound) cannot be changed by the user. Category, condition, and distance filters, plus Newest/Price/Distance sort, all worked correctly and combined properly, but the checklist's required "maximum price" filter is not actually implemented/exposed.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from /categories page does not filter browse results
    - Actual: Clicking the 'Furniture' category card on /categories navigated to /?category=furniture (URL updated), but the Browse All Items page displayed all 10 items with count '10 items available' instead of the 3 furniture items listed on the categories page. The Furniture checkbox in the sidebar filter panel remained unchecked (value=false). The category query parameter is not applied to the actual filter/result state.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing feature to view purchase requests or buyer messages
    - Actual: As buyer "Seller One", added prod-1 to cart and sent a purchase request (toast: "Purchase requests sent to sellers!", cart emptied). Logged in as the item's actual seller "Emma Green" and searched all available surfaces (user menu: My Profile [404 - not found], My Listings, product detail page for prod-1) for any request/inbox/notification view. None exists: My Listings only shows View/Mark Sold/Delete with no requests indicator, and the owned product detail page shows no buyer messages or purchase-request list. There is no way for a seller to view a submitted purchase request or its buyer message.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Login succeeds with an incorrect password for a non-demo registered account
    - Actual: Registered account duptest.qa@example.com with password "CorrectPass123". Logged out, then attempted sign-in with the same email but wrong password "WrongPassword999" (this account is not one of the documented demo accounts where "any password works"). The app nonetheless created an authenticated session: page navigated to home with header showing Sell/Cart/user-menu, and the user menu displayed "Dup Test User" / "duptest.qa@example.com" — i.e. successfully signed in despite the incorrect password. Password is not actually validated on login.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated direct navigation to protected routes yields a blank page instead of a sign-in prompt or redirect
    - Actual: While signed out, clicking "Add to Cart" on a product page correctly redirects to /login (good). However, directly navigating to http://localhost:6023/create-listing or http://localhost:6023/cart while unauthenticated renders a completely blank page (document.body.innerText === "", no header, no content, no redirect, no error) instead of a clear sign-in prompt or redirect to login. Reproduced twice for /create-listing.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.