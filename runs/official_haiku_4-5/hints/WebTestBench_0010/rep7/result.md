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
    - Issue: Date modification not available in reservation modification interface
    - Actual: Modification dialog only supports changing time and party size, but not date. Successfully changed time (19:00 → 18:00) and party size (2 → 4 Guests) and verified changes persisted in reservation list, but no mechanism exists to modify reservation date.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review eligibility constraint not enforced - review submitted without completed reservation
    - Actual: Successfully submitted review for Ember & Smoke restaurant without having a completed reservation for it. System allowed review submission by Jane Doe with 5-star rating for visit date Aug 20, 2026, even though no completed reservation exists for this user at this restaurant.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility constraints not enforced - no validation of first-reservation, party-size, or day conditions
    - Actual: Successfully applied WELCOME10 coupon to a 2-guest reservation on Thursday (Aug 28, 2026) without any validation of stated eligibility conditions. System provided no feedback about coupon restrictions and accepted the coupon regardless of apparent conditions (e.g., GROUP15 hints 6+ guests requirement, but WELCOME10 was applied to 2 guests without rejection).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant ratings inconsistent between list and detail views
    - Actual: The Golden Fork shows 4.8 on restaurant list but 4.5 on detail page. Ember & Smoke shows 4.7 on list but 5.0 on detail page. Review counts appear consistent (e.g., both show 2 reviews for The Golden Fork), but displayed ratings differ between views.