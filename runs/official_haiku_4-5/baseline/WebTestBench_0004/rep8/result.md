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
    - Issue: Budget validation not enforced - zero value was accepted
    - Actual: Entered budget 0 and it was saved; dashboard now shows Budget: $0. Expected: should reject zero and keep previous value $100,000.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Guest count validation not enforced - negative value was accepted
    - Actual: Entered guest count -50 and it was saved; dashboard now shows Guest Count: "-50". Expected: should reject negative value and keep previous value 250.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Date validation not enforced - past date was accepted
    - Actual: Entered past date 2026-08-20 (current date is 2026-08-25) and it was saved; dashboard now shows "Aug 20, 2026". Expected: should reject past date and keep previous value 2026-12-25.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: View Details button does not open venue/vendor details
    - Actual: Clicked "View Details" button for "Rosewood Garden Estate" venue multiple times. Button registers click (shows [active] state) but does not navigate to details page or open a modal. Page URL remains /venues and no details view is displayed.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Reopening editor shows cancelled draft values, not last saved values
    - Actual: Changed Partner One to "Diana Prince" and clicked Cancel. Dashboard correctly remained "Carol Davis & Bob Smith" (last saved). But reopening the editor showed "Diana Prince" (the cancelled draft) instead of "Carol Davis" (the last saved value).


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.