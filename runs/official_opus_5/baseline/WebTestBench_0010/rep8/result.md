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
    - Actual: The "Modify Reservation" dialog contains only "New Time" and "Party Size" controls (dialog text: "Modify Reservation / New Time 19:00 / Party Size 2 Guests / Save Changes / Close") — no date field at all. Time 19:00->20:30 and party 2->5 saved and displayed correctly, but the date (September 1, 2026) is unchangeable.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews are not gated by a completed reservation
    - Actual: On Trattoria Bella (restaurant 3), where the user has no reservation of any kind, the "Write a Review" tab was fully enabled and a 5-star review was accepted: toast "Review Submitted!", tab changed to Reviews (2), and the review persisted (stored under restaurantId 3). The earlier Sakura Garden review was also accepted against only an upcoming (not completed) reservation, and no reservation-to-review linkage exists, so the one-review-per-completed-reservation limit is not enforced either.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (weekend-day and first-reservation) are not enforced
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for a Tuesday reservation (Sep 1, 2026) -> toast "Coupon Applied! 20% off weekend dining". WELCOME10 ("10% off your first reservation") was accepted on a second booking made after an existing confirmed reservation already existed -> toast "Coupon Applied! 10% off your first reservation". Only the party-size rule works (GROUP15 with 2 guests rejected: "This coupon requires a party of 6 or more"; accepted at 6 guests). One-coupon-max is enforced (input replaced by badge + remove button once applied).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review-count inconsistent between list and detail; list rating not derived from reviews and never updates
    - Actual: The Golden Fork: list card shows 4.8 (no review count) while its detail page shows 4.5 (2). Sakura Garden: list shows 4.6, detail showed 5.0 (1); after submitting a 4-star review the detail updated to 4.5 (2) and tab "Reviews (2)", but the list card still shows 4.6 — unchanged and inconsistent.