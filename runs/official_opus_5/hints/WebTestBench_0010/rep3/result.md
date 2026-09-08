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
    - Actual: The Modify Reservation dialog contains only "New Time" and "Party Size" (dialog text: "Modify Reservation / New Time / 19:30 / Party Size / 2 Guests / Save Changes / Close") — there is no date control, so the date of an upcoming reservation cannot be changed. Time 19:30→20:30 and party size 2→5 did save and display ("August 31, 2026 / 20:30 / 5 Guests"), but the date stayed August 31, 2026 with no way to alter it.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review authorization constraint not enforced
    - Actual: With zero reservations of any kind (Upcoming (0) / Past (0)), the "Write a Review" form for The Golden Fork was fully available and the submitted review was accepted: banner "Review Submitted!" appeared and the restaurant rating/review count changed from 4.5 (2) to 4.3 (3). No completed-reservation check and no per-reservation review limit exist.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules not enforced (day rule, first-reservation rule, stale eligibility)
    - Actual: 1) WEEKEND20 ("20% off weekend dining") was accepted for Monday Aug 31, 2026. 2) WELCOME10 ("10% off your first reservation") was accepted on a second booking while an existing reservation was already in Upcoming (1). 3) GROUP15 was applied with 6 guests, then party size was lowered to 2 — the coupon stayed applied, submission succeeded and the reservation card records "Coupon applied: GROUP15" despite the 6+ rule. Only the initial party-size check ("This coupon requires a party of 6 or more.") and the single-coupon-at-a-time UI behaved correctly.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and detail; list summary not updated by new reviews
    - Actual: The Golden Fork: list card shows 4.8 while detail showed 4.5 (2 reviews) before and 4.3 (3 reviews) after submitting a 4-star review — the list rating stayed 4.8 in both cases. Sakura Garden: list 4.6 vs detail 5.0 (1 review). List cards also show no review count.