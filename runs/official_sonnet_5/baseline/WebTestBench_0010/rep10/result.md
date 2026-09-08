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
    - Actual: The 'Modify Reservation' dialog only offers 'New Time' and 'Party Size' fields; there is no date field. Time (19:00→20:00) and Party Size (2→4 Guests) changes saved successfully and persisted in the list, but the date (August 27, 2026) could not be changed at all, so users cannot change the date of an upcoming reservation as required.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not restricted to restaurants with a completed reservation
    - Actual: Navigated directly to Trattoria Bella (restaurant/3), a restaurant with no reservation (completed or otherwise) ever made by this user/session, opened 'Write a Review' tab, and successfully submitted a review with no gating, eligibility check, or error. The review was accepted and displayed immediately, proving the app does not enforce that a review requires a completed reservation at that restaurant, nor limit reviews to one per completed reservation.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility day-of-week rule not enforced
    - Actual: On restaurant/2 with date set to Thursday, August 27, 2026 (a weekday, not a weekend), applying coupon 'WEEKEND20' succeeded with toast 'Coupon Applied! 20% off weekend dining' and the discount chip was added — the eligibility condition restricting this coupon to weekend dates was not enforced. By contrast, GROUP15 correctly rejected an ineligible party size ('This coupon requires a party of 6 or more'), showing eligibility checks exist for some but not all stated conditions. The UI does structurally prevent applying more than one coupon at a time (input is replaced by a single applied-coupon chip with no way to add a second without removing the first).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating shown on restaurant list is inconsistent with, and does not sync with, the rating on the restaurant detail page
    - Actual: List page shows Trattoria Bella at a static 4.5 rating; its detail page shows 4.0 (1) before any new review and still 4.0 (2) after submitting a new review. Similarly, The Golden Fork shows 4.8 on the list vs 4.5 (2) on its detail page, and Sakura Garden shows 4.6 on the list vs 5.0 (1) on its detail page. The list-page rating appears to be a hardcoded/independent value that never reflects the actual review-derived rating shown on the detail page, and does not update after a new review is submitted.