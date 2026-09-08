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
    - Actual: Submitted an identical review (same name, date of visit, and text) for Trattoria Bella twice in a row; both submissions succeeded ("Review Submitted!" shown each time) and the review count incremented from (1)→(2)→(3) with two duplicate entries, with no restriction or error preventing the repeat submission.

- [ ] CS-11: Discount coupons must be used within their validity period and comply with the applicable rules. Only one basic discount coupon can be used per order; they cannot be combined.
  - Bug Report:
    - Issue: Coupon validity rules not fully enforced
    - Actual: GROUP15 correctly rejected for party size <6 ("This coupon requires a party of 6 or more") and only one coupon can be active at a time (input replaced by applied badge), satisfying the no-combination rule. However, WEEKEND20 (advertised as "20% off weekend dining") was accepted and applied both with no date selected and with a weekday date (Tuesday, Sep 8, 2026) selected — the system does not validate the coupon's weekend-only condition against the chosen reservation date.

- [X] CS-12: Restaurant average prices, opening hours, and other information are maintained by the merchants; users can only view them, not edit them. Reservations cannot exceed the restaurant's maximum capacity per table.

- [ ] CS-13: After selecting the date, time, and number of people, users can select discount coupons, and the system will calculate the discounted price in real time. After clicking submit, the system will verify the validity of the information and provide the booking result.
  - Bug Report:
    - Issue: No real-time discounted price calculation shown
    - Actual: After selecting date/time/party size, coupons can be applied and validity is checked (invalid codes rejected, party-size rule enforced) and submitting without required fields is blocked. However, at no point does the app display an actual computed price/total (only price-tier symbols like "$$$$" and a qualitative "-15% off" badge) — there is no real-time discounted dollar amount calculated or shown before or after applying a coupon.


## Interaction
- [X] IX-14: Users can click on their booking record in their personal center and choose "Modify" or "Cancel". When modifying, users need to re-select information and submit; when canceling, a confirmation pop-up will appear.

- [ ] IX-15: When the booking status changes, the page refreshes in real time and a pop-up notification appears. You can also view historical notifications through the message center.
  - Bug Report:
    - Issue: No message/notification center
    - Actual: Pop-up toast notifications do appear in real time for booking confirm/modify/cancel/review/coupon events. However, there is no message center or notification history page anywhere in the app (nav only offers "Restaurants" and "My Reservations"; a DOM scan found no additional routes/links) to view past notifications after they disappear.


## Content
- [ ] CT-16: Users can view their comment history, applications, and completed order information in their personal center.
  - Bug Report:
    - Issue: Personal center lacks comment history and applications
    - Actual: "My Reservations" (the personal center) only shows Upcoming/Past reservation cards; there is no section listing the user's own submitted reviews/comment history or any distinct "applications" (e.g., coupon usage) list — only booking records are visible.