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
    - Actual: The "Modify Reservation" dialog only exposes "New Time" and "Party Size" — there is no date control. Time (19:00→20:30) and party size (2→5) saved and re-displayed correctly, but the date (August 29, 2026) cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation gating on review submission
    - Actual: On /restaurant/1 the "Write a Review" tab was fully available with zero reservations in the session. A 5-star review by "QA Tester" was accepted and published ("Review Submitted!" toast, review count 2→3), with no check for a completed reservation and no per-reservation review limit.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (weekend-day rule and first-reservation rule) not enforced
    - Actual: On /restaurant/3 with date Sep 2, 2026 (a Wednesday), WEEKEND20 was accepted: toast "Coupon Applied! 20% off weekend dining" and chip "WEEKEND20 - 20% off". Also, with one confirmed reservation already existing, WELCOME10 ("10% off your first reservation") was accepted on this second reservation. Only the party-size rule (GROUP15 rejected for party of 2: "This coupon requires a party of 6 or more") and the one-coupon-at-a-time rule (Apply input replaced by the applied-coupon chip) work.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [ ] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.
  - Bug Report:
    - Issue: Modification choices omit the reservation date
    - Actual: "Modify" opens a dialog exposing only "New Time" and "Party Size"; the date is not offered as a modification choice (saving time 19:00→20:30 and party 2→5 did work, with a "Reservation Updated" toast). Cancellation behaves correctly: it opens the "Cancel Reservation?" alert dialog, "Keep Reservation" leaves the booking untouched, and only "Cancel Reservation" applies the cancellation.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and details; list summary not updated by new review
    - Actual: Restaurant list card for "The Golden Fork" shows 4.8 and no review count, while its detail page showed 4.5 (2) computed from its 2 reviews. After submitting a 5-star review the detail page updated to 4.7 (3) but the list card still shows the hard-coded 4.8.