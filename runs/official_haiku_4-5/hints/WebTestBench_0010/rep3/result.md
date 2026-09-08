# Test Result

## Functionality
- [X] FT-1: Users can browse restaurants and view each restaurant's name, cuisine, average price per diner, rating, location, description, and notable dining features.

- [X] FT-2: Users can search restaurants by name, cuisine, or neighborhood and can combine the search with cuisine and price-range filters; every displayed restaurant satisfies all active criteria.

- [X] FT-3: Users can create a reservation by selecting a future date, an available time, and a party size, providing the required contact information, and submitting it; a successful booking is confirmed and appears among upcoming reservations with the supplied details.

- [X] FT-4: Users can view their upcoming reservations and cancel one after confirming the action; the cancelled reservation is removed from upcoming reservations and retained with a cancelled status in reservation history.

- [X] FT-5: Users can submit a one-to-five-star rating, visit date, name, and written review for a restaurant, and the new review is retained and displayed only with that restaurant.

- [X] FT-6: During reservation, users can apply a supported coupon, see the offered discount, remove it if desired, and have the applied coupon recorded with the resulting reservation.

- [X] FT-7: Past dates are unavailable for new reservations, while a valid future date can be combined with one of the restaurant's available reservation times.

- [X] FT-8: Users can change the date, time, and party size of an upcoming reservation, and the reservation list retains and displays all submitted changes.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Multiple reviews allowed for same visit date/reservation. System accepted a second review with the same visit date (2024-01-15) that already had one review.
    - Actual: Review count increased from 2 to 3, indicating the second review for the same date was accepted. The constraint limiting one review per completed reservation is not enforced.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: WELCOME10 coupon accepted for non-first reservation. The first-reservation eligibility condition is not enforced.
    - Actual: Successfully applied WELCOME10 coupon to Ember & Smoke reservation (not a first reservation). GROUP15 coupon works correctly with 6+ guests. Only one coupon can be applied per reservation (UI-enforced), but coupon eligibility conditions are not fully validated.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing or invalid information is not clearly identified to the user. Form submission fails silently without visible error messages.
    - Actual: Attempted to submit reservation with missing phone number and time fields, and invalid email format. Form submission was prevented but no error messages were displayed to identify the problems to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant rating is inconsistent between restaurant list and detail page. Sakura Garden shows 4.6 on home page list but 4.7 on detail page.
    - Actual: Home page: Sakura Garden rating = 4.6, Detail page: Sakura Garden rating = 4.7. Review count (3) is consistent, but the calculated average rating differs.