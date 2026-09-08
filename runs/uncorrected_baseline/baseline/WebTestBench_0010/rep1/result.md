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
    - Issue: Duplicate review submission not prevented
    - Actual: Submitted the identical review (same name "Timothee Issenmann", same date of visit 2026-08-15, same review text, same rating) twice for the same restaurant/order. Both submissions succeeded with "Review Submitted!" and the review count incremented each time (1->2->3), with no error or duplicate-detection message.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon validity rule not re-enforced after order modification
    - Actual: GROUP15 coupon (requires party of 6+) was correctly rejected when initially applying it to a 2-guest reservation ("This coupon requires a party of 6 or more"). However, after booking with 6 guests + GROUP15 applied, then using Modify to reduce party size to 4 guests and saving, the reservation still displayed "Coupon applied: GROUP15" with no re-validation, removal, or warning, violating the coupon's applicable-rules constraint. (Combining multiple coupons was correctly prevented by the UI, which only allows one applied coupon at a time.)

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time discounted price calculation/display
    - Actual: After selecting date, time, party size and applying a coupon, the reservation form only shows the coupon name and discount percentage (e.g. "GROUP15 - 15% off") but never displays any actual price, subtotal, or discounted total anywhere in the booking flow or in the confirmed reservation/My Reservations view. There is no real-time price calculation visible to the user. Field-level validation on submit does work (empty required fields blocked submission and focused the Name field), and invalid coupon codes are correctly rejected.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Booking status changes (create, modify, cancel) do trigger transient toast pop-up notifications (e.g. "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled") and the reservations list refreshes in real time. However, there is no persistent message/notification center anywhere in the app (no bell icon or notifications page found via DOM scan of aria-labels/titles across the site) to view historical notifications after the toast disappears.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No personal center section for comment/review history
    - Actual: The only personal-center-like page is /reservations, which shows Upcoming and Past reservation tabs (applications/completed orders) but has no section listing the user's submitted reviews/ratings ("comment history"). Reviews are only viewable per-restaurant on that restaurant's detail page, not aggregated anywhere in the user's personal center.