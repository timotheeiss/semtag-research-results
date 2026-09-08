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
    - Actual: The "Modify Reservation" dialog contains only "New Time" and "Party Size" controls plus Save Changes — there is no date field or date picker. Time (19:00→20:00) and party size (2→5 Guests) saved and are reflected in the list, but the date Aug 29, 2026 cannot be changed anywhere in the UI.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review eligibility not enforced
    - Actual: With zero reservations in the account (My Reservations showed Upcoming (0) and Past (0)), the "Write a Review" form on /restaurant/1 accepted a 5-star review from "QA Tester" and showed "Review Submitted!". Review count went 2 → 3 and rating 4.5 → 4.7. No completed-reservation check or per-reservation one-review limit exists.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (weekend, first-reservation) not enforced
    - Actual: On a Wednesday booking (Sep 2, 2026) WEEKEND20 was accepted with toast "Coupon Applied! 20% off weekend dining". WELCOME10 ("10% off your first reservation") was also accepted on the user's second reservation, after a WELCOME10 booking had already been made at Sakura Garden. Only the GROUP15 party-size rule is enforced ("This coupon requires a party of 6 or more."). The one-coupon-per-reservation limit is respected (the input is replaced by the applied-coupon chip).

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid input is not identified to the user
    - Actual: Clicking "Confirm Reservation" with an empty form, and again with date+time set but name/email/phone empty, and again with name/phone filled but email "not-an-email", produced no toast, no inline error, no aria-invalid and no field highlight — the button silently did nothing. The app does block submission, but the user is never told what is missing or invalid (the same button shows a "Reservation Confirmed!" toast once valid data is entered).


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Rating inconsistent between list and details; list summary not updated by new review
    - Actual: The Golden Fork shows 4.8 on the restaurant list but 4.5 (2 reviews) on its detail page before any new review — already inconsistent. After submitting a 5-star review the detail page updated to 4.7 (3) while the list card still shows 4.8. The list card also shows no review count.