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
    - Issue: Reservation date cannot be changed — modify dialog offers no date control
    - Actual: The "Modify Reservation" dialog contains only "New Time" and "Party Size" selects plus Save Changes/Close (dialog text: "Modify Reservation / New Time / 19:00 / Party Size / 2 Guests / Save Changes / Close"). Time (19:00→20:30) and party size (2→6) saved and are displayed correctly, but there is no way to change the date, which remained August 29, 2026.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: On Sakura Garden (restaurant 2) the user had NO reservation at all (only an upcoming one at The Golden Fork). The "Write a Review" tab was fully available and a 5-star review by "QA Tester" was accepted and persisted (tablespot_reviews now contains restaurantId "2" / "QA Tester"), immediately appearing in the review list and raising the displayed rating to 5.0. No eligibility check and therefore no one-review-per-completed-reservation limit exists.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Day-of-week and first-reservation coupon eligibility rules are not enforced
    - Actual: WEEKEND20 was accepted for a reservation on Monday, Aug 31 2026 (shown as applied, "- 20% off"). WELCOME10 (first-reservation offer) was also accepted while the user already had 1 existing reservation in tablespot_reservations, giving "- 10% off". Only the party-size rule works (GROUP15 with 2 guests → "Coupon Not Applicable: This coupon requires a party of 6 or more."). The single-coupon rule holds: once a coupon is applied the code input/Apply button is replaced by the applied badge + remove button.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid required reservation information is never identified to the user
    - Actual: Clicking "Confirm Reservation" with a completely empty form, and again with date+time set but empty name/email/phone, and again with name/phone filled but email "not-an-email", each time silently did nothing: no toast, no inline error, no aria-invalid, no field highlighting (queried [data-sonner-toast],[role=alert],[role=status],[aria-invalid=true] → all empty) and localStorage tablespot_reservations stayed "[]". The same toast query DOES return coupon errors, proving toasts are detectable, so the form genuinely gives zero feedback.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating shown in restaurant list is inconsistent with the detail page and is not updated by new reviews; list shows no review count
    - Actual: The Golden Fork: list card shows 4.8, detail page shows 4.5 (2) — its two reviews are 5 and 4 stars. Sakura Garden: after submitting a new 5-star review the detail page rating changed to 5.0 (2), while the list card still shows the hard-coded 4.6. No review count is displayed anywhere in the list, so list and detail summaries never agree.