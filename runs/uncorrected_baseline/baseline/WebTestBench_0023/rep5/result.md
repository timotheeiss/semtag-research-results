# Test Result

## Functionality
- [X] FT-1: Sellers can create product listings that include photos, descriptions, prices, and locations.

- [ ] FT-2: Sellers can update the details of existing product information.
  - Bug Report:
    - Issue: No edit/update feature exists for listings
    - Actual: On My Listings, each listing card only offers "View", "Mark Sold", and "Delete" buttons — no "Edit" option. The product detail page for an owned listing only shows a "Manage Your Listings" button, no inline edit fields. App source confirms App.tsx registers routes for /, /product/:id, /login, /register, /create-listing, /my-listings, /cart, /about, /categories, /not-found only — no edit-listing route — and MyListings.tsx contains no "edit" functionality at all. Sellers cannot update title, description, price, photos, or location of an existing listing.

- [ ] FT-3: Sellers can update the information of the products they own.
  - Bug Report:
    - Issue: No edit/update feature exists for listings (duplicate of FT-2)
    - Actual: Same as FT-2: the only actions available on a seller's own listing are View, Mark Sold, and Delete. There is no UI path or route to modify an existing product's information after creation.

- [ ] FT-4: The shopping cart cannot contain items that have been removed or are no longer available.
  - Bug Report:
    - Issue: Cart does not filter out or remove items that become unavailable (marked sold)
    - Actual: Source inspection of AppContext.tsx shows the "Mark Sold" action only calls updateProduct() to change a product's status field; it never calls removeFromCart(). Cart.tsx renders all items in the cart array unconditionally with no availability/status check. Live test: added a still-Active item (Sony Headphones) to Emma's cart, then logged out and back in as Emma — the item disappeared from the cart, but this was proven (via source) to be solely because logout() unconditionally calls setCart([]), not because of any availability-based filtering. Only deleteProduct() (full removal of a listing) cleans matching cart entries; marking an item "sold" while it remains in a buyer's active session cart does not remove it, so the constraint is not actually enforced.

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
    - Actual: Re-submitted the registration form with the same email (qa.tester.rv@example.com) already used moments earlier. The app accepted it, showed "Account created successfully!" toast again, and redirected to home logged in, instead of rejecting the duplicate.

- [ ] CS-12: Unable to log in when user account password is incorrect.
  - Bug Report:
    - Issue: Incorrect password is accepted for login
    - Actual: Logged in with existing account emma@example.com using an incorrect password (definitelyWrongPassword123). App showed "Welcome back!" and authenticated the user as "Emma Green", redirecting to home. Login page itself notes demo accounts accept "any password", confirming password is not validated.

- [ ] CS-13: Sellers cannot update other people's product information.
  - Bug Report:
    - Issue: Cannot verify protection because the update feature itself does not exist
    - Actual: Since there is no "Edit"/update capability anywhere in the app (confirmed via UI inspection and source code review of App.tsx and MyListings.tsx), there is no way to attempt updating another seller's listing, so the constraint that sellers cannot edit others' listings cannot be demonstrated as a working, purpose-built protection — the underlying update feature required by FT-2/FT-3 is missing entirely.

- [X] CS-14: Users need to fill in the necessary fields to create product information.

- [X] CS-15: Prices must be non-negative numbers.


## Interaction
- [ ] IX-16: Creating duplicate accounts provides visual feedback.
  - Bug Report:
    - Issue: No error/duplicate feedback shown; success feedback shown instead
    - Actual: Registering with an already-used email produced the same "Account created successfully!" toast as a fresh registration, with no indication that the account already existed.

- [X] IX-17: Search results are displayed in real time when users use the search function.

- [X] IX-18: The filtering results are displayed in real time when the user uses the filtering function.

- [X] IX-19: Provide visual feedback when users add items to their cart.


## Content
- [X] CT-20: All product photos displayed must be related to the used or upgraded products described in the product details.