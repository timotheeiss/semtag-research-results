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
    - Issue: Reservation date cannot be modified — no date control in the Modify dialog
    - Actual: Modify Reservation dialog contains only "New Time" and "Party Size" selects plus Save Changes/Close (dialog innerText: "Modify Reservation / New Time / 19:00 / Party Size / 2 Guests / Save Changes / Close"); no date picker or date input exists. Time 19:00→20:30 and party size 2→5 saved correctly ("August 31, 2026 / 20:30 / 5 Guests"), but the date stayed August 31, 2026 with no way to change it.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No reservation gating on reviews — anyone can review any restaurant, unlimited times
    - Actual: The "Write a Review" tab on /restaurant/1 is freely available. The user had no completed reservation for The Golden Fork (Past tab held only a Cancelled future booking; Past completed = 0), yet the review submitted successfully ("Review Submitted! Thank you for sharing your experience.", count 2→3). A second review was then submitted immediately for the same restaurant (count 3→4, rating 4.7→3.8), so there is no one-review-per-completed-reservation limit and no eligibility check at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules not enforced: weekend-only and first-reservation conditions are ignored
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for Monday, Aug 31, 2026 (calendar row confirms 30=Sunday, 31=Monday) — toast "Coupon Applied! 20% off weekend dining", "- 20% off". WELCOME10 ("10% off your first reservation") was accepted on /restaurant/5 after the user already had 2 prior reservations (a cancelled Golden Fork booking and an active Trattoria Bella booking on Aug 29). Only the party-size rule works (GROUP15 with 2 guests → "Coupon Not Applicable: This coupon requires a party of 6 or more") and the single-coupon rule holds (applying a coupon replaces the input with an applied chip + remove button).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is a static value inconsistent with the review-derived rating on the details page and never updates after a review
    - Actual: Before any review: list card for The Golden Fork showed 4.8 while its detail page showed 4.5 (2 reviews) — already inconsistent. After submitting reviews the detail page updated correctly (4.5→4.7 with 3 reviews, then →3.8 with 4 reviews), but the list card still shows 4.8. The list card also displays no review count, so the list summary is not derived from the reviews at all.