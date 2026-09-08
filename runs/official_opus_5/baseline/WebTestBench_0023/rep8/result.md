# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos; only canned stock images can be added
    - Actual: The Create Listing form has no file input — the photo control is an "Add Photo" button labelled "Click to add sample images (up to 4)" that cycles through a hard-coded sampleImages array of Unsplash URLs (verified: CreateListing.tsx has no type="file" input). All other data is retained correctly: the created listing (prod-1787890215835) shows title "QA Test Upcycled Crate Shelf", the submitted description, $42.5, Furniture, Good, Seattle, WA, Upcycled badge, 2 photos, seller Emma Green, Listed 2026-08-28, with toast "Listing created successfully!".

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: No edit capability for existing listings
    - Actual: Signed in as Emma Green, My Listings shows each owned listing with only "View", "Mark Sold" and "Delete" buttons — no Edit control. The owner's product detail page shows only a "Manage Your Listings" button. The router has no edit route (only /create-listing for new listings), so title, description, photos, price, category, condition, location and upcycled status of an existing listing cannot be changed.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter; price control is a single-thumb minimum-only slider
    - Actual: Category, condition, distance, upcycled-only filters and all three sorts (Newest/Price/Distance) combine correctly and every result matched. However the "Price Range" control renders one Radix thumb with aria-label="Minimum" (aria-valuemin=0, aria-valuemax=500); the upper bound is fixed at $500 and cannot be changed, so a maximum price cannot be applied. Verified in DOM: the price slider group contains a single [role=slider] element.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category selection from the Categories page does not filter browse results
    - Actual: The /categories page correctly reported "Furniture 2 items". Clicking the Furniture tile navigated to /?category=furniture, but the browse page ignored the query parameter: it listed all 9 available listings (Clothing, Electronics, Books, Sports, Toys items included) with the count "9 items available", and the Furniture filter checkbox stayed unchecked. Expected only the 2 available Furniture listings and a count of 2.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Sellers have no way to view purchase requests or buyer messages
    - Actual: Marcus sent a direct message on Emma's prod-1 ("Message sent to seller!") and a cart purchase request for prod-1 + prod-5 ("Purchase requests sent to sellers!"). Signing in as Emma Green (the seller) there is no requests/messages view anywhere: My Listings only shows View / Mark Sold / Delete per listing with no request indicator, the "My Profile" menu item leads to a 404, and the app router exposes only /, /product/:id, /login, /register, /create-listing, /my-listings, /cart, /about, /categories, * — no inbox/requests page. The requested item and buyer message are therefore never visible to the seller.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is not verified; any password authenticates a registered account
    - Actual: Registered qa.buyer@example.com with password "qapass123", logged out, then signed in with "TotallyWrongPass". Login succeeded: toast "Welcome back!", redirected to /, header shows Sell + QA Buyer account menu. The login page even states "(any password works)".

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Protected pages render a blank screen when signed out instead of a sign-in prompt or redirect
    - Actual: Action-level gating works: Add to Cart while signed out redirects to /login with toast "Please log in to add items to your cart", and Message Seller opens a dialog whose only control is a disabled "Log in to message". But visiting the protected routes while unauthenticated renders nothing at all — /cart, /create-listing and /my-listings each return an empty document (document.body.innerText length 0, root div contains only the toast containers) with no header, no message, no sign-in prompt and no redirect. Also, the header cart icon does nothing when clicked signed out.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.