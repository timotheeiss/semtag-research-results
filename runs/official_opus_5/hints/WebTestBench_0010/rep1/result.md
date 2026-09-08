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
    - Actual: The Modify Reservation dialog only contains "New Time" and "Party Size" controls (dialog text: "Modify Reservation / New Time / 19:00 / Party Size / 2 Guests / Save Changes / Close") — there is no date field, so the date of an upcoming reservation cannot be changed. Time (19:00→20:30) and party size (2→4 Guests) did save and were reflected in the list.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation and is unlimited
    - Actual: With zero completed reservations (Upcoming 0, Past 1 = a CANCELLED reservation at The Golden Fork), the "Write a Review" form on Trattoria Bella (restaurant never reserved) was fully enabled and accepted a review ("Review Submitted!", review count 1→2). A second review was then submitted for the same restaurant immediately afterwards (count 2→3, rating 4.0→4.5→3.3), so there is no one-review-per-completed-reservation limit and no eligibility check at all.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility conditions not enforced
    - Actual: WEEKEND20 was accepted for Mon Aug 31, 2026 and Fri Aug 28, 2026 (non-weekend days) showing "- 20% off". GROUP15 (stated 6+ guests) was correctly rejected at 2 guests, but once applied at 6 guests it remained applied ("GROUP15 / -15% off") after party size was changed back to 2 Guests, i.e. eligibility is checked only at apply time. Only the one-coupon-at-a-time rule holds (input is replaced by the applied coupon chip).

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid data is silently rejected with no user-facing message
    - Actual: Clicking "Confirm Reservation" with an empty form, and again with date+time set but name/email/phone empty, and again with an invalid email ("notanemail") produced no reaction at all: no inline error, no toast/alert element ([data-sonner-toast],[role=alert],[role=status] all empty), no aria-invalid, no change in page text. Submission is blocked (no confirmation appears), but the user is never told what is missing or invalid. Same form submitted with a valid email immediately showed "Reservation Confirmed!".


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is a static value inconsistent with the review-derived rating on the detail page, and is not updated by new reviews
    - Actual: The Golden Fork: list card rating 4.8 vs detail rating 4.5 (2 reviews: 5★+4★). Trattoria Bella: list card 4.5 vs detail 4.0 (1 review, 4★) before any change. After submitting a 5★ and then a 1★ review on Trattoria Bella, the detail summary updated correctly (4.0/(1) → 4.5/(2) → 3.3/(3)) but the restaurant list card still showed 4.5. The list cards also display no review count at all.