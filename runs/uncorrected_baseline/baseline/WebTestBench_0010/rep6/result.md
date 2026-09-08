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
    - Issue: Duplicate review not blocked
    - Actual: Submitted an identical review (same name "Timothee Issenmann", same date, same text) twice in a row on Sakura Garden restaurant; both submissions succeeded, review count increased from 1 to 2 to 3, with no error or duplicate-prevention message. The review form has no link to a specific completed order at all, so the "same order" restriction cannot be enforced.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon rule not re-validated after reservation modification
    - Actual: Single-coupon-at-a-time is enforced (UI replaces input with an applied badge, requiring removal before applying another) and GROUP15 correctly rejected for a 2-guest reservation ("This coupon requires a party of 6 or more"). However, after applying GROUP15 to a 6-guest reservation and then using Modify to reduce party size to 4 (below the 6+ requirement), the system saved the change and kept "Coupon applied: GROUP15" active on the reservation without re-validating or removing the now-invalid coupon.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No price/discounted price displayed
    - Actual: Users can select date/time/party size and apply a coupon, and submission is validated (invalid/inapplicable coupons rejected, confirmation/rejection shown on submit). However, a JS scan of the page (regex for "$" amounts) found zero dollar values anywhere on the reservation form or confirmation - only a percentage-off badge (e.g. "-15% off") is shown, with no subtotal, discount amount, or total price ever calculated or displayed.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Status changes (e.g., cancelling a reservation) do refresh the reservation list in real time and show a transient toast notification (e.g. "Reservation Cancelled"). However, there is no message center or notification history page/icon anywhere in the app - site navigation only has "Restaurants" and "My Reservations" links, and a DOM scan found no "message" or dedicated notification-history UI, only the ephemeral toast region.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No comment/review history in personal center
    - Actual: The personal center (/reservations page) only has "Upcoming" and "Past" tabs showing reservation orders (including a cancelled one), which covers order information. However, there is no section anywhere in the personal center to view the user's submitted reviews/comment history.