# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No listing edit/update functionality available
    - Actual: On My Listings, the only actions available per listing are "View", "Mark Sold", and "Delete" — there is no Edit control. Clicking the listing's image or title does not navigate to an edit form; "View" only opens the read-only public product page with a "Manage Your Listings" button that returns to the same My Listings list. Sellers cannot change title, description, price, or location of an existing listing.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No listing edit/update functionality available
    - Actual: Same as FT-2: My Listings only offers View, Mark Sold, and Delete actions for owned listings; there is no way to update any product information (title, description, price, category, condition, location, photos) after creation.

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
    - Issue: Duplicate account registration is not blocked
    - Actual: Registered a second account using the already-registered email qa.tester.revive@example.com (different name "QA Tester Duplicate", different password). The app accepted it, showed "Account created successfully!" toast, and logged in as the new duplicate account instead of rejecting the registration.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Login accepts incorrect password for a registered account
    - Actual: Logged in as qa.tester.revive@example.com using password "WrongPassword999" (registered password was "Test1234!"). App redirected to home page, showed "Welcome back!" toast, and header displayed the authenticated QA Tester profile menu — login succeeded despite the wrong password.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No duplicate-account warning/error feedback shown
    - Actual: Attempting to re-register with an already-used email produced the same success toast ("Account created successfully!") as a normal new registration, with no error/warning indicating the email was already registered.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.