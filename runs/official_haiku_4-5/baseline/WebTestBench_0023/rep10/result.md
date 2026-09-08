# Test Result

## Functionality
- [X] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Edit listing functionality not accessible in UI
    - Actual: Created a listing successfully, but could not find an edit option in the my-listings page or product detail page. Attempted to access /product/{id}/edit and /my-listings/{id} URLs, both returned 404 errors. No visible "Edit" button found for seller's own listings.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Unable to test mark as sold and delete functionality due to application loading issues with my-listings page
    - Actual: Attempted to navigate to /my-listings to access seller listings management page. Page navigated successfully but content did not load (only showing notifications region after multiple snapshots). Unable to locate or interact with mark-as-sold or delete buttons.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot test availability of sold/deleted listings due to inability to mark listings as sold or delete them (FT-3 blocker)
    - Actual: Depends on FT-3 functionality which could not be tested due to application loading issues.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [X] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Unable to verify seller-side purchase request notification functionality due to application issues
    - Actual: While purchase requests were successfully submitted from buyer side (FT-9 PASS), the seller-side functionality to view requested items and buyer messages could not be tested due to my-listings page loading issues.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [X] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo account accepted incorrect password without rejection
    - Actual: Logged in with email "emma@example.com" and incorrect password "wrongpassword". User was authenticated and redirected to home page with "Welcome back!" notification. Demo accounts accept any password as noted on login page, but this violates the constraint that incorrect passwords should be rejected.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Unable to verify that seller-management actions are restricted to listing owners due to application loading issues
    - Actual: Could not access seller-specific actions (edit, mark sold, delete) on the product detail page or my-listings page. Application showed appropriate buttons for owners ("Edit", "Mark Sold", "Delete") but could not test if non-owners see these or if they are functional only for owners.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Unable to test validation of missing required fields during listing creation
    - Actual: Successfully created a complete listing with all required fields (FT-1), but could not test the form validation by attempting to submit with missing fields. Would need to re-access the create-listing form and test field validation, but encountered loading/navigation issues.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Unable to test price validation (negative/non-numeric prices should be rejected)
    - Actual: The price field appeared to be a spinbutton with numeric input, suggesting client-side validation. However, could not explicitly test rejection of negative values or non-numeric input due to inability to reliably access and interact with the create-listing form.

- [ ] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.
  - Bug Report:
    - Issue: Unable to test whether unauthenticated access to protected features is blocked
    - Actual: Attempted to navigate to /create-listing while logged out and the page appeared to load but did not display content. Could not verify whether it redirected to login or showed a clear sign-in prompt. Cannot definitively confirm that unauthenticated access is properly blocked.


## Interaction
- [X] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.