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
    - Actual: The "Modify Reservation" dialog for an upcoming reservation only offers "New Time" and "Party Size" fields (dialog text: "Modify Reservation\nNew Time\n18:00\nParty Size\n2 Guests\nSave Changes\nClose") — there is no date field. Changed time to 19:00 and party size to 4 Guests, saved successfully and the list reflected both changes immediately, but the date remained unchangeable, so users cannot modify an upcoming reservation's date.


## Constraint
- [ ] CS-10: A review can be submitted only for a restaurant covered by the user's completed reservation, and each completed reservation can authorize at most one review.
  - Bug Report:
    - Issue: Reviews can be submitted without any completed reservation at the restaurant
    - Actual: Navigated to Ember & Smoke restaurant page, which had no prior reservation of any kind by this user. Opened "Write a Review" and submitted a 5-star review with name, visit date, and comment with no prompt to select/verify an eligible completed reservation. The review was accepted immediately (review count went from (1) to (2) and the new review appeared in the list), demonstrating the app does not restrict reviews to restaurants covered by a completed reservation, nor limit one review per completed reservation.

- [ ] CS-11: A coupon is accepted only while all of its stated eligibility conditions are met, including first-reservation, day, and party-size rules, and no reservation can have more than one coupon applied.
  - Bug Report:
    - Issue: Coupon eligibility rules (day and first-reservation) are not enforced
    - Actual: Party-size rule for GROUP15 correctly enforced: rejected silently at 2 guests, accepted with "Coupon Applied! 15% off for parties of 6 or more" at 6 guests. However, WEEKEND20 (stated as weekend-only, "20% off weekend dining") was accepted with a success toast on Aug 27, 2026, which is a Thursday (a weekday, verified via the date picker calendar layout where 23=Sun...29=Sat), violating the day-based eligibility rule. WELCOME10 was also successfully re-applied on a new reservation attempt despite the browsing session already having 2 prior reservations (one active, one cancelled) for this contact/email, indicating no "first-reservation" restriction is enforced. Only the party-size condition works correctly; the day and first-reservation conditions do not.

- [ ] CS-13: A reservation cannot be created without a date, time, guest name, valid email address, and phone number, and the missing or invalid information is identified to the user.
  - Bug Report:
    - Issue: Missing/invalid reservation fields are not identified to the user
    - Actual: Submitting the reservation form with all fields empty produced no reservation and no visible error message (no toast, no field highlighting, no aria-invalid). Submitting with a valid date/time/name/phone but an invalid email ("not-an-email") also silently failed to submit (no navigation, no reservation created, no toast) with no aria-invalid attribute or border/style change on the email field indicating the problem. The form blocks invalid submissions but never communicates why to the user.


## Interaction
- [X] IX-14: For an upcoming reservation, users can open its modification choices and save supported changes, while cancellation requires an explicit confirmation before taking effect.

- [X] IX-15: Creating, modifying, or canceling a reservation produces immediate visible confirmation, and the reservation views reflect the new state in the same session without a manual reload.


## Content
- [X] CT-16: Users can distinguish upcoming reservations from past or cancelled reservations and can view each reservation's restaurant, date, time, party size, status, location, and applied coupon when present.

- [ ] CT-17: A restaurant's displayed rating and review count remain consistent between the restaurant list and its details, and submitting a review updates every displayed summary derived from those reviews.
  - Bug Report:
    - Issue: Restaurant list rating is inconsistent with and does not update alongside the detail page rating
    - Actual: Home page list shows Ember & Smoke rating as 4.7 both before and after a new 5-star review was submitted for it, while the restaurant's detail page shows rating 5.0 (average of its two 5-star reviews) with review count (2). Similarly, The Golden Fork shows 4.8 on the list but 4.5 on its detail page (average of its actual reviews: 5 and 4 stars). The list-page rating is a static value disconnected from the review-derived rating shown on the detail page, and submitting a review does not update the list's displayed rating.