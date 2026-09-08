# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own photos — no file upload exists
    - Actual: The Create Listing form's photo control is a button "Add Photo" with helper text "Click to add sample images (up to 4)"; it inserts hard-coded Unsplash sample URLs (e.g. images.unsplash.com/photo-1555041469...). There is no input[type=file] anywhere in the form, so a seller can never attach a photo of their actual item. All other submitted data (title, description, $48, Furniture, Good, Austin TX, Upcycled) was retained and displayed correctly on /product/prod-1787827574220.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: On /my-listings the only actions for an owned listing are View, Mark Sold and Delete (button list: New Listing, View, Mark Sold, Delete). The owned product page offers only "Manage Your Listings". The account menu contains My Profile / My Listings / Log out (My Profile leads to a 404). There is no UI anywhere to change a listing's title, description, photos, price, category, condition, location or upcycled status.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter — price range slider exposes only a minimum thumb
    - Actual: The "Price Range" control renders a single Radix thumb with aria-label="Minimum" (min 0, max 500); moving it to 250 changed the label to "$250 - $500" and returned only items priced >= $250, so the upper bound is permanently fixed at $500 and a buyer cannot cap the price. All other criteria worked: Furniture+condition New+Upcycled-only → exactly prod-10 and prod-3; Max Distance 1 mi → 0 items; sorts verified against underlying data (Newest = dates 11-23…11-10 desc, Price Low→High = 35,45,55,65,95,125,175,180,285,320, Distance = 2.5,3,4,5,6,7,8,9,12,15 asc).

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection does not filter the browse results
    - Actual: Clicking "Home Decor" (1 item) on /categories navigated to /?category=home-decor but the browse page still listed all 9 available items (prod-10, prod-3, prod-1, prod-7, prod-2, prod-8, prod-4, prod-9, prod-6) and reported "9 items available"; no category checkbox was checked. Re-checked after 1.2 s — unchanged. Expected only the Home Decor listing (prod-8) and a count of 1.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller view for purchase requests, and cart "requests sent" confirmation records nothing
    - Actual: As Emma Green (seller of prod-1, which had received both a Message-Seller request and a cart purchase request from Buyer Test) there is nowhere to see the requested item or buyer message: /my-listings shows only View/Mark Sold/Delete per listing, the account menu has only My Profile (404)/My Listings/Log out, and App.tsx defines only the routes /, /product/:id, /login, /register, /create-listing, /my-listings, /cart, /about, /categories, *. Additionally Cart.tsx's Send Purchase Request handler only calls toast.success('Purchase requests sent to sellers!') and clearCart() — it never calls the context's sendPurchaseRequest, so the confirmation is shown although no request is recorded for any cart item.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never verified — any password authenticates a registered account
    - Actual: Account buyer.test@example.com was registered with password "buypass123". After logging out, signing in with "totallyWrongPassword" succeeded: toast "Welcome back!", redirect to /, header shows Sell + avatar "Buyer Test" (authenticated session created). The login page even advertises "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected routes render a blank page instead of a sign-in prompt or redirect
    - Actual: While signed out, /create-listing, /cart and /my-listings each render an empty document (document.body.innerText === "", #root contains only the toast container) and stay on that URL — no sign-in prompt, no redirect, no explanation. In-app entry points do behave correctly: the Sell button is hidden when signed out, "Add to Cart" on a product redirects to /login, and "Message Seller" opens a dialog with a "Log in to message" button. So the actions are blocked, but direct/unauthenticated access to the protected pages leaves the user on a blank screen.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.