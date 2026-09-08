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
    - Issue: Date cannot be changed when modifying a reservation
    - Actual: The "Modify Reservation" dialog only offers "New Time" and "Party Size" selectors; there is no date field or date picker in the modify dialog. Time (19:00→20:00) and party size (2→4 Guests) changes saved and were reflected in the reservation list, but the reservation's date (August 28, 2026) cannot be modified at all through the UI.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission not restricted to completed reservations
    - Actual: Submitted a review for The Golden Fork despite having no completed reservation there (the only reservation made for it had status "Cancelled", not completed). The review form had no eligibility check, showed no warning, and accepted the review, incrementing the review count from 2 to 3. This violates the constraint that a review may only be submitted for a restaurant covered by a completed reservation.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility "day" rule not enforced
    - Actual: Applied coupon "WEEKEND20" (labeled as a weekend-only discount) on a Friday (Aug 28, 2026, confirmed via Date API), a non-weekend date, with 2 guests. The coupon was accepted and a -20% off discount was shown, when it should have been rejected for not meeting the weekend-day eligibility condition. (Party-size eligibility for GROUP15 was correctly enforced: applying GROUP15 with only 2 guests, below the stated 6+ guest minimum, was correctly rejected with no discount applied.)

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid reservation info is not identified to the user
    - Actual: Clicking "Confirm Reservation" with no date/time selected and empty required fields produced no visible error, message, or field highlighting. Repeating with Name, invalid email ("notanemail"), and Phone filled but no date/time still produced no feedback. The page text remained unchanged and no reservation was created (Upcoming (0)). The user has no way to know why submission failed or what to fix.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating/review-count inconsistent between restaurant list and details; review submission doesn't update all summaries
    - Actual: Restaurant detail page for The Golden Fork initially showed rating 4.5 (2) reviews, but the home restaurants list showed 4.8 for the same restaurant — a mismatch before any new review was added. After submitting a new 5-star review, the detail page recomputed to 4.7 (3), but the home page grid still displayed the stale 4.8 rating, unchanged. The homepage summary was not updated by the new review.