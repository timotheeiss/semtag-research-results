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
    - Issue: Duplicate review submission is not prevented
    - Actual: Submitted a second review for Sakura Garden using the exact same reviewer name ("Timothee Issenmann") and same visit date (2026-08-15) as an already-submitted review. The system accepted it without any warning or blocking, increasing the review count from (2) to (3) and adding a duplicate/near-duplicate entry. No mechanism ties reviews to a specific completed order/reservation to prevent repeat commenting on the same order.

- [X] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time discounted price calculation is displayed
    - Actual: The reservation form lets users pick date/time/party size and apply a coupon (which shows a toast and a "-X% off" badge), and submission is correctly validated (missing required Name/Email/Phone blocks submission and focuses the first invalid field; confirmation/booking result is shown for valid submissions). However, nowhere in the flow is an actual price total or discounted price amount computed/displayed in real time — there is no subtotal, discount amount, or final total shown before or after applying a coupon, only the restaurant's general "$"-tier symbol and the coupon's percentage label.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message center to view historical notifications
    - Actual: Booking status changes (create/modify/cancel reservation, apply coupon, submit review) do trigger real-time pop-up toast notifications (e.g., "Reservation Confirmed!", "Reservation Updated", "Reservation Cancelled"). However, the site's header navigation only offers "Restaurants" and "My Reservations" links — there is no notification bell, message center, or any page to view a history of past notifications. Once a toast disappears, there is no way to retrieve it.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: No personal center with comment history
    - Actual: The only account-related page in the app is "/reservations" ("My Reservations"), which shows Upcoming/Past reservation cards (including cancelled ones) — this covers order/booking info but there is no personal-center section showing the user's own comment/review history (reviews the user submitted are only visible mixed into each restaurant's public Reviews tab, not aggregated anywhere for the user) and no "applications" section. The site header offers only "Restaurants" and "My Reservations" links, with no profile/account/personal-center page.