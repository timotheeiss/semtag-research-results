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
    - Actual: The "Modify Reservation" dialog contains only "New Time" and "Party Size" controls (dialog innerText: "Modify Reservation / New Time / 19:00 / Party Size / 2 Guests / Save Changes / Close"; no date picker, no date input in DOM). Time 19:00→20:30 and party size 2→4 saved and displayed correctly, but the date (August 28, 2026) cannot be changed anywhere in the app.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: On Trattoria Bella (/restaurant/3), where the user has no reservation at all (only a future confirmed booking at restaurant 1 and a cancelled one at restaurant 2), the "Write a Review" tab was fully enabled and the review submitted successfully ("Review Submitted!" toast, stored in tablespot_reviews with restaurantId "3"). No completed-reservation check and no one-review-per-reservation limit exists anywhere in the flow.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules not enforced: weekend-day rule and first-reservation rule
    - Actual: WEEKEND20 ("20% off weekend dining") was accepted for Monday, August 31, 2026 (and also Friday Aug 28) with toast "Coupon Applied! 20% off weekend dining". WELCOME10 ("10% off your first reservation") was accepted on a second reservation while an existing confirmed reservation (The Golden Fork, res_1787862444658) already existed. Only the GROUP15 party-size rule was enforced ("This coupon requires a party of 6 or more."). One-coupon-per-reservation is enforced structurally (applied coupon replaces the input field).

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid required reservation fields are not identified to the user
    - Actual: Clicking "Confirm Reservation" with empty name/email/phone (and with date+time set), and with an invalid email "not-an-email", silently does nothing: no toast, no inline error, no field highlight (DOM MutationObserver captured zero added nodes, localStorage tablespot_reservations stayed "[]"). The same observer did capture a "Coupon Not Applicable" toast for the coupon flow, so the toast mechanism works — reservation validation simply gives no feedback.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is a static value inconsistent with the review-derived rating on the details page; list is not updated by new reviews and shows no review count
    - Actual: List card ratings differ from detail pages computed from the same reviews: The Golden Fork list 4.8 vs detail 4.5 (2 reviews); Sakura Garden list 4.6 vs detail 5.0 (1); Ember & Smoke list 4.7 vs detail 5.0 (1); Trattoria Bella list 4.5 vs detail 4.5 (1 review) before my review and detail 4.0 (2) after — while the list card still displayed 4.5 unchanged after submission. List cards also never display a review count.