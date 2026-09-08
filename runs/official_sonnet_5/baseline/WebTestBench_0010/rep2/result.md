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
    - Issue: Reservation modification dialog does not support changing the date
    - Actual: Clicking 'Modify' on an upcoming reservation opens a 'Modify Reservation' dialog offering only 'New Time' and 'Party Size' fields; there is no date field/control. Time was successfully changed from 19:00 to 18:00 and party size from 6 to 4, and these persisted in the Upcoming list, but the date (Aug 26, 2026) cannot be modified through this dialog.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Missing constraint enforcement: review submission not gated by completed reservation
    - Actual: On restaurant/3 (Trattoria Bella), with zero reservations ever made for this restaurant/user, the 'Write a Review' form accepted and successfully submitted a review (name 'TestReviewerNoRes', rating 4, review text) with no validation against any completed reservation. Review count increased from (1) to (2) immediately, confirming the review was accepted despite no completed reservation existing at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (day-of-week and first-reservation) not enforced; only party-size rule enforced
    - Actual: Party-size rule works correctly: GROUP15 (requires 6+ guests) was rejected with 'This coupon requires a party of 6 or more.' when party size was 2, and accepted at 6 guests. However, WEEKEND20 ('20% off weekend dining') was accepted on Wednesday Aug 26, 2026, a non-weekend date, violating the day-of-week eligibility condition. Also, WELCOME10 ('10% off your first reservation') was accepted for a booking attempt on restaurant/4 even though the user already had a confirmed reservation (Trattoria Bella, Aug 26 2026) from earlier in the session, violating the first-reservation-only eligibility condition. The UI does correctly prevent applying more than one coupon at a time (coupon input is replaced by a single applied-coupon chip with remove button).

- [X] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review-count inconsistency between restaurant list and details page; list rating not updated after new review
    - Actual: Restaurant list page shows Trattoria Bella at 4.5 rating (no review count), while its details page shows 4.0 (2) after reviews. Even after submitting a new review that changed the details page from 4.0 (1) to 4.0 (2), the list page still displayed the static 4.5 unchanged. Same mismatch observed for The Golden Fork: list=4.8, details=4.5 (2).