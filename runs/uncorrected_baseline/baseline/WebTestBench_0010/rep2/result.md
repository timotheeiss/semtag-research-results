# Test Result

## Functionality
- [X] FT-1: Users can browse the restaurant list and view detailed information for each restaurant, including cuisine type and average price.

- [X] FT-2: Users can search for restaurants by entering a text field.

- [X] FT-3: Users can create reservations for restaurants by selecting the date, time, and number of diners, and then submitting the reservation after confirming the information.

- [X] FT-4: Users can view their upcoming booking list and cancel or modify their bookings within this list.

- [X] FT-5: Users can rate and review the restaurants they have visited. The ratings and reviews will be linked to the corresponding restaurants and displayed correctly.

- [ ] FT-6: During the booking process, users can selectively enter discount coupon codes and enjoy corresponding price reductions.
  - Bug Report:
    - Issue: No actual price reduction is calculated or displayed
    - Actual: Applying coupon GROUP15/WELCOME10 shows a badge like "GROUP15 - 15% off" but the app never displays any dollar/price total anywhere (no subtotal, no total, no discounted total) in the reservation form, confirmation, or reservation list. A DOM scan for "$<number>" patterns found none. Users cannot "enjoy corresponding price reductions" because no price is ever computed or shown.

- [X] FT-7: When verifying booking times, the system does not allow users to book past dates and times.

- [X] FT-8: When a user attempts to modify their booking, the system can correctly process the modification request and update the booking information while maintaining the accuracy of the booking information.


## Constraint
- [X] CS-9: Users cannot perform a search without entering any content.

- [ ] CS-10: Users cannot leave comments on the same order repeatedly.
  - Bug Report:
    - Issue: Duplicate reviews are not prevented
    - Actual: Submitted a second review on Sakura Garden using the identical reviewer name "QA Tester" and identical "Date of Visit" (2026-08-15) right after the first. The system accepted it without any warning/rejection, increasing the review count from (2) to (3), demonstrating a user can comment repeatedly on the same visit/order.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon rules not revalidated after reservation modification
    - Actual: Initial application correctly enforces rules (GROUP15 rejected for party of 2 with message "This coupon requires a party of 6 or more"; only one coupon field/slot exists, preventing combination). However, after applying GROUP15 to a 6-guest reservation, then using Modify to reduce party size to 4 guests (below the 6-guest requirement) and saving, the coupon "GROUP15" remained applied/displayed on the reservation with no re-validation or removal, violating the rule that the coupon must comply with applicable rules.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time price calculation displayed; only partial validation on submit
    - Actual: After selecting date/time/party size and applying a coupon, no real-time discounted price/total is shown anywhere (confirmed via DOM scan for currency amounts - none found). Submitting with required fields (Name/Email) empty is blocked (focus moves to first empty required field) but no explicit price/validity summary is provided before or after submission other than a generic confirmation toast.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center for historical notifications
    - Actual: Real-time toast popups do appear on booking status changes (e.g., "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled", "Coupon Applied!"). However, no message center or notification history feature exists anywhere in the app (checked header nav: only "Restaurants" and "My Reservations" links; no bell icon or notifications page) to view past notifications after they are dismissed.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No comment/review history visible in personal center
    - Actual: "My Reservations" (the personal center) shows Upcoming/Past reservation orders correctly (e.g., a Cancelled reservation appeared under Past). However, there is no way to view a list of reviews/comments the user has submitted from the personal center — reviews are only visible embedded on each individual restaurant's page, not aggregated or attributed to the user's own history.