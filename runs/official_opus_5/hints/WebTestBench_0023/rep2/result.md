# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: No way to attach the seller's own photos — the photo control only inserts canned stock images
    - Actual: The Photos section offers only an "Add Photo" button labelled "Click to add sample images (up to 4)"; the page contains no input[type=file] and clicking it appended a random Unsplash stock URL (https://images.unsplash.com/photo-1555041469-...). All other submitted data was retained: the created listing "Restored Oak Dining Chair" shows $75, Furniture, Good, Brooklyn NY, Upcycled badge, the description, seller Test Buyer and Listed 2026-08-27.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: On /my-listings the owned listing "Restored Oak Dining Chair" exposes only View, Mark Sold and Delete buttons; the owner's product detail page exposes only "Manage Your Listings". There is no edit affordance anywhere, so title, description, photos, price, category, condition, location and upcycled status of an existing listing cannot be changed.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter: the "Price Range" control is a single-thumb slider that only sets a minimum
    - Actual: Price Range slider renders one thumb (aria-valuemin 0, aria-valuemax 500, one [role=slider] node). Moving it to 250 produced label "$250 - $500" and filtered to items priced >= $250 only; the upper bound stays fixed at $500 and cannot be changed, so a buyer cannot cap the price. Other criteria worked: category Furniture -> 3 items; +condition New -> prod-10/prod-3; +min price 250 -> prod-10 only; upcycled-only -> exactly the 5 upcycled items; max distance 2 mi -> 0 items, 26 mi -> all 10; sort price-low ordered $35..$320 and sort distance ordered Brooklyn(2.5mi) -> Austin -> Portland -> San Francisco.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the Categories page does not filter browse results
    - Actual: Clicking "Home Decor 2 items" on /categories navigated to /?category=home-decor, but the browse section still showed all 10 listings and the count "10 items available"; no category checkbox was checked (no toggle had aria-checked=true). Expected only the 2 Home Decor listings with a matching count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view received purchase requests or buyer messages
    - Actual: Marcus sent a purchase-request message on Emma's listing prod-1 ("Marcus here - I would like to buy the armchair this weekend...") and got "Message sent to seller!". Signed in as Emma, /my-listings shows only her two listings with View/Mark Sold/Delete and no requests/messages section; the owner product page shows only listing data plus "Manage Your Listings"; the user menu's "My Profile" route (/profile) renders "404 Oops! Page not found". No inbox or request list exists anywhere in the app.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never verified; any password authenticates a registered account
    - Actual: Account testbuyer@example.com was registered with password "buyerpass1". Logging in with "totallyWrongPass" succeeded: redirected from /login to / and the header switched to the signed-in state (Sell / cart / user menu). The login page itself states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank screen instead of a sign-in prompt or redirect
    - Actual: Good: logged-out "Add to Cart" redirected to /login, and the message dialog's send button was disabled with the label "Log in to message". Bad: opening /cart or /create-listing while signed out (both by in-app navigation and by direct load) rendered an empty page — document.body.innerText was "" with no header, no message and no redirect (URL stayed /cart and /create-listing), so the unauthenticated user gets no sign-in prompt or redirect at all.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.