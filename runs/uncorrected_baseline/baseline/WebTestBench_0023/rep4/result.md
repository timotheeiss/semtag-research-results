# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update functionality for existing listings
    - Actual: In My Listings, each owned listing only offers 'View', 'Mark Sold', and 'Delete' actions. No 'Edit' button/link exists anywhere (product detail page, listing card, or DOM search for 'edit' text returned no results), so sellers cannot update title, description, price, or location of an existing listing.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update functionality for own listings
    - Actual: Same as FT-2: only View, Mark Sold, and Delete actions are available for a seller's own listings; there is no way to modify listing fields after creation.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: Cart does not validate item availability; also no cross-session sync of item status
    - Actual: Logged in as Emma (buyer, browser tab A) and added Marcus Chen's 'Vintage Denim Jacket - Size M' (prod-2, $65) to cart. In a separate tab (tab B) logged in as Marcus (the seller/owner), navigated to My Listings and clicked 'Mark Sold' on that same item, which succeeded (status changed to 'Sold' in tab B). Returning to tab A (still logged in as Emma, no reload/logout) and opening the cart, the item was still shown as a normal active line item ('Vintage Denim Jacket - Size M', $65.00, San Francisco CA) with no 'sold'/'unavailable' indicator, and the 'Send Purchase Request' button remained fully enabled — the cart never reflects that the item became unavailable. Additionally, the product detail page for prod-2 viewed in tab A still displayed 'Add to Cart' as available (Active), confirming the app has no shared/persistent backend state between user sessions, so item availability changes made by a seller are never propagated to a buyer's existing cart or product view.

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
    - Actual: Re-registering with the same email (tim.qa.tester@example.com) that was already used succeeded, showing 'Account created successfully!' and logging the user in again, instead of being rejected as an existing account.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Login does not validate password
    - Actual: Login page itself states 'Demo accounts... (any password works)'. Confirmed by logging in as emma@example.com with a deliberately wrong password 'definitely_wrong_pw_123' — login succeeded with toast 'Welcome back!' and user was authenticated as Emma Green. Incorrect passwords are not rejected.

- [X] CS-13: Sellers cannot update other people's product information.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No error feedback for duplicate account creation
    - Actual: Attempting to create a duplicate account shows a success toast 'Account created successfully!' rather than any error/warning feedback indicating the account already exists.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [ ] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.
  - Bug Report:
    - Issue: Broken/missing product photo
    - Actual: Checked all product photo URLs across the browse listing via DOM (img src/alt attributes) and direct fetch requests. 8 of 9 checked images (Reclaimed Wood Bookshelf, Mid-Century Modern Armchair, Vintage Denim Jacket, etc.) loaded successfully (HTTP 200) with alt text correctly matching their product titles. However, the photo for 'Handmade Macrame Wall Hanging' (prod-5, src https://images.unsplash.com/photo-1622464689583-a09e88f0d34e?w=600&h=600&fit=crop) returned HTTP 404 and failed to load (img.naturalWidth=0), meaning no relevant (or any) photo is actually displayed for that listing despite the alt text being correctly labeled.