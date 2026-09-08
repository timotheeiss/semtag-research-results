# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update functionality for existing listings
    - Actual: On 'My Listings' page, each listing only offers 'View', 'Mark Sold', and 'Delete' actions. The product detail page for an owned listing only offers 'Manage Your Listings' (navigates back to My Listings). No Edit/Update control exists anywhere in the UI to modify a listing's title, description, price, or other details after creation.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update functionality for own listings
    - Actual: Same as FT-2: no Edit control is available for a seller's own products; only View, Mark Sold, and Delete buttons exist on My Listings, so sellers cannot update the information of products they own.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: Cart is fully cleared instead of selectively removing only unavailable items
    - Actual: As Emma, added two items to cart: 'Kids Wooden Train Set' (prod-9, seller Sofia, remained Active throughout) and 'Vintage Denim Jacket - Size M' (prod-2, seller Marcus). Cart badge correctly showed '2'. Logged in as Marcus and marked only prod-2 as 'Sold' (prod-9 was never modified and remained Active, confirmed still visible in the 8-item browse list afterward). Logged back in as Emma and checked the Cart page: it showed '0 items' / 'Your cart is empty' — meaning BOTH items were removed, including the still-active, untouched 'Kids Wooden Train Set'. Expected behavior: only the sold/unavailable item (Vintage Denim Jacket) should be removed while the still-available item (Kids Wooden Train Set) remains in the cart. Instead the entire cart was wiped, indicating the app does not correctly filter out only unavailable items but rather clears the whole cart.

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
    - Issue: Login accepts incorrect password
    - Actual: Logging in as emma@example.com with an intentionally wrong password ('totallyWrongPassword') succeeded — user was authenticated as 'Emma Green' with toast 'Welcome back!'. Also, a freshly registered account (qa.tester.revive@example.com) could not log back in even with its own correct password, receiving 'Invalid credentials'. Password verification is not functioning correctly.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [X] IX-16: Creating duplicate accounts provides visual feedback.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [ ] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.
  - Bug Report:
    - Issue: One product photo is broken/fails to load (404), so it cannot display content related to its listing
    - Actual: Checked all 9 marketplace listing images (alt text and underlying image src/load status). 8 of 9 images loaded successfully with alt text and Unsplash source photos that appropriately correspond to their listing titles (e.g., 'Reclaimed Wood Bookshelf', 'Mid-Century Modern Armchair', 'Kids Wooden Train Set', etc., each unique, non-duplicated, and contextually plausible for the product category). However, the image for 'Handmade Macrame Wall Hanging' (prod-5) has src https://images.unsplash.com/photo-1622464689583-a09e88f0d34e?w=600&h=600&fit=crop which returns HTTP 404 (verified by navigating directly to the URL, which rendered a plain '404' page) and has naturalWidth/naturalHeight of 0 on both the Browse grid and the product detail page (/product/prod-5) despite img.complete being true. This means no photo is actually displayed for this listing, so its content cannot be verified as related to the described product — violating the requirement that all displayed product photos be related to the item.