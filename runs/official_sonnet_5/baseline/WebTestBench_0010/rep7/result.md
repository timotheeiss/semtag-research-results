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
    - Actual: The "Modify Reservation" dialog for an upcoming reservation only offers "New Time" and "Party Size" fields — there is no date field/control at all. Changed time 18:00->19:00 and party size 2->3 Guests, saved successfully and both changes were reflected in the list, but the date (Aug 26, 2026) could not be changed since no UI is provided for it.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission not restricted to completed reservations
    - Actual: Submitted a review for Sakura Garden while having zero completed reservations there (only one Upcoming reservation existed, which is not a completed visit). The review form had no restriction, and submission succeeded immediately ("Review Submitted!"), increasing the review count from 1 to 2. The constraint that a review requires a completed reservation, with one review per completed reservation, is not enforced.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: First-reservation coupon eligibility rule not enforced
    - Actual: GROUP15 correctly rejected for a 2-guest party ("This coupon requires a party of 6 or more"), showing party-size rule works. However, WELCOME10 (labeled "10% off your first reservation") was accepted again on a second reservation for Trattoria Bella even though the account already had a prior confirmed reservation at Sakura Garden with WELCOME10 applied — the first-reservation eligibility condition is not enforced.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: No validation feedback shown to user on invalid/missing reservation submission
    - Actual: Clicking "Confirm Reservation" with all fields (date, time, name, email, phone) empty produced no error message, no inline field errors, no toast notification, and no reservation was created (Upcoming (0) confirmed on /reservations). The button remained enabled and the form simply did nothing, giving the user no indication of what is missing or invalid.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating/review count not synced with detail page after new review
    - Actual: Before the new review, list showed Sakura Garden rating 4.6; detail page showed 5.0 (1) — already inconsistent. After submitting a new review, detail page updated to 4.5 (2), but the restaurant list still shows the old static 4.6 rating with no review count. The list-page rating never reflects submitted reviews.