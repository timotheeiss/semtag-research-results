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
    - Actual: The "Modify Reservation" dialog only exposes "New Time" and "Party Size" controls — there is no date field/date picker. Time (19:00→20:00) and party size (2→4) saved and displayed correctly, but the date (September 1, 2026) cannot be changed anywhere.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review eligibility not enforced
    - Actual: With zero reservations (My Reservations showed Upcoming (0) / Past (0)), the "Write a Review" form on /restaurant/1 was fully available and a 4-star review by "QA Tester" was accepted ("Review Submitted!" toast; review count went 2 → 3, rating 4.5 → 4.3). No completed-reservation requirement and no one-review-per-reservation limit is applied.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (weekend-day and first-reservation) not enforced
    - Actual: WEEKEND20 was accepted for Tue Sep 1, 2026 (badge "WEEKEND20 - 20% off", no rejection). WELCOME10 ("10% off your first reservation") was accepted on a 2nd and 3rd reservation attempt while the user already had a confirmed Trattoria Bella reservation. Only the GROUP15 party-size rule worked (rejected with "This coupon requires a party of 6 or more." at 2 guests). One-coupon-per-reservation is correctly enforced (input is replaced by the applied coupon badge until removed).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and details; list summary not updated by new review
    - Actual: The Golden Fork: list card shows 4.8 (no review count) while detail page showed 4.5 (2) before my review and 4.3 (3) after. After submitting a 4-star review the detail rating/count updated (4.5→4.3, 2→3) but the list card still shows the static 4.8.