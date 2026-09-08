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
    - Actual: The 'Modify Reservation' dialog only offers 'New Time' and 'Party Size' fields; there is no control to change the reservation date. Time and party size changes were saved successfully and reflected in the list (20:00, 4 Guests), but the date field is entirely absent from the modify flow.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not restricted to users with a completed reservation at the restaurant
    - Actual: Submitted a review for Trattoria Bella via the 'Write a Review' tab with no prior reservation (completed or otherwise) for that restaurant existing in the session (previous reservation was cancelled). The review form contains no reservation-linking mechanism and the submission succeeded immediately, increasing review count from 1 to 2, with no eligibility check enforced.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: WELCOME10 'first reservation' eligibility rule is not enforced
    - Actual: GROUP15 correctly rejected 'This coupon requires a party of 6 or more' when party size was 2, and was accepted once party size was raised to 6 (party-size rule works). Single-coupon-per-reservation is enforced structurally (coupon input is replaced by an applied-coupon badge with no way to add a second code). However, after confirming a reservation for timothee.issenmann@gmail.com using WELCOME10 (its first use), applying WELCOME10 again for the same email on a new reservation at a different restaurant was accepted again with toast 'Coupon Applied! 10% off your first reservation', even though this was no longer the user's first reservation. The first-reservation eligibility condition is not validated.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant rating is inconsistent between list and detail views, and does not update after a new review is submitted
    - Actual: Restaurant list page shows Trattoria Bella rating as 4.5, while its detail page shows 4.0 for the same restaurant (also observed for The Golden Fork: 4.8 on list vs 4.5 on detail). After submitting a new 4-star review (existing review was 5-star), the review count updated from (1) to (2) but the displayed average rating on the detail page remained unchanged at 4.0, and the list page still showed the old 4.5 — the average is not recalculated from actual review data.