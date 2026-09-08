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
    - Issue: Reservation date cannot be changed — modify dialog offers no date control
    - Actual: "Modify Reservation" dialog for the upcoming Golden Fork booking contains only "New Time" and "Party Size" selects plus Save Changes/Close; there is no date picker or any way to change the reservation date. Time (19:00→20:30) and party size (2→4 Guests) saved correctly and the list updated, but the date remained August 31, 2026 with no option to modify it.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation-based gating on reviews
    - Actual: On Spice Route (/restaurant/5), for which the user has never made any reservation (only reservation ever created was for The Golden Fork, later cancelled), the "Write a Review" tab is fully enabled and the review submitted successfully ("Review Submitted!" toast, review persisted and shown as Reviews (1)). There is no check for a completed reservation and no per-reservation one-review limit anywhere in the UI.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon day-eligibility rule not enforced: weekend-only coupon accepted on a weekday
    - Actual: With date = Mon Aug 31, 2026 and party size 2 on The Golden Fork, applying WEEKEND20 succeeded: badge "WEEKEND20 - 20% off" and toast "Coupon Applied! 20% off weekend dining", despite Aug 31, 2026 being a Monday. (Party-size rule works: GROUP15 with 2 guests was rejected with "This coupon requires a party of 6 or more."; only one coupon can be held at a time since the input is replaced by the applied-coupon badge.)

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Ratings inconsistent between list and detail; list summary not derived from reviews and never updates
    - Actual: Restaurant list shows static ratings with no review count (The Golden Fork 4.8, Spice Route 4.4), while detail pages compute from reviews (The Golden Fork 4.5 (2); Spice Route 4.0 (1) after my 4-star review). After submitting the Spice Route review, the detail header/Reviews tab updated to 4.0 (1) but the list card still shows 4.4 — the two views disagree.