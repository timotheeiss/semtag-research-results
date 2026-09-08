# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Missing edit-listing functionality
    - Actual: On the My Listings page, each owned listing only exposes "View", "Mark Sold", and "Delete" actions. There is no Edit control on the listing card, on the product detail page (only "Manage Your Listings" link is shown to the owner), and no element containing "edit" text exists anywhere in the DOM. Sellers have no way to modify title, description, photos, price, category, condition, location, or upcycled status of an existing listing.

- [X] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.

- [X] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category browsing page does not filter results
    - Actual: Clicking the "Furniture" category card (labeled "3 items") on the /categories page navigates to /?category=furniture, but the Browse page shows all 9 items available (unfiltered) and the "Furniture" checkbox in the sidebar filter panel is NOT checked. The category selection from the Categories page has no effect on the actual displayed/filtered results.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller has no way to view purchase requests or buyer messages
    - Actual: Signed in as Marcus Chen, added Emma Green's listing "Mid-Century Modern Armchair" (prod-1) to cart and submitted it via "Send Purchase Request" (toast "Purchase requests sent to sellers!" confirmed submission). Logged out and logged in as the seller, Emma Green. On My Listings, the "Mid-Century Modern Armchair" listing shows only status "Active" with View/Mark Sold/Delete actions — no indicator of a pending purchase request, no request count/badge, and no way to open or read the buyer's request. The header account menu (My Profile/My Listings/Log out) and the header icon buttons (search, cart) contain no "Requests" or "Messages"/inbox section anywhere in the app. There is no UI surface for a seller to view requested items or buyer messages.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Login accepts incorrect password
    - Actual: Registered account tim.seller.qa@example.com with password "secretpw". Logging in with the same email but incorrect password "wrongpassword" succeeded: toast "Welcome back!" appeared, user was redirected to home page, and header showed the authenticated "Tim Seller" account button — a signed-in session was created despite the wrong password.

- [X] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.

- [X] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.

- [X] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unauthenticated access to protected route renders blank page instead of redirecting/prompting login
    - Actual: While signed out, navigating to the /cart route directly renders a completely blank page (only the empty notification regions are present in the DOM — no header, no cart content, no login prompt, no redirect to /login). The browser console shows a React warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," confirming the app attempts an improper navigate() call to guard the route but fails to actually redirect or block access gracefully, leaving the user on a broken blank page instead of being redirected to sign in.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [ ] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.
  - Bug Report:
    - Issue: "Clear all filters" does not fully restore the unfiltered item list when a search term is active
    - Actual: Checkbox/slider filter changes do update the result list live/immediately (e.g., checking "Furniture" instantly narrowed 9→3 items; combined with search text "lamp" instantly narrowed to 0 items). However, clicking "Clear all filters" only resets the checkbox/slider filter controls — it does NOT clear the search box text. After searching "lamp" + checking "Furniture" (0 items/"No items found"), clicking "Clear all filters" unchecked Furniture but left "lamp" in the search box, leaving the view showing only 1 item (Vintage Brass Floor Lamp) instead of restoring the full 9-item unfiltered browse list.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [ ] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.
  - Bug Report:
    - Issue: Availability status is not consistently/explicitly displayed on listing detail pages
    - Actual: Core listing fields (title, price, category badge, condition badge, location with distance, "Listed" date, description, seller name/photo/location, Upcycled badge where applicable) are displayed consistently across all active listing detail pages checked (prod-10, prod-7, prod-5). However, none of these active listings show an explicit availability status label (e.g., "Available"/"Active") — status is only inferable indirectly from the presence of the "Add to Cart"/"Message Seller" buttons. By contrast, a sold listing displays an explicit "This item is no longer available" message with purchase controls removed. This is an inconsistent presentation of the availability status field: explicit for sold items, implicit/absent for active items.