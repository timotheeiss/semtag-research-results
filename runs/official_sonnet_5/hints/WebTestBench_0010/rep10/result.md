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
    - Issue: Reservation date cannot be modified
    - Actual: The 'Modify Reservation' dialog for an upcoming reservation only offers 'New Time' and 'Party Size' fields - there is no control to change the reservation date. Time and party size changes were saved successfully and reflected in the list (18:00->19:00, 2 Guests->4 Guests), but the date (Aug 28, 2026) could not be changed, so the requirement to change date, time, and party size is only partially met.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted with no completed (or any) reservation at the restaurant
    - Actual: Submitted a full review (rating, name, visit date, comment) for Ember & Smoke (restaurant 4) despite the current user having zero reservations - upcoming, past, or cancelled - at that restaurant. The review form contained no eligibility check and the submission succeeded immediately ('Review Submitted!'), so the constraint that a review requires a completed reservation at that restaurant is not enforced.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (first-reservation, day) not enforced
    - Actual: Party-size rule correctly enforced: GROUP15 rejected with '6+ guests required' toast when party size was 2. However: (1) WELCOME10 (advertised as presumably a first-reservation/welcome offer) was successfully re-applied on a second, separate reservation attempt after the user already had one confirmed reservation - no first-reservation restriction enforced. (2) WEEKEND20 was successfully applied to a reservation dated Aug 27, 2026, which is a Thursday (confirmed via the date picker's weekday columns), not a weekend day - no day-of-week restriction enforced. Single-coupon-per-reservation UI constraint (Apply replaced by Remove once applied) does work correctly.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: No validation feedback shown for missing/invalid required fields
    - Actual: Clicking 'Confirm Reservation' with all fields empty (no date, time, name, email, phone) produced no error message, no toast, no inline field errors, and no navigation - the form silently did nothing. Repeating with name/phone filled but an invalid email ('not-an-email') and no date/time selected also produced no visible error indication anywhere on the page (checked DOM for role=alert/error/destructive classes - none found).


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating/review count is static and inconsistent with the detail page, and does not update after a new review
    - Actual: Before any new review, restaurant list showed The Golden Fork at 4.8 while its detail page showed 4.5 (computed correctly from its 2 reviews, avg of 5&4). For Ember & Smoke: list showed 4.7 both before and after submitting a new review, while the detail page rating changed from 5.0(1) to 4.5(2) after the review was submitted. The homepage list rating values never reflect the actual review-derived averages/counts shown on restaurant detail pages, and do not update when a new review is submitted.