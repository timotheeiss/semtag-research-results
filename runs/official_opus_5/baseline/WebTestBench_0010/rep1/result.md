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
    - Actual: The "Modify Reservation" dialog only offers "New Time" and "Party Size" controls — no date picker/field. Time 19:00→20:30 and party 2→4 saved and displayed correctly, but the date (September 1, 2026) cannot be changed anywhere in the modify flow.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission not gated by a completed reservation
    - Actual: At Trattoria Bella, where the user had no reservation at all (only a cancelled Golden Fork booking and an upcoming Sakura Garden booking, no completed ones), the "Write a Review" tab was fully open with no eligibility notice and the review was accepted ("Review Submitted!"). The form can be reused repeatedly, so there is no one-review-per-completed-reservation limit either.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules not enforced (day rule and first-reservation rule)
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for Wednesday Sep 2, 2026 and stored on that reservation (localStorage couponCode: "WEEKEND20"). WELCOME10 ("10% off your first reservation") was accepted again at Trattoria Bella while the user already had a confirmed Sakura Garden reservation plus an earlier reservation that already used WELCOME10. Only the GROUP15 party-size rule was enforced ("This coupon requires a party of 6 or more."). Single-coupon-per-reservation is enforced (input replaced by applied chip).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating inconsistent with detail rating and not updated by new reviews
    - Actual: List card shows Trattoria Bella 4.5 while its detail header shows 4.0 (2 reviews); same mismatch for The Golden Fork (list 4.8 vs detail 4.5 (2)) and Sakura Garden (list 4.6 vs detail 5.0 (1)). After submitting a 4-star review for Trattoria Bella the detail header updated 4.0 (1)→4.0 (2) and the tab to "Reviews (2)", but the list card rating stayed at the static 4.5.