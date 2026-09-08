# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit functionality exists for listings
    - Actual: Checked My Listings page and the owner's product detail page for a way to edit a listing. My Listings only offers View, Mark Sold, and Delete buttons per item; the detail page for an owned listing only offers "Manage Your Listings". No edit form, edit button, or inline-editing affordance exists anywhere for title, description, photos, price, category, condition, location, or upcycled status.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: Price filter has no maximum-price control; only a "Minimum" thumb exists
    - Actual: Category, condition, upcycled-only, distance filters and Newest/Price-Low/Price-High/Distance sorts all work correctly individually and in combination (verified: Furniture+Upcycled -> 2 correct items; distance=1mi -> 0 items; distance=2mi -> 0 items (min real distance is 3mi); price-low/high sorts produced correct ascending/descending order; distance sort produced correct ascending order 3,4,5,6,7,8,9,12,15 mi). However, DOM inspection of [data-semtag-id='browse.filters.price'] shows only a single role=slider element with aria-label="Minimum" (min=0,max=500) - there is no second "Maximum" thumb, so buyers cannot actually set a maximum price bound despite the UI displaying a "$0 - $500" range label.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from Categories page does not filter browse results
    - Actual: Categories page correctly shows per-category counts (Furniture 2, Clothing 1, Electronics 1, Home Decor 2, Books 1, Sports 1, Toys 1, Other 0, summing to 9 total). Clicking the "Furniture" category card navigates to http://localhost:6023/?category=furniture, but the resulting browse page does NOT apply the furniture filter: browse.count still reads "9 items available" (unfiltered), the Furniture checkbox in browse.filters.categories remains unchecked (value=false), and all 9 products (including clothing, electronics, books, etc.) are shown instead of only the 2 furniture items.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view exists for purchase requests or buyer messages
    - Actual: As Marcus, added "Yoga Mat & Blocks Set" (prod-7, sold by Sofia Martinez) to cart and clicked "Send Purchase Request" on /cart; the cart emptied (0 items) with no visible confirmation detail beyond that. Logged out and logged in as Sofia Martinez (the seller) to check if she could view the requested item/buyer message: checked the user menu (only "My Profile" and "My Listings" links exist, no Messages/Requests/Orders/Inbox), checked /my-listings (each row only shows title, price, status, View/Mark Sold/Delete - no request or message indicator), and checked the product detail page for prod-7 as owner (shows only category/condition/title/price/description/seller name and a "Manage Your Listings" link - no buyer message or purchase-request content). There is no discoverable UI anywhere in the app for a seller to view a submitted purchase request or an associated buyer message, so this requirement cannot be satisfied.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email (case-only variation) not rejected; existing account replaced
    - Actual: Registering "TIM.TEST.QA@example.com" (case variant of already-registered tim.test.qa@example.com) succeeded, signed in as new "Duplicate User" account, and the original account's credentials (tim.test.qa@example.com / secret6) subsequently failed login with "Invalid credentials" — the original account was overwritten/replaced instead of the registration being rejected.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Correct password for a registered (non-demo) account is not accepted on login
    - Actual: Registered account "logincheck123@example.com" / "checkpass1" was created and auto-signed-in successfully. After logging out and attempting to log back in with the exact same, correct credentials, the app returned "Invalid credentials. Try: emma@example.com" and did not sign in. Only the 3 hardcoded demo accounts (emma/marcus/sofia@example.com, any password) can log in; freshly registered accounts can never re-authenticate via the login form.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: No missing-field feedback shown; form just silently fails to submit
    - Actual: Clicking "Create Listing" with no photo, title, description, price, category, or condition produced no navigation (stayed on /create-listing) but also no toast, inline error text, or aria-invalid markers anywhere on the page — the seller receives no indication of what is missing.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Inconsistent enforcement: some unauthenticated actions redirect/prompt, others render a blank page with no message
    - Actual: While signed out: navigating to /create-listing and /cart both render a completely blank page (no content, no redirect, no sign-in prompt). In contrast, clicking "Add to Cart" on a product correctly redirects to /login with toast "Please log in to add items to your cart", and "Message Seller" opens a dialog with a disabled "Log in to message" button. Since /create-listing and /cart give no clear sign-in prompt or redirect, the constraint is not consistently met.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No duplicate-account feedback shown; registration silently succeeded and replaced the existing account
    - Actual: Submitting registration with an already-used email (case variant) produced no error message and no return to the form; instead the app navigated to the home page signed in as the new duplicate account, overwriting the original.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.