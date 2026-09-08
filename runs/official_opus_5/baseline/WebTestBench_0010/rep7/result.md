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
    - Issue: Reservation date cannot be changed
    - Actual: The "Modify Reservation" dialog exposes only "New Time" and "Party Size" controls (dialog DOM contains just those two comboboxes plus Save/Close); there is no date field. Time 19:00→20:00 and party 2→4 saved and displayed correctly (card shows September 2, 2026 / 20:00 / 4 Guests), but the date is unmodifiable.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation requirement for reviews
    - Actual: The "Write a Review" tab is available on every restaurant page regardless of reservations. A review was submitted for Sakura Garden while the account had zero completed reservations (the only reservation ever made was for The Golden Fork and it was cancelled), and it was accepted and persisted. No per-reservation limit is enforced either — the form can be resubmitted repeatedly.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rule not enforced (weekend-only coupon accepted on a weekday)
    - Actual: On The Golden Fork reservation form with date Wed Sep 2, 2026, entering WEEKEND20 was accepted: toast "Coupon Applied! 20% off weekend dining" and chip "WEEKEND20 - 20% off". Party-size rule works (GROUP15 with 2 guests rejected: "This coupon requires a party of 6 or more."), and only one coupon slot exists at a time.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing date/time is blocked silently — no error identified to the user
    - Actual: On Sakura Garden, with valid Name/Email/Phone but no date selected and time left at "Select time", clicking Confirm Reservation produced no toast, no inline error and no aria-invalid; nothing was created (tablespot_reservations unchanged). Same silence for a fully empty form. Name/email/phone are enforced only by native HTML5 required/type=email attributes (email "alice.tester" gave native message), so date and time have no user-facing validation feedback.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and details; list summary not updated by new reviews
    - Actual: Sakura Garden: list card shows "4.6" while detail showed "5.0 (1)" before the review and "4.5 (2)" after; the list still shows 4.6 after submitting the review. The Golden Fork: list 4.8 vs detail 4.5 (2). The list also shows no review count.