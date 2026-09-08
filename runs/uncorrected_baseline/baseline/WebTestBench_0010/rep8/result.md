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
- [ ] CS-9: Users cannot perform a search without entering any content.
  - Bug Report:
    - Issue: Empty search is not restricted
    - Actual: Clearing the search box (empty content) simply displays the full unfiltered list of "6 restaurants available" with no validation, warning, or restriction preventing a contentless search from being performed.

- [ ] CS-10: Users cannot leave comments on the same order repeatedly.
  - Bug Report:
    - Issue: Duplicate reviews for the same visit/order are allowed
    - Actual: Submitted two separate reviews for Sakura Garden using the identical reviewer name ("Timothee Issenmann") and identical Date of Visit ("2026-08-15"); both were accepted without any duplicate-check error, review count went from 1 -> 2 -> 3 with both entries visible in the Reviews tab.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Applied coupon is not re-validated against rule changes
    - Actual: Coupon application itself is correctly gated (invalid codes rejected with "Invalid Coupon" toast; GROUP15 correctly rejected for party <6 with "Coupon Not Applicable - This coupon requires a party of 6 or more"; only one coupon field/badge exists at a time so codes cannot be combined). However, after booking with GROUP15 at 6 guests and then using Modify to reduce party size to 4 guests, the reservation still displayed "Coupon applied: GROUP15" without any re-validation or removal, violating the rule that GROUP15 requires 6+ guests.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time discounted price/total is calculated or displayed
    - Actual: Coupon selection occurs after date/time/party-size selection, and submit does validate coupon rules (see CS-11) and returns a booking result. However, a full DOM text scan of the reservation form and confirmation screens found no numeric price/total field anywhere ("$[0-9]" regex matched nothing) - only a discount percentage badge (e.g. "-15% off") is shown, so no actual discounted price is calculated or displayed in real time.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Pop-up toast notifications do appear in real time for status changes (e.g. "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled") and the page updates without a full reload. However, the app has no message center: the header DOM only contains a logo, "Restaurants" and "My Reservations" nav links, and a mobile menu toggle - there is no bell icon, notifications page, or any route besides "/" and "/reservations" to view historical notifications.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No unified personal center for comment history / applications
    - Actual: The only account-related page is /reservations, which shows Upcoming/Past reservations (completed order info) with coupon info. There is no view anywhere in the app listing the user's submitted reviews/comments or "applications" - reviews the user wrote are only visible mixed into each restaurant's public Reviews tab, not aggregated in a personal center.