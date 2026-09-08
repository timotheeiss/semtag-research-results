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
    - Actual: Submitted a second review for the same restaurant using the identical reviewer name and date of visit as a review just submitted moments before. The system accepted it without any warning/blocking, incrementing review count from 2 to 3 (toast: "Review Submitted!"). There is no mechanism linking reviews to a specific completed order/reservation, so repeated reviews on the same order/visit are not prevented.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon rule re-validation missing after reservation modification
    - Actual: Applying GROUP15 to a 2-guest party correctly shows "Coupon Not Applicable - This coupon requires a party of 6 or more", and an invalid code shows "Invalid Coupon"; only one coupon can be applied at a time (new apply replaces old). However, when an existing reservation with GROUP15 (6-guest requirement) applied was modified via "Modify" to reduce party size to 4 guests, the system did NOT re-validate/remove the coupon: the reservation list continued to display "Coupon applied: GROUP15" for a 4-guest party, violating the "must comply with applicable rules" constraint.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time discounted price displayed
    - Actual: Users can select date/time/party size and then apply a coupon, and clicking Confirm Reservation does validate info and return a booking result (toast + confirmation screen) - this part works. However, the system never displays any actual price or total anywhere in the reservation flow (before or after applying a coupon) - only a percentage badge like "GROUP15 - 15% off" is shown, with no dollar subtotal/discount/total. DOM scan confirmed no "Total"/"Subtotal" text and no numeric dollar amounts on the page. This does not satisfy "the system will calculate the discounted price in real time."


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message center for historical notifications
    - Actual: Pop-up toast notifications do appear in real time when booking status changes (e.g., "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled"), satisfying the real-time pop-up requirement. However, the app has no message/notification center anywhere in the UI (checked header/nav: only "TableSpot" logo, "Restaurants", "My Reservations" links, no bell/notifications icon or history page) to view historical notifications after the toast disappears.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No consolidated personal center with comment history
    - Actual: The only account-like page is "My Reservations" (Upcoming/Past tabs), which shows completed/cancelled order (reservation) information. There is no section or page showing the user's own comment/review history (reviews they've submitted across restaurants) or "applications" (e.g., coupon usage history) - reviews are only viewable per-restaurant on that restaurant's detail page, not aggregated in a personal center.