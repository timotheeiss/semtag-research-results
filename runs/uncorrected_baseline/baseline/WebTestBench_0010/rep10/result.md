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
    - Issue: Duplicate reviews are not prevented
    - Actual: Submitted an identical review (same reviewer name 'Timothee I.', same visit date, same review text, same rating) twice in a row for the same restaurant. Both submissions succeeded with 'Review Submitted!' toast; the review count incremented each time (2->3->4) and both duplicate entries appear in the Reviews list. The system does not tie reviews to a specific completed order/reservation nor block resubmission for the same order, allowing repeated comments.

- [X] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time price/total calculation shown
    - Actual: After selecting date, time, party size and applying a coupon, the reservation form never displays any monetary subtotal, discount amount, or total price anywhere (checked via DOM text search for 'total' - not found). Only a percentage badge like '- 15% off' is shown, not an actual calculated discounted price. Booking submission does work and returns a confirmation result, but the 'system will calculate the discounted price in real time' requirement is not met since no price is ever computed/displayed.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message center for historical notifications
    - Actual: Booking status changes (e.g., cancelling a reservation) do trigger a real-time page update (tab counts change Upcoming 1->0, Past 0->1 instantly) plus a pop-up toast notification ('Reservation Cancelled'), satisfying the first half of this requirement. However, there is no message/notification center anywhere in the app (header only contains 'Restaurants' and 'My Reservations' links, no bell icon or notifications page) to view historical notifications after the toast disappears.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No comment/review history visible in personal center
    - Actual: The /reservations personal-center page shows Upcoming and Past reservation tabs with full order details (restaurant, date, time, party size, coupon, status), satisfying the 'applications and completed order information' part. However, there is no section, tab, or link anywhere in the personal center (or navigation) that shows the user's submitted review/comment history, even after submitting two reviews earlier in this session.