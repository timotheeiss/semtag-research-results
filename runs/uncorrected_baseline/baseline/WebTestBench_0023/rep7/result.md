# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update feature for listings
    - Actual: On My Listings, each item only offers 'View', 'Mark Sold', and 'Delete' actions. The product detail page for an owned listing only shows a 'Manage Your Listings' link. No Edit control or edit form exists anywhere in the DOM (verified via full-page search for 'edit' text/buttons), so sellers cannot update details like price, description, photos, or location of an existing listing.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update feature for own listings
    - Actual: Same as FT-2: only View, Mark Sold, and Delete actions are available for a seller's own listings; there is no way to modify product information after creation.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: No verifiable mechanism excludes unavailable items from cart; cart state is unreliable across sessions
    - Actual: Test: added Sofia's 'Sony Noise-Canceling Headphones' to Emma's cart (badge=1), then logged in as Sofia and marked that listing 'Sold' (toast 'Item marked as sold', browse count dropped 10→9 items, persisting across sessions). Logged back in as Emma and found the cart empty (0 items). However, a control test revealed this is NOT proof of active unavailable-item filtering: adding a still-AVAILABLE item ('Yoga Mat & Blocks Set') to Emma's cart, then logging out and back in as Emma (no ownership/availability change at all), also reset the cart to empty (no badge shown). This proves cart contents are cleared on ANY login/logout transition, regardless of item availability. Therefore the original observation is confounded and does not demonstrate a genuine 'cart cannot contain unavailable items' feature - no explicit unavailable/sold indicator, warning, or automatic-removal notice was ever observed within a cart while items were present. There is no positive evidence the app actively checks/filters cart items against current availability.

- [X] FT-5: Users can register new accounts

- [X] FT-6: Buyers can filter and sort products by category, price range, condition, and distance.

- [X] FT-7: Buyers can search for products using keywords.

- [X] FT-8: Buyers can add items to their shopping cart

- [X] FT-9: Buyers can send shopping requests


## Constraint
- [X] CS-10: Users cannot log in if they do not have a registered account.

- [ ] CS-11: Accounts that have already been registered cannot be registered again.
  - Bug Report:
    - Issue: Duplicate account registration not prevented
    - Actual: Registering with the same email (qa.tester.revive@example.com) a second time succeeded again, showing 'Account created successfully!' toast and returning to home page logged in, instead of being rejected.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Password not validated for demo/existing accounts
    - Actual: Login page itself states 'Demo accounts... (any password works)'. Logging in as emma@example.com with an arbitrary incorrect password 'anyrandompassword' succeeded and navigated to the homepage logged in, instead of being rejected.

- [ ] CS-13: Sellers cannot update other people's product information.
  - Bug Report:
    - Issue: Update feature absent, constraint unverifiable/unmet
    - Actual: The application provides no listing-edit functionality at all (confirmed no 'Edit' controls exist in the UI or DOM), so there is no mechanism through which sellers could attempt to update another seller's product information to begin with. Since FT-2/FT-3 (update capability) fail, the constraint that depends on it cannot be demonstrated as enforced.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No distinct feedback for duplicate account creation
    - Actual: Submitting registration with an already-registered email shows the same generic 'Account created successfully!' toast as a first-time registration; no error or duplicate-account warning is displayed.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.