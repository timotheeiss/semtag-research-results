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
    - Issue: Modify dialog does not allow changing the reservation date
    - Actual: The "Modify Reservation" dialog only exposes "New Time" and "Party Size" controls; there is no date field. Time (19:00→20:00) and party size (2→4 Guests) changes saved and are correctly reflected in the reservation list, but the date (Aug 28, 2026) cannot be modified through this UI.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: Submitted a review for "Sakura Garden" with no prior reservation of any kind (all reservations for the session were cancelled beforehand) and the review was accepted without any error, warning, or eligibility check.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon day-eligibility rule not enforced
    - Actual: GROUP15 correctly rejected for a 2-guest party ("This coupon requires a party of 6 or more"), and only one coupon field is available at a time (no way to stack two coupons). However, WEEKEND20 (a weekend-only discount, "20% off weekend dining") was accepted and applied for Aug 27, 2026, which is a Thursday, not a weekend day — the day-of-week eligibility condition was not checked.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating shown in restaurant list is inconsistent with restaurant detail page and does not update after a new review
    - Actual: Restaurant list shows Sakura Garden rating 4.6 both before and after submitting a new review, while the detail page showed 5.0 before the review and 4.5 after (average of the two reviews). The Golden Fork similarly shows 4.8 in the list but 4.5 on its detail page. List rating never matches detail rating and does not react to new reviews.