# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Sellers cannot attach their own product photos; only canned stock images can be added
    - Actual: The create-listing form has no file input at all (document.querySelectorAll('input[type=file]').length === 0). The photo control is labelled "Click to add sample images (up to 4)" and each "Add Photo" click appends a fixed Unsplash stock URL (https://images.unsplash.com/photo-1555041469-a586c61ea9bc). A seller therefore cannot attach a photo of the actual item. All other fields worked: listing "Restored Oak Writing Desk" saved and displays $240, Furniture, Good, Brooklyn NY, Upcycled badge, description, seller Tim Tester, Listed 2026-08-28.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Listing editing is not implemented — no edit affordance exists
    - Actual: On /my-listings the only per-listing controls are View, Mark Sold, and Delete. The word "edit" appears nowhere in the page (/edit/i test on body text = false), and the owner's product detail page offers only a "Manage Your Listings" link. There is no way for a seller to change title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [ ] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.
  - Bug Report:
    - Issue: No maximum-price filter; the price control is a single-thumb slider that only sets a minimum
    - Actual: The "Price Range" control renders one thumb only (aria-valuenow=0, min=0, max=500) and its label is always "$X - $500": moving it changed the label $0->$10->$130 and filtered to items priced >= $130 (prod-10 $320, prod-3 $175, prod-4 $180). The $500 upper bound cannot be lowered, so a buyer cannot set a maximum price. All other criteria worked correctly: Furniture -> 2 furniture items; upcycled + Like New -> only prod-8; distance <=10mi -> exactly the 6 items with distance 4-9, excluding prod-2 (15mi) and prod-6 (12mi); sorts newest/price-low ($35..$320)/price-high ($320..$35)/distance (4,5,6,7,8,9) all ordered correctly.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category cards on the /categories page do not apply the chosen category to browse results
    - Actual: The /categories page correctly advertises "Furniture 2 items", but clicking it navigates to /?category=furniture and the browse section still lists all 8 available items across every category (sports, clothing, home-decor, electronics, toys, books) with the count reading "8 items available" and the Furniture filter checkbox still unchecked. The ?category= query parameter is ignored. By contrast the home page's own Furniture tile correctly filters to "2 items available", so the defect is specific to the category-browsing page.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: No seller-facing view of purchase requests, and cart requests are reported as sent without any request being recorded
    - Actual: (a) Emma sent a request on Marcus's prod-2 (stored in app state as req-1787907223118 with the buyer message). Signing in as Marcus, no surface shows it: My Listings lists only his items, the owner product page shows only listing data, and the "My Profile" menu item leads to a 404 "Oops! Page not found". The message text never appears (body.innerText.includes('QA-MARKER-77') === false on every seller screen). (b) As Marcus, adding prod-1 and prod-5 to the cart and clicking "Send Purchase Request" emptied the cart and toasted "Purchase requests sent to sellers!", yet the in-memory requests array still held only the single pre-existing prod-2 request — zero requests were created for the two cart items.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Password is never verified; any password authenticates a registered account
    - Actual: Signed in as emma@example.com with the deliberately wrong password "totally-wrong-password-xyz". App redirected to / and the header changed to the authenticated state (Sell link, no Log in/Sign up), proving an authenticated session was created despite an incorrect password.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Guarded pages render a blank white screen instead of a sign-in prompt or redirect
    - Actual: Two of the four guarded surfaces work: Add to Cart while signed out redirects to /login, and the Message Seller send button is disabled and labelled "Log in to message". But visiting /cart or /create-listing while signed out leaves the URL unchanged and renders a completely empty document (document.body.innerText.trim().length === 0) — no sign-in prompt, no redirect, no content. Console shows React Router's "You should call navigate() in a React.useEffect(), not when your component is first rendered", indicating a broken redirect guard.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.