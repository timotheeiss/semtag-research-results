# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit/update control exists for a seller's own listing
    - Actual: On My Listings, each owned listing only exposes View, Mark Sold, and Delete buttons (confirmed via full accessibility snapshot and DOM query for any element with an 'edit' id — none found). The product detail page for the owner only shows a 'Manage Your Listings' link, no edit action. There is no way to change title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Missing maximum-price filter control
    - Actual: Category, condition, upcycled-only, distance, and sort (newest/price-low/price-high/distance) filters all correctly combine and update results. However, the price range slider (browse.filters.price) contains only ONE Radix slider thumb (aria-label='Minimum', min=0 max=500) - confirmed via raw innerHTML dump of the slider container. There is no 'Maximum' thumb anywhere in the DOM, so buyers cannot set an upper price bound; the $500 ceiling is fixed/non-adjustable despite the filter being labeled 'Price Range' implying a min AND max are settable.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from categories page does not filter browse results
    - Actual: Navigating to /categories showed 'Furniture 3 items'. Clicking the Furniture category card navigated to /?category=furniture, but the Browse page displayed all 10 items (browse.count = '10 items available'), not the expected 3 furniture items. The Furniture checkbox in browse.filters.categories remained unchecked (value: false), confirming the category query param is not being read/applied to the filter state.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view exists for purchase requests or buyer messages
    - Actual: As buyer, sent a direct 'Message Seller' text to Marcus Chen and submitted a cart Send Purchase Request for 2 items (one from Marcus Chen); both produced buyer-side confirmation toasts ('Message sent to seller!' and 'Purchase requests sent to sellers!'). However, logging in as seller Marcus Chen, there is no way to view these: the user menu only offers My Profile (which 404s - 'Oops! Page not found'), My Listings (shows only View/Mark Sold/Delete, no requests/messages), and the product detail page for an owned listing shows no message/requests section. No notification icon or inbox exists in the header. The cart-checkout 'reported as sent' behavior could be observed (toast + cart cleared), but the first half of the requirement - sellers viewing the requested item and buyer message - is not implemented anywhere in the UI.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Registered account accepts incorrect password
    - Actual: Registered a fresh account (qa.credtest@example.com / password 'CorrectPass123'), logged out, then attempted login with the same email but WRONG password 'WrongPassword999'. Login succeeded: toast 'Welcome back!' appeared, page redirected to '/', and header showed the authenticated user-menu button 'QA Cred Tester'. Password verification is not enforced for self-registered accounts (any password is accepted, similar to demo accounts, but this was NOT one of the documented demo accounts).

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Direct navigation to protected routes while signed out renders a blank page instead of a sign-in prompt or redirect
    - Actual: While logged out: clicking 'Add to Cart' on a product correctly redirected to /login with toast 'Please log in to add items to your cart', and opening 'Message Seller' correctly showed the dialog with a disabled 'Log in to message' button - both good. However, directly navigating (URL entry / deep link) to protected routes /create-listing and /cart while unauthenticated rendered a completely blank page (no header, no content, no redirect, no message) rather than being blocked with a clear sign-in prompt or redirect.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.