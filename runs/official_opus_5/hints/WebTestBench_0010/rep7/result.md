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
    - Actual: The Modify Reservation dialog contains only "New Time" and "Party Size" (dialog text: "Modify Reservation / New Time / Party Size / Save Changes / Close") — there is no date control. Time (19:00→20:30) and party size (2→4) saved and are displayed correctly, but the reservation date (September 1, 2026) cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: With zero past/completed reservations (Past tab showed "Past (0)"), the "Write a Review" form at Trattoria Bella was fully open and a 4-star review by "QA Tester" was accepted ("Review Submitted! Thank you for sharing your experience.") and persisted. No eligibility check, and no per-reservation one-review limit exists since reviews are not linked to reservations at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Weekend-day and first-reservation coupon eligibility rules are not enforced
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for a reservation dated Tue Sep 1, 2026 (toast "Coupon Applied! 20% off weekend dining"). WELCOME10 ("10% off your first reservation") was accepted on a second reservation at Trattoria Bella while a confirmed reservation (res_1787879580929, The Golden Fork) already existed. Party-size rule (GROUP15 rejected at 2 guests, accepted at 6), invalid-code rejection and the one-coupon-at-a-time limit (code input disappears once a coupon is applied) do work.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid required-field information is never identified to the user
    - Actual: Clicking "Confirm Reservation" with an empty form, and again with date+time+name+phone but email "invalid-email", silently does nothing: no inline error, no toast (MutationObserver captured zero added nodes), no field highlight. Submission is blocked (localStorage tablespot_reservations stays []), but the user gets no indication of what is missing or invalid.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating shown in restaurant list is inconsistent with the detail page and is not updated by new reviews
    - Actual: The Golden Fork: list card shows 4.8, detail shows 4.5 (2) — computed from its 2 reviews (5,4). Trattoria Bella: list card shows 4.5 both before and after submitting a 4-star review, while its detail rating changed from 4.0 (1 review) to 4.0 (2) — the list rating is a static value and never reflects reviews. The list also shows no review count.