# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update capability for existing listings
    - Actual: On My Listings, each listing only offers "View", "Mark Sold", and "Delete" buttons — no Edit option. Attempting a direct route guess (/edit-listing/{id}) returned a 404 page. There is no way to change a listing's title, description, price, photos, category, condition, or location after creation.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update capability for owned listings
    - Actual: Same as FT-2: My Listings page for the logged-in seller (Emma Green) only exposes View, Mark Sold, and Delete actions for her own listings — no way to update any listing information.

- [X] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [X] CS-11: Accounts that have already been registered cannot be registered again.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Password validation is not enforced for demo/seeded accounts
    - Actual: Logging in as emma@example.com with an incorrect password ("definitely_wrong_password_123") succeeded — toast "Welcome back!" shown and header switched to logged-in state as "Emma Green". Login page itself states "(any password works)" for demo accounts, confirming password is not actually verified.

- [ ] CS-13: Sellers cannot update other people's product information.
  - Bug Report:
    - Issue: Update/edit functionality is absent entirely, so this access-control restriction cannot be exercised or verified
    - Actual: Since no seller (including the owner) has any UI or route to update listing details (see FT-2/FT-3), it is not possible to confirm that sellers are specifically prevented from updating other sellers' listings versus their own; the restriction is untestable because the underlying update feature does not exist.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [X] IX-16: Creating duplicate accounts provides visual feedback.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.