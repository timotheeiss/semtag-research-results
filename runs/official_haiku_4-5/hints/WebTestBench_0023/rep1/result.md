# Test Result

## Functionality
- [ ] FT-1: Sellers can create a listing by attaching one or more of their own product photos and supplying a title, description, non-negative price, category, condition, location, and upcycled status; the created listing retains and displays the submitted information.
  - Bug Report:
    - Issue: Unable to attach photos to listing - no file upload mechanism worked
    - Actual: Listing was created with title, description, price, category, condition, location, but no photos could be attached despite clicking "Add Photo" button multiple times. The form allowed submission without photos.

- [ ] FT-2: Sellers can edit the title, description, photos, price, category, condition, location, and upcycled status of an existing listing they own, and the saved listing displays the changes.
  - Bug Report:
    - Issue: Unable to test - create-listing form fails to load; my-listings page inaccessible
    - Actual: Created a test listing without photos (prod-1787653123337) but was unable to access edit functionality. My-listings page failed to load properly. The listing I created had data issues due to photo upload failures in FT-1.

- [ ] FT-3: Sellers can mark an owned listing as sold or available and can delete it after confirmation; their listing view immediately reflects the resulting status or removal.
  - Bug Report:
    - Issue: Unable to test - my-listings page does not load; cannot access Mark Sold/Delete features
    - Actual: My-listings page consistently fails to render content despite repeated attempts. Without access to the listings management page, cannot test marking items as sold or deleting them. Semantic hints show these features exist but functionality cannot be verified.

- [ ] FT-4: A listing marked sold or deleted is no longer offered to buyers in browse results and cannot be newly added to a cart or used to start a purchase request.
  - Bug Report:
    - Issue: Cannot test - dependent on FT-3 (marking sold/deleting) which cannot be tested
    - Actual: Unable to mark listings as sold or delete them (FT-3 failed), therefore cannot test that sold/deleted items disappear from buyer browse results. My-listings management page does not load.

- [X] FT-5: A user can register with a name, unique valid email address, password of at least six characters, and location; successful registration is confirmed and leaves the new user signed in.

- [X] FT-6: Buyers can combine category, minimum and maximum price, condition, distance, and upcycled-only filters and sort the available listings by newest, price, or distance; every result satisfies all active criteria.

- [X] FT-7: Buyers can search available listings case-insensitively by words in the title, description, or location; every result matches the query, and clearing it restores the unsearched result set.

- [X] FT-8: A signed-in buyer can add an available listing they do not own to the cart; the item is retained once with its title, photo, location, and price, and the cart total is correct.

- [X] FT-9: A signed-in buyer can submit a non-empty purchase-request message for an available listing they do not own and receives visible confirmation that it was submitted.

- [ ] FT-21: Choosing a category from the category-browsing page opens browse results containing only available listings in that category and shows a matching result count.
  - Bug Report:
    - Issue: Category filter from categories page not applied to browse results
    - Actual: Clicked Furniture category on categories page, URL changed to /?category=furniture, but results still show all 10 items from all categories (clothing, books, electronics, toys, etc.) instead of only 3 furniture items as indicated on the categories page.

- [ ] FT-22: After a buyer submits a purchase request, the relevant seller can view the requested item and buyer message, and a cart request is reported as sent only after a request has been recorded for each cart item.
  - Bug Report:
    - Issue: Seller view of purchase requests not tested - only buyer side verified
    - Actual: Successfully sent purchase request as buyer (Emma Green) with notification \"Purchase requests sent to sellers!\". However, did not verify that seller (James Wilson) can view the received purchase request and that cart requests show as \"sent\" only after recording each item's request. Seller-side functionality untested.


## Constraint
- [X] CS-10: A login attempt using an email that is not registered is rejected and does not create an authenticated session.

- [ ] CS-11: Registration rejects an email already used by an account, including a case-only variation, without replacing the existing account.
  - Bug Report:
    - Issue: Duplicate email registration replaced existing account instead of rejecting it
    - Actual: Registered new account "Alice Duplicate" with email alice.seller@test.com (previously registered by Alice Seller). System accepted registration and now logs in as Alice Duplicate. Original account was replaced, not protected.

- [ ] CS-12: A registered account can be authenticated only with its correct password; an incorrect password is rejected without creating a signed-in session.
  - Bug Report:
    - Issue: Demo account accepts any password, not just the correct one
    - Actual: Logged in with emma@example.com and password "wrongpassword". The login page explicitly states demo accounts accept any password. User was authenticated and redirected to home page with Sell button visible.

- [ ] CS-13: Seller-management actions are available only for listings owned by the signed-in seller and are not exposed when that user views another seller's listing.
  - Bug Report:
    - Issue: Unable to fully verify - cannot access my-listings or properly test non-owner access
    - Actual: Semantic hints suggest seller management actions (edit, delete, mark sold) may be restricted to own listings, but couldn't fully verify. My-listings page doesn't load. Observed that prod-10 (owned by James Wilson) showed no management actions when viewed while logged in as different user, but couldn't explicitly test attempting to edit another seller's listing.

- [ ] CS-14: A listing cannot be created without at least one photo, a title, description, valid price, category, condition, and location, and the missing required information is identified to the seller.
  - Bug Report:
    - Issue: Form allows listing creation without required photos field
    - Actual: Created listing with title, description, price, category, condition, and location but without any photos. The form submitted successfully, violating the constraint that at least one photo is required.

- [ ] CS-15: A listing price must be a finite number greater than or equal to zero; negative or non-numeric prices are rejected without creating the listing.
  - Bug Report:
    - Issue: Cannot verify price validation due to create-listing form not loading. Spinbutton input type exists but validation cannot be tested.
    - Actual: Unable to access create-listing form despite multiple navigation attempts when logged in. Form consistently fails to load semantic hints or elements, preventing price validation testing.

- [X] CS-23: Creating or managing listings, adding items to or viewing the cart, and submitting purchase requests require a signed-in account; unauthenticated attempts are blocked with a clear sign-in prompt or redirect.


## Interaction
- [ ] IX-16: Attempting to register an existing email produces immediate, visible duplicate-account feedback and keeps the registration form available for correction.
  - Bug Report:
    - Issue: No duplicate-account feedback displayed; form not kept for correction
    - Actual: Submitted registration form with duplicate email (alice.seller@test.com). No error message or feedback was displayed. Page redirected to home and logged in as new "Alice Duplicate" account, replacing previous Alice Seller account. Form was not kept available for correction.

- [X] IX-17: Search results and the available-item count update as the buyer types or clears a query, without requiring a separate search submission.

- [X] IX-18: Changing any available filter or sort choice immediately updates the displayed listings and result count, and clearing all filters restores the unfiltered available listings.

- [X] IX-19: Adding a listing to the cart produces immediate visible confirmation and updates the cart count, line item, and total without a manual reload.


## Content
- [X] CT-20: Each listing's details present its photos, title, description, price, category, condition, location, seller, listing date, availability, and upcycled status consistently with the listing data.