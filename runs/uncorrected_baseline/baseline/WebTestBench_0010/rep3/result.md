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
    - Issue: No duplicate-review prevention
    - Actual: Submitted a second review for Sakura Garden with identical reviewer name and visit date as an existing review; the system accepted it without any warning or blocking (review count increased from 2 to 3), showing no restriction preventing repeated comments on the same order/visit.

- [X] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time price/discount calculation shown
    - Actual: Throughout the reservation flow (date/time/party size selection, coupon application, and the final reservation confirmation/list), no price, subtotal, or total amount is displayed anywhere; applying a coupon only shows a percentage badge (e.g. "WELCOME10 - 10% off") with no computed discounted price, so the system does not calculate or display a real-time discounted price as required.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Booking status changes (confirm, modify, cancel) do trigger real-time page updates and pop-up toast notifications (e.g. "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled"). However, there is no message/notification center anywhere in the app (only "Restaurants" and "My Reservations" nav links, no bell icon or history log) to view past notifications after the toast disappears.


## Content
- [X] CT-16: Users can view their comment history, applications, and completed order information in their personal center.