# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update functionality for existing listings
    - Actual: On My Listings page, each listing only offers "View", "Mark Sold", and "Delete" actions. There is no "Edit" button or any UI path to modify a listing's title, description, price, photos, or location after creation.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update functionality for owned listings
    - Actual: Same as FT-2: My Listings page and the product detail page for an owned listing expose only View, Mark Sold, and Delete — no editing capability is provided for sellers to update their own product information.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: Cart cannot be verified to exclude removed/unavailable items; cart state resets entirely on every login/logout transition
    - Actual: Added Sofia's Sony Headphones (prod-4) to Emma's cart (confirmed via toast + badge "1"). Logged out, logged in as Sofia, marked prod-4 as Sold (confirmed toast "Item marked as sold", listing status changed to Sold, Browse count dropped from 10 to 9 items - sold items correctly excluded from Browse). Logged out, logged back in as Emma to check if her cart now excluded/flagged the now-unavailable item - but her cart showed "0 items" (completely empty), not because the item was excluded due to unavailability, but because cart state does not persist across any login/logout transition at all. Verified this independently: added a different item (prod-8, James Wilson's floor lamp) to Emma's cart (badge showed "1"), logged Emma out and back in as Emma (same user, no account switch) - cart was again empty (no badge). This confirms the app's cart is purely in-memory per active session and is wiped on every auth transition, with no backend/API persistence (network inspection showed zero API calls, only static assets). Because of this, there is no legitimate UI flow to place an item in a cart and then have that same item become unavailable (via another user marking it sold) while the cart session remains active - sellers also cannot add their own listings to cart to self-test. The constraint could not be positively demonstrated as working since the only observable outcome (empty cart) is caused by session reset rather than availability-based filtering, and the cart UI has no visible "unavailable/sold" indicator logic to fall back on.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [ ] CS-11: Accounts that have already been registered cannot be registered again.
  - Bug Report:
    - Issue: Duplicate email registration is allowed
    - Actual: Re-registering with the already-used email tim.qa.tester@example.com (different name) succeeded: toast "Account created successfully!" was shown and the app logged in as the new duplicate account "Tim QA Tester Dup" instead of rejecting the registration.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Login succeeds with incorrect password
    - Actual: Logging in as emma@example.com with an incorrect password "wrongpassword999" succeeded: toast "Welcome back!" was shown and the app authenticated as "Emma Green". The login page itself states "(any password works)" for demo accounts, confirming passwords are not validated.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No duplicate-account error feedback; success feedback shown instead
    - Actual: Submitting a duplicate-email registration produced a success toast ("Account created successfully!") rather than any error/warning feedback, because the app does not detect duplicates.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.