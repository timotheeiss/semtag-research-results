# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update functionality for listing details
    - Actual: On My Listings page, each listing only offers "View", "Mark Sold", and "Delete" actions. There is no "Edit" button, pencil icon, or any other affordance to modify a listing's title, description, price, category, condition, or location. The product detail page for own listings also has no edit control (only Manage Your Listings link, wishlist, share, and cart icons).

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update functionality for owned product listings
    - Actual: Same as FT-2: reviewed My Listings for seller Emma Green; only View/Mark Sold/Delete actions exist for owned listings, with no way to update listing information (title, price, description, etc.).

- [X] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [ ] CS-11: Accounts that have already been registered cannot be registered again.
  - Bug Report:
    - Issue: Duplicate account registration not blocked
    - Actual: Submitted the registration form again with the same email (qa.tester.revive@example.com) already used to create an account. The app accepted it, showed "Account created successfully!" toast, and redirected to home logged in, instead of rejecting the duplicate registration.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Incorrect password is accepted for login
    - Actual: Logged in with demo account emma@example.com using an arbitrary/incorrect password ("definitely_wrong_password_123"). Login succeeded with toast "Welcome back!" and the user was authenticated as Emma Green, instead of being rejected for the wrong password.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No error/duplicate feedback shown; misleading success toast
    - Actual: Re-registering with an already-used email produced the same "Account created successfully!" toast as a first-time registration, with no indication that the account/email already existed.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.