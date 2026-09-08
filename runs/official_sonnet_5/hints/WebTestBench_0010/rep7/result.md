# Test Result

## Functionality
- [X] FT-1: Users can browse restaurants and view each restaurant's name, cuisine, average price per diner, rating, location, description, and notable dining features.

- [X] FT-2: Users can search restaurants by name, cuisine, or neighborhood and can combine the search with cuisine and price-range filters; every displayed restaurant satisfies all active criteria.

- [X] FT-3: Users can create a reservation by selecting a future date, an available time, and a party size, providing the required contact information, and submitting it; a successful booking is confirmed and appears among upcoming reservations with the supplied details.

- [X] FT-4: Users can view their upcoming reservations and cancel one after confirming the action; the cancelled reservation is removed from upcoming reservations and retained with a cancelled status in reservation history.

- [X] FT-5: Users can submit a one-to-five-star rating, visit date, name, and written review for a restaurant, and the new review is retained and displayed only with that restaurant.

- [X] FT-6: During reservation, users can apply a supported coupon, see the offered discount, remove it if desired, and have the applied coupon recorded with the resulting reservation.

- [X] FT-7: Past dates are unavailable for new reservations, while a valid future date can be combined with one of the restaurant's available reservation times.

- [ ] FT-8: Users can change the date, time, and party size of an upcoming reservation, and the reservation list retains and displays all submitted changes.
  - Bug Report:
    - Issue: Modify dialog does not support changing the reservation date
    - Actual: Opening 'Modify' on an upcoming reservation shows a dialog with only 'New Time' and 'Party Size' fields; there is no date field/control. Changed time to 19:30 and party size to 3 Guests, saved, and both changes were correctly reflected in the reservation list immediately - but the date (Aug 28, 2026) could not be changed at all since the dialog lacks a date picker, so users cannot change the date of an upcoming reservation as required.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted with no reservation-based authorization at all
    - Actual: Submitted a full review (rating, name, visit date, comment) for Trattoria Bella despite never having made or completed any reservation there (or anywhere else with a completed status) in this session. The review was accepted and published with no check for a completed reservation at that restaurant, and no limit mechanism was ever presented (e.g. no reservation was consumed/linked). This violates the requirement that a review requires an authorizing completed reservation and that each completed reservation authorizes at most one review.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: First-reservation coupon eligibility rule not enforced
    - Actual: Coupon GROUP15 correctly rejected for a party size below 6 ('This coupon requires a party of 6 or more'), and only one coupon can be applied at a time (UI hides input once one is applied, replacing it with a badge+remove control). However, WELCOME10 (marketed as '10% off your first reservation') was accepted and applied again for a second reservation made by the same guest (name Timothee Issenmann, email timothee.issenmann@gmail.com) who already had a confirmed prior reservation with WELCOME10 applied. The coupon should have been rejected as this was not the guest's first reservation, but it was applied and the second reservation was confirmed with WELCOME10 discount again.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: No missing/invalid field is identified to the user
    - Actual: Submitting the reservation form completely empty, and separately submitting with an invalid email format, both silently failed to create a reservation (0 upcoming/past reservations after), but no toast notification, inline error text, or aria-invalid/red-border styling appeared anywhere on the form to tell the user what is missing or wrong.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review count inconsistent between restaurant list and detail view after new review
    - Actual: Before submitting a review, Trattoria Bella showed rating 4.5 on both the home list and its detail page. After submitting a new 4-star review on the detail page, the detail page updated to rating 4.0 (review count still incorrectly showed (2) instead of (3), suggesting a review may have been dropped), but the home page restaurant list still displayed the stale rating of 4.5 for Trattoria Bella - the two views are inconsistent after a review submission.