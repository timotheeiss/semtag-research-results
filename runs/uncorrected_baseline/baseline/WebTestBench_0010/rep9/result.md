# Test Result

## Functionality
- [X] FT-1: Users can browse the restaurant list and view detailed information for each restaurant, including cuisine type and average price.

- [X] FT-2: Users can search for restaurants by entering a text field.

- [X] FT-3: Users can create reservations for restaurants by selecting the date, time, and number of diners, and then submitting the reservation after confirming the information.

- [X] FT-4: Users can view their upcoming booking list and cancel or modify their bookings within this list.

- [X] FT-5: Users can rate and review the restaurants they have visited. The ratings and reviews will be linked to the corresponding restaurants and displayed correctly.

- [X] FT-6: During the booking process, users can selectively enter discount coupon codes and enjoy corresponding price reductions.

- [X] FT-7: When verifying booking times, the system does not allow users to book past dates and times.

- [X] FT-8: When a user attempts to modify their booking, the system can correctly process the modification request and update the booking information while maintaining the accuracy of the booking information.


## Constraint
- [X] CS-9: Users cannot perform a search without entering any content.

- [ ] CS-10: Users cannot leave comments on the same order repeatedly.
  - Bug Report:
    - Issue: Duplicate review not prevented
    - Actual: Submitted a second review for the same restaurant using the identical reviewer name and identical Date of Visit as the first review. The system accepted it without any warning/blocking: toast said "Review Submitted!" again, review count went from 2 to 3, and rating recalculated (4.5→4.3). No constraint prevents leaving multiple comments tied to the same order/visit.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon rule not re-validated after reservation modification
    - Actual: Applying an invalid/inapplicable coupon is correctly blocked at apply-time (e.g. GROUP15 with party size 2 → "Coupon Not Applicable: This coupon requires a party of 6 or more."; unknown code → "Invalid Coupon"), and only one coupon can be attached at a time (UI replaces the input with a single applied-coupon chip, no way to add a second). However, in an earlier test, a reservation was created with GROUP15 (6 guests, rule: 6+ guests only), then modified down to 4 guests via "Modify" - the system kept "Coupon applied: GROUP15" on the booking without removing it or re-validating that the party size no longer qualifies, violating "coupons must comply with applicable rules".

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time monetary discounted price/total is calculated or displayed
    - Actual: After selecting date/time/party size and applying a coupon, the UI only shows a discount label (e.g. "GROUP15 - 15% off") with no base price, subtotal, or computed total price anywhere on the reservation form, confirmation toast, or reservations list. Restaurants only expose a qualitative price tier ($, $$, $$$, $$$$), so no numeric price is ever calculated in real time as the requirement describes. Submitting does correctly validate info and return a booking result ("Reservation Confirmed!"), but the discounted-price-calculation portion of this requirement is not implemented.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Real-time toast popups do appear when booking status changes (e.g. "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled", "Coupon Applied!"), satisfying the real-time popup portion. However, there is no message center or notification history feature anywhere in the app - the header only contains "TableSpot", "Restaurants", and "My Reservations" links; no bell/notifications icon or history page exists to review past notifications after the toast disappears.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No comment/review history visible in personal center
    - Actual: The "My Reservations" page (personal center) shows Upcoming and Past reservations (applications/completed orders) correctly, e.g. cancelled booking appeared under Past with status "Cancelled". However, there is no section anywhere in "My Reservations" or elsewhere showing the user's own submitted reviews/comment history - reviews written (e.g. on Sakura Garden) are only visible on the individual restaurant's page, not aggregated in a personal center view.