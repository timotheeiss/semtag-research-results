# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: Missing feature: no way to edit/update listing details
    - Actual: On My Listings page, each listing card only offers "View", "Mark Sold", and "Delete" actions. No "Edit" button/link exists anywhere in the UI, and the product detail page for the owner only shows a "Manage Your Listings" button, not an edit form. Confirmed via DOM query that no element contains "edit" text or href.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: Missing feature: sellers cannot update their own product listings
    - Actual: Same as FT-2: no edit UI exists for listings owned by the current user. Only View, Mark Sold, and Delete are available.

- [X] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [X] CS-11: Accounts that have already been registered cannot be registered again.

- [X] CS-12: Unable to log in when user account password is incorrect.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [X] IX-16: Creating duplicate accounts provides visual feedback.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.