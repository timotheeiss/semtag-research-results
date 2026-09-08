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
    - Actual: The Modify Reservation dialog contains only "New Time" and "Party Size" controls (dialog text: "Modify Reservation / New Time / Party Size / Save Changes / Close") — no date picker or date field. Time 19:00→20:30 and party 2→4 saved and displayed correctly, but the date (August 29, 2026) cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation-based gating on review submission
    - Actual: On /restaurant/1 the "Write a Review" tab was fully open and a review (5 stars, "QA Reviewer", visit 2026-08-20) was accepted with toast "Review Submitted!" even though the user had no completed reservation there (the only reservation was a future, confirmed booking). No completed-reservation requirement or one-review-per-reservation limit is enforced anywhere.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Weekend and first-reservation coupon eligibility rules not enforced
    - Actual: On /restaurant/4 with date Sep 2, 2026 (a Wednesday), WEEKEND20 ("20% off weekend dining") was accepted with "Coupon Applied!". WELCOME10 ("10% off your first reservation") was also accepted after the user already had reservations (1 cancelled + 1 confirmed booking already stored), i.e. on a third booking attempt. Only the GROUP15 party-size rule is enforced ("This coupon requires a party of 6 or more."); the single-coupon limit is respected (one coupon slot).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review-count inconsistent between restaurant list and detail; list summary not updated by new reviews
    - Actual: The Golden Fork shows 4.8 on the list card (no review count) but 4.5 (2) on its detail page before review submission. After submitting a 5-star review the detail updated to 4.7 (3) while the list card still shows 4.8 (static seed value, and data reviewCount is 312 vs 3 displayed).