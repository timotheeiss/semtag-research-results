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
    - Actual: The Modify Reservation dialog contains only "New Time" and "Party Size" controls (dialog text: "Modify Reservation / New Time 19:00 / Party Size 2 Guests / Save Changes / Close") — there is no date field. Time 19:00→20:30 and party size 2→5 saved and displayed correctly, but the date (September 1, 2026) cannot be changed at all.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: With zero reservations (My Reservations showed "Upcoming (0)" / "Past (0)" and empty state), the "Write a Review" form on /restaurant/1 was fully available and a 5-star review was accepted and published (Reviews count 2→3). No eligibility check or per-reservation one-review limit is enforced anywhere.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Day-of-week and first-reservation eligibility rules are not enforced
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for Tuesday Sep 1, 2026 (- 20% off). WELCOME10 ("10% off your first reservation") was accepted again on a second reservation after one had already been booked (- 10% off). Only the party-size rule works (GROUP15 with 2 guests → "This coupon requires a party of 6 or more."), and one-coupon-at-a-time is enforced structurally (apply control replaced by applied coupon + remove). Unknown codes are silently ignored with no feedback.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing date/time blocks submission silently with no message identifying the problem
    - Actual: Name/email/phone are enforced by native HTML5 validation with visible messages (e.g. "Please fill out this field.", invalid email "not-an-email" → "Please include an '@'..."). However, with all contact fields valid but Date and Time left unset, clicking "Confirm Reservation" does nothing: no toast, no inline error text, no aria-invalid / error styling on the Date or Time controls, and no reservation created. The user is given no indication of what is missing.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and detail; list summary not updated by new review; review count absent from list
    - Actual: Home list shows The Golden Fork 4.8 while /restaurant/1 shows 4.5 (2 reviews) before the review — already inconsistent. After submitting a 5-star review the detail updated to 4.7 (3) but the home list still shows 4.8. Sakura Garden: list 4.6 vs detail 5.0 (1). List cards display no review count at all.