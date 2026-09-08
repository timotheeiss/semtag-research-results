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
    - Issue: Reservation date cannot be modified — the Modify dialog offers no date control
    - Actual: The "Modify Reservation" dialog contains only "New Time" and "Party Size" comboboxes plus Save Changes/Close (accessibility snapshot of [role=dialog] shows no date picker, and semantic_snapshot of the dialog lists only .modify.time and .modify.party-size). Time 19:00→20:30 and party 2→6 saved correctly and the card updated to "September 2, 2026 / 20:30 / 6 Guests", but the date is unchangeable, so date changes cannot be submitted or displayed.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: No completed-reservation gating on review submission; reviews can be posted by anyone, unlimited times
    - Actual: On Trattoria Bella (restaurant 3) the user had no reservation of any kind (tablespot_reservations contained only one cancelled reservation for restaurant 1). The "Write a Review" tab and form were fully available and submitting produced "Review Submitted! Thank you for sharing your experience." with the review persisted and displayed. There is no reservation check and no one-review-per-completed-reservation limit anywhere in the flow.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: WEEKEND20 coupon's day-of-week eligibility rule is not enforced
    - Actual: With reservation date Sep 2, 2026 (a Wednesday — calendar row "30 31 1 2 3 4 5" maps 2 to the We column) and party size 2, applying WEEKEND20 succeeded: toast "Coupon Applied! 20% off weekend dining" and reservation.coupon.applied="WEEKEND20", discount "- 20% off". A weekend-only coupon was accepted on a weekday. (GROUP15 correctly rejected with "This coupon requires a party of 6 or more."; only one coupon can be held at a time since the code input is replaced by the applied-coupon row.)

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid reservation input is silently rejected with no user-facing error message
    - Actual: Clicking "Confirm Reservation" with a completely empty form does nothing: no toast (MutationObserver on document.body captured no notification node), no inline text, no aria-invalid/role=alert element, and localStorage tablespot_reservations stays "[]". Same silent no-op when date=Sep 2 2026, time=19:00, name and phone filled but email="not-an-email" — reservation is correctly blocked, but the user is never told which field is missing or invalid.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is static and inconsistent with the review-derived rating shown on the detail page; list is not updated by new reviews
    - Actual: The Golden Fork: list card shows 4.8, detail shows 4.5 (2 reviews of 5 and 4). Trattoria Bella: list card showed 4.5 while detail showed 4.0 with (1) review. After submitting a 5-star review for Trattoria Bella the detail correctly updated to 4.5 / (2) reviews, but the list card still displayed the unchanged static 4.5. The list also shows no review count at all, so counts cannot be compared between views.