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
    - Issue: Date cannot be modified on an existing reservation
    - Actual: Modify Reservation dialog contains only "New Time" and "Party Size" controls (dialog text: "Modify Reservation / New Time / 19:00 / Party Size / 2 Guests / Save Changes / Close") — no date field. Time 19:00→20:00 and party 2→4 saved and displayed correctly, but the reservation date cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation gating on review submission
    - Actual: With zero reservations in the account (Upcoming (0) / Past (0)), the "Write a Review" form on The Golden Fork accepted and saved a 5-star review; no completed-reservation check or one-review-per-reservation limit was enforced.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility conditions not enforced (day rule and first-reservation rule)
    - Actual: WEEKEND20 was accepted ("- 20% off") for Aug 31, 2026, a Monday. WELCOME10 was accepted again on a second reservation even though the account already had a confirmed reservation (first-reservation rule). Only the party-size rule worked (GROUP15 rejected at 2 guests, accepted "- 15% off" at 6 guests), and the one-coupon-at-a-time limit holds (input replaced by applied coupon + remove).

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: No validation feedback for missing/invalid reservation data
    - Actual: Submitting with all fields empty, and later with date+time set but missing name/email/phone, and later with name/phone filled but email "not-an-email", produced no message, toast, aria-live text, or inline field error (no [aria-invalid]/error nodes in the form). Reservation is silently not created (localStorage tablespot_reservations stays []), so the user is never told what is missing or invalid.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between restaurant list and detail; list rating not derived from reviews and not updated after review submission
    - Actual: Restaurant 1 list card showed rating 4.8 while its detail page showed 4.5 with 2 reviews (avg of 5 and 4 = 4.5). After submitting a 5-star review, detail updated to 4.7 (3 reviews) but the list card still shows 4.8; list cards also show no review count.