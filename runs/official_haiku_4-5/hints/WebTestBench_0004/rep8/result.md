# Test Result

## Functionality
- [X] FT-1: Couples can create their locally saved wedding dashboard by entering both partner names, a future wedding date, a positive budget, and a non-negative whole-number guest count; the saved values are shown on the dashboard.

- [X] FT-2: Couples can reopen their wedding details, change any saved partner name, date, budget, or guest count, save the changes, and see the updated values on the dashboard.

- [X] FT-3: Users can filter venues by venue type and price range and vendors by service type and price range, individually or in combination; clearing the criteria restores the full corresponding list.

- [X] FT-4: Keyword search matches relevant venue and vendor information, including names, descriptions, locations, and vendor specialties, without case sensitivity and updates the visible results and count.

- [X] FT-5: Users can add or remove any venue or vendor from their favorites and view the current favorite venues and vendors together in the saved-favorites area.

- [X] FT-6: Users can browse planning checklists by category, open a checklist, and view every task with its explanatory guidance.

- [X] FT-7: Users can save or unsave a whole planning checklist and the dashboard updates its saved-checklist count and preview accordingly.


## Constraint
- [ ] CS-8: The dashboard accepts only a positive numeric wedding budget; zero, negative, empty, or otherwise invalid values are rejected without replacing the previously saved budget.
  - Bug Report:
    - Issue: Budget validation allows zero
    - Actual: Application accepted and saved budget = 0, which is not positive. Expected to reject zero value and retain previous budget of $60,000.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Guest count validation allows negative values
    - Actual: Application accepted and saved guest count = -50, which is negative. Expected to reject negative values and retain previous count of 180.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past date validation fails
    - Actual: Application accepted and saved past date 2026-01-01 (before today 2026-08-25). Expected to reject past dates and retain previous date of Jun 15, 2027.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: Venue/vendor details do not open
    - Actual: Clicked "View Details" button for multiple venues (v1, v2). No details page or modal appeared. URL remained on /venues listing page. Semantic snapshot shows no details view or modal elements.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Form shows cancelled draft instead of last saved values
    - Actual: After changing partner name to "Diana Lee", clicking Cancel, and reopening the dialog, the form shows "Diana Lee" (the cancelled draft) rather than "Alice Chen" (the last saved value). Dashboard correctly retained "Alice Chen & Bob Johnson".


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [ ] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.
  - Bug Report:
    - Issue: Venue and vendor listings missing required content fields
    - Actual: Venues show: name, type, price, rating, capacity. Missing: location and summary. Vendors show: name, service type, price, rating. Missing: location, summary, and offered specialties. Required fields from CT-16 not fully displayed.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.