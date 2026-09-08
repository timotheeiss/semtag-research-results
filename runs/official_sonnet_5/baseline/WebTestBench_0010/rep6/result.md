# Test Result

## Functionality
- [ ] FT-1: Users can browse restaurants and view each restaurant's name, cuisine, average price per diner, rating, location, description, and notable dining features.
  - Bug Report:
    - Issue: Missing average price per diner
    - Actual: Restaurant list and detail pages show only symbolic price tier ($, $$, $$$, $$$$) rather than an actual average price per diner amount. Name, cuisine, rating, location, description, and dining features (e.g. Private Dining, Wine Pairing) are all present, but no numeric average price value exists anywhere on the page (confirmed via DOM scan for $-prefixed numbers).

- [X] FT-2: Users can search restaurants by name, cuisine, or neighborhood and can combine the search with cuisine and price-range filters; every displayed restaurant satisfies all active criteria.

- [X] FT-3: Users can create a reservation by selecting a future date, an available time, and a party size, providing the required contact information, and submitting it; a successful booking is confirmed and appears among upcoming reservations with the supplied details.

- [X] FT-4: Users can view their upcoming reservations and cancel one after confirming the action; the cancelled reservation is removed from upcoming reservations and retained with a cancelled status in reservation history.

- [X] FT-5: Users can submit a one-to-five-star rating, visit date, name, and written review for a restaurant, and the new review is retained and displayed only with that restaurant.

- [X] FT-6: During reservation, users can apply a supported coupon, see the offered discount, remove it if desired, and have the applied coupon recorded with the resulting reservation.

- [X] FT-7: Past dates are unavailable for new reservations, while a valid future date can be combined with one of the restaurant's available reservation times.

- [ ] FT-8: Users can change the date, time, and party size of an upcoming reservation, and the reservation list retains and displays all submitted changes.
  - Bug Report:
    - Issue: Date cannot be changed when modifying a reservation
    - Actual: The "Modify Reservation" dialog opened from an upcoming reservation only exposes "New Time" and "Party Size" fields — there is no date field or control to change the reservation's date. Changing time (19:00→19:30) and party size (2→4 Guests) worked correctly and was saved and reflected in the reservation list, but the date (August 26, 2026) could not be modified through this dialog, so the requirement to change date, time, and party size is only partially met.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted without any completed reservation at the restaurant
    - Actual: The "Write a Review" form on a restaurant's page (Star rating, Name, Date of Visit, Review text) has no linkage to the reviewer's reservations and no check for a completed visit. A review for The Golden Fork was submitted successfully even though the only reservation ever held there by this session had been Cancelled (not completed) at the time of submission — the site accepted the review with no eligibility validation and no way to select/reference a qualifying reservation. This also means the "one review per completed reservation" cap cannot be enforced since there's no reservation-review linkage at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (first-reservation, day) not enforced
    - Actual: Party-size rule works correctly: GROUP15 was rejected for a 2-guest party with message "This coupon requires a party of 6 or more." However: (1) First-reservation rule not enforced — after already completing one reservation with email testuser@example.com using WELCOME10 ("10% off your first reservation"), the same coupon was accepted again on a second reservation for the same email without any rejection. (2) Day rule not enforced — WEEKEND20 ("20% off weekend dining") was accepted for Wednesday, August 26, 2026, a weekday, with no rejection. The UI does correctly prevent applying a second coupon on top of an already-applied one (input is replaced by a single applied-coupon pill), so the "at most one coupon" rule holds, but two of the three stated eligibility conditions are not actually validated.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating shown on restaurant list is inconsistent with restaurant details and does not update after new reviews
    - Actual: Restaurant list initially showed The Golden Fork at rating 4.8, while its own details page showed 4.5 (2) — the two values were inconsistent even before any new review was submitted. After submitting a 3-star review, the details page correctly recalculated to 4.0 (3) and the new review appeared under Reviews (3), but the restaurant list page (revisited after submission) still displayed the stale 4.8 rating, unchanged by the new review. The list-page rating is not derived from/consistent with the details-page review data.