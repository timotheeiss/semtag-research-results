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
    - Issue: Modify reservation dialog does not support changing the date
    - Actual: Opening 'Modify' on an upcoming reservation shows a dialog with only 'New Time' and 'Party Size' fields; there is no date field. Time was changed 13:00→19:00 and Party Size 2→4 Guests, saved successfully and reflected in the list (toast 'Reservation Updated'), but the reservation's date (Aug 27, 2026) could not be changed at all through this UI, violating the requirement to change date, time, and party size.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission has no eligibility restriction tied to completed reservations
    - Actual: Submitted a review for The Golden Fork despite having no completed reservation there (only an 'Upcoming' reservation existed at the time of first review submission) - it was accepted immediately ('Review Submitted!'). Then submitted a second review for the same restaurant with a different name and it was accepted again without any error, block, or limit, taking the review count from 3 to 4. This shows no restriction exists requiring a completed reservation to review, and no limit of one review per completed reservation.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (first-reservation, day) not enforced server/client-side
    - Actual: GROUP15 (requires party of 6+) was correctly rejected for a 2-guest reservation with message 'This coupon requires a party of 6 or more.' However: (1) WEEKEND20 ('20% off weekend dining') was accepted and applied for Aug 26, 2026, which is a Wednesday, not a weekend day - the day-of-week eligibility rule was not enforced. (2) After already completing one reservation with WELCOME10 applied (a 'first reservation' coupon), WELCOME10 was successfully applied again on a second, different restaurant's reservation form, showing 'Coupon Applied! 10% off your first reservation' even though this was no longer the user's first reservation - the first-reservation eligibility rule was not enforced. Only the party-size rule and the 'max one coupon per reservation' constraint (verified via UI replacing the input with a single applied-coupon badge) worked correctly.

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating/count not synced with detail page or review submissions
    - Actual: The Golden Fork detail page initially showed rating 4.5 (2 reviews) computed from actual reviews, while the restaurant list page showed a different, static rating of 4.8 for the same restaurant - inconsistent from the start. After submitting two new reviews, the detail page rating updated correctly (4.5→4.7→4.0, review count 2→3→4), but the restaurant list page continued to display the original static '4.8' and did not reflect the new review count or updated average at all.