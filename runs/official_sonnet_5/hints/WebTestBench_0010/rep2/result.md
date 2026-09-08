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
    - Issue: Modify dialog does not support changing the reservation date
    - Actual: The 'Modify Reservation' dialog only exposes 'New Time' and 'Party Size' selects; there is no date field/control. Time and party size changes (19:00→20:00, 2→4 Guests) saved and reflected correctly in the list, but the date (August 26, 2026) cannot be changed via any UI control.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: Wrote and submitted a review for The Golden Fork even though the user's only reservation there had status 'Cancelled' (not completed) at the time; the review form had no restriction or error and the review was accepted and displayed. No reservation-authorization check or 'one review per completed reservation' limit was observed.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (day, first-reservation) not enforced
    - Actual: Only the party-size rule for GROUP15 is enforced (correctly rejected with toast for party<6). WEEKEND20 (implies a weekend/day restriction) was accepted on Thursday Aug 27 and again on Monday Aug 31, both non-weekend weekdays, with no rejection. WELCOME10 was successfully applied to a second reservation for this user even though a prior reservation had already used WELCOME10, so the 'first-reservation' eligibility condition is not enforced.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating/review count is inconsistent with detail page and does not update after a new review
    - Actual: Before any new review, restaurant list showed The Golden Fork rating 4.8, while its detail page showed 4.5 (2 reviews) — already inconsistent. After submitting a 5-star review, the detail page recalculated to 4.7 (3 reviews), but the home list still displays the unchanged static 4.8, so the list summary never reflects submitted reviews.