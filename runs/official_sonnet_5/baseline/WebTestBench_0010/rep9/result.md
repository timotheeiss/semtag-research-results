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
    - Issue: Reservation date cannot be modified
    - Actual: The 'Modify Reservation' dialog only offers 'New Time' and 'Party Size' fields; there is no control to change the reservation date. Time (18:00→19:30) and party size (6→4 Guests) changes saved successfully and were reflected in the reservation list, but the date (Aug 27, 2026) could not be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission not gated by a completed reservation
    - Actual: A review was successfully submitted for The Golden Fork via the 'Write a Review' tab despite the only reservation for that restaurant having been cancelled (not completed), and with no prior reservation existing for that restaurant under the reviewer's name at all otherwise. The app accepted the review with no eligibility check tying it to a completed reservation, and no limit enforcement of one review per completed reservation was observable since no gating existed at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon day-eligibility rule not enforced
    - Actual: WEEKEND20 coupon (intended for weekend dining per its message '20% off weekend dining') was successfully applied when the selected reservation date was Aug 27, 2026, which is a Thursday (a weekday), not a weekend. GROUP15's party-size rule (6+ guests) and WELCOME10's first-reservation rule worked correctly, and the UI structurally prevents applying two coupons at once (input replaced by applied-coupon badge), but the day-based eligibility condition for WEEKEND20 was not validated.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review count inconsistent between list and detail, and list does not update after new review
    - Actual: Before any review submission, The Golden Fork showed rating 4.8 on the restaurant list page but 4.5 (2) on its detail page — already inconsistent. After submitting a new review, the detail page updated to 4.3 (3), but the list page still showed the stale 4.8, confirming the list-page summary is not derived from / synced with the review data.