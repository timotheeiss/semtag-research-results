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
    - Issue: Date cannot be modified for an existing reservation
    - Actual: The 'Modify Reservation' dialog only offers 'New Time' and 'Party Size' selectors; there is no date field/control. Changing time (19:00→18:00) and party size (4→2 Guests) worked and persisted correctly in the reservation list, but the date of an upcoming reservation cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted without any completed reservation, and reservation-to-review authorization is not enforced
    - Actual: Successfully submitted a review for 'Spice Route' restaurant despite never having made any reservation there at all (Reviews count went from 0 to 1 review immediately). Also submitted a review for 'Ember & Smoke' where the only reservation was Cancelled (not completed). No restriction based on a completed reservation, and no limit of one review per completed reservation, is enforced anywhere in the review submission flow.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (first-reservation, day) are not enforced
    - Actual: WELCOME10 ('10% off your first reservation') was accepted for a second reservation made in the same session after an earlier reservation already existed, violating the first-reservation rule. WEEKEND20 ('20% off weekend dining') was accepted for Aug 27, 2026, which is a Thursday (a weekday, not a weekend), violating the day-of-week eligibility rule. Only the GROUP15 party-size rule (6+ guests) was correctly enforced, rejecting it for a 2-guest reservation with the message 'This coupon requires a party of 6 or more.'

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid field information is not identified to the user
    - Actual: Submitting the reservation form with all fields empty, or with an invalid email (e.g. 'bademail'), silently fails to create a reservation (confirmed no reservation appears in My Reservations) but shows no error text, toast, aria-invalid attribute, or visual indication of which field(s) are missing/invalid. Only focus moves to the Name field in the empty-form case.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is inconsistent with detail page rating and does not update after new reviews
    - Actual: Home page list shows a static seed rating per restaurant (e.g. The Golden Fork 4.8) which does not match the rating computed/shown on that restaurant's own detail page (4.5, derived from its 2 reviews) even before any new review was added. After submitting new reviews for Ember & Smoke (rating changed 5.0→4.5, count 1→2 on detail page) and Spice Route (rating became 3.0, count 1 on detail page), the home page restaurant list still displayed the original unchanged ratings (Ember & Smoke 4.7, Spice Route 4.4), showing the list summary is never refreshed from actual review data.