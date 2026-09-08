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
    - Issue: Modify dialog does not allow changing the reservation date
    - Actual: Opening "Modify" on an upcoming reservation only exposed "New Time" and "Party Size" fields; there was no date field/control. Time (19:00→20:00) and party size (2→4 Guests) changes saved and were reflected in the reservation list, but the date (August 26, 2026) could not be changed, so users cannot change date/time/party size as required.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted with no completed-reservation restriction
    - Actual: Submitted a review for Sakura Garden despite having no completed reservation there (my only reservation for that restaurant had been cancelled, not completed). The "Write a Review" form and Submit action are available unconditionally with no check against reservation history, and the review was accepted without any error.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Day-of-week eligibility rule for WEEKEND20 coupon is not enforced
    - Actual: On restaurant 3, selected date Aug 26, 2026 (a Wednesday, confirmed via the date-picker grid column position under "We"), then entered coupon WEEKEND20 ("20% off weekend dining") and clicked Apply. It was accepted: reservation.coupon.applied="WEEKEND20", discount "- 20% off" shown, despite the date not being a weekend. (Party-size rule for GROUP15 worked correctly: rejected with "This coupon requires a party of 6 or more" at 2 guests, accepted at 6 guests. Single-coupon-per-reservation UI also behaved correctly - applying a coupon replaces the input with a remove button, preventing a second code entry without first removing.)

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: No error message shown when required/invalid reservation fields are missing
    - Actual: Submitting the reservation form with all fields empty, and later with an invalid email format ("not-an-email") plus other valid required fields, both silently blocked submission (page stayed on restaurant page, no reservation created) but no error/validation text appeared anywhere in the DOM (checked via full-text regex scan for 'required','invalid','error','please').


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is inconsistent with restaurant detail rating and does not update after review submission
    - Actual: Homepage grid shows a static rating for restaurant 1 "The Golden Fork" of 4.8, but its detail page shows 4.5 (2 reviews, averaging 5+4=4.5) - a permanent mismatch. Similarly, restaurant 2 "Sakura Garden" showed 4.6 on the homepage before any change; on its detail page rating was 5.0 (1 review) before, then after submitting a new 4-star review the detail page recalculated to 4.5 (2 reviews). Returning to the homepage afterward still shows the unchanged static 4.6 for Sakura Garden - it never reflected either the pre- or post-review actual value, confirming the list rating is not derived from the same review data as the detail page and is not updated when a review is submitted.