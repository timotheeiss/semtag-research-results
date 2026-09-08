# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update functionality for listings
    - Actual: In My Listings, each listing only offers "View", "Mark Sold", and "Delete" actions; there is no Edit control. Clicking the listing title or image does nothing (no navigation). Directly navigating to a guessed edit route (/edit-listing/:id) returns a 404. Sellers have no way to update an existing listing's details (title, description, price, category, condition, location).

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update functionality for listings
    - Actual: Same as FT-2: My Listings page provides no Edit action, only View/Mark Sold/Delete. There is no UI path for a seller to update information on a product they own.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: No verifiable mechanism removes only unavailable/sold items from an existing cart
    - Actual: As Emma, added two items to cart from different sellers (Kids Wooden Train Set/Sofia and Harry Potter Book Set/Marcus), confirmed cart badge showed "2". Logged in as Sofia and marked the Kids Wooden Train Set "Sold" (confirmed via My Listings status change and it disappearing from the public Browse grid, 10->9 items). Logged back in as Emma to check the cart: it showed "0 items"/"Your cart is empty" - i.e. BOTH the now-sold item and the still-active Harry Potter book were wiped, not just the unavailable one. This indicates the cart is indiscriminately cleared on logout/login rather than being selectively filtered based on listing availability; no distinct feature was found that flags/removes only unavailable items while preserving valid cart items during a session.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [ ] CS-11: Accounts that have already been registered cannot be registered again.
  - Bug Report:
    - Issue: Duplicate registration allowed
    - Actual: Submitted registration form again with the exact same email (timothee.qa.test@example.com) already used for an existing account. The app accepted it and displayed "Account created successfully!" instead of rejecting the duplicate registration.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Login succeeds with incorrect password
    - Actual: Logged in as demo account emma@example.com using a deliberately wrong password ("definitely-wrong-password"). App displayed "Welcome back!" and logged in successfully as Emma Green instead of rejecting the incorrect password. (Login page itself states "Demo accounts... any password works", confirming password is not validated.)

- [ ] CS-13: Sellers cannot update other people's product information.
  - Bug Report:
    - Issue: Update feature does not exist, so the restriction cannot be verified
    - Actual: Since no listing-update feature exists at all in the UI (only View/Mark Sold/Delete, and the guessed edit route 404s), it is impossible to verify that sellers are blocked from updating other sellers' listings; the underlying capability being restricted is entirely absent.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No duplicate-account error feedback shown
    - Actual: Re-registering with an already-used email produced the same "Account created successfully!" toast as a normal successful registration, with no error/warning indicating the account already exists.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.