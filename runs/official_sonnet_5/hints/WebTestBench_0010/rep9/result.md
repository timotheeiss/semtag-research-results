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
    - Issue: Modify dialog does not support changing the reservation date
    - Actual: The 'Modify Reservation' dialog for an upcoming reservation only exposes 'New Time' and 'Party Size' controls; there is no date field. Time (19:00→20:00) and party size (2→4 Guests) changes saved and are correctly reflected in the reservation list, but the date (August 29, 2026) cannot be changed through this UI.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Review submission is not gated by a completed reservation
    - Actual: The 'Write a Review' form on a restaurant page is freely accessible and submittable with no check that the reviewer has any reservation (completed or otherwise) at that restaurant. A review was successfully submitted for The Golden Fork even though the user's only reservation there had been cancelled (not completed), and there was no prompt, restriction, or link to a specific completed reservation anywhere in the review flow.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (day and first-reservation) are not enforced
    - Actual: Party-size rule for GROUP15 (6+ guests) is enforced correctly ('Coupon Not Applicable - This coupon requires a party of 6 or more.') and only one coupon can be applied at a time. However: (1) WEEKEND20 ('20% off weekend dining') was accepted for Aug 31, 2026, which is a Monday, not a weekend day. (2) WELCOME10 ('10% off your first reservation') was accepted again for a second reservation attempt after the user already had a confirmed reservation with WELCOME10 applied, i.e. it is not actually the user's first reservation.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid field validation is not communicated to the user
    - Actual: Submitting the reservation form with no fields filled, and separately with a valid date/time/party size but an invalid email format ('not-an-email'), silently fails to create the reservation (confirmed via 0 upcoming reservations) but shows no error message, no aria-invalid attributes, and no field highlighting anywhere on the page or via toast/notification region.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is inconsistent with detail page rating and does not update after new reviews
    - Actual: Home page list shows The Golden Fork with a static rating of 4.8, while its detail page showed 4.5 (2 reviews) before any new review was added — an inconsistency even at baseline. After submitting a new 3-star review, the detail page recalculated to 4.0 (3 reviews), but the home page list still displays the unchanged 4.8, showing the list-page summary is not derived from the same review data and does not update when a review is submitted.