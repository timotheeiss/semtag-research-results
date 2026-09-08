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
    - Issue: No budget validation; zero and negative budgets are accepted and overwrite the previously saved budget.
    - Actual: With $52,500 saved, entering budget 0 and saving closed the dialog with no error and the dashboard showed "$0". Then entering -5000 and saving showed "$-5,000" on the dashboard. Both invalid values replaced the previously saved budget.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: No guest count validation; negative and fractional guest counts are accepted and overwrite the saved count.
    - Actual: With 165 saved, entering guest count -20 and saving closed the dialog with no error and dashboard showed "-20". Entering 12.5 and saving showed "12.5". Both invalid values replaced the previously saved count.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past wedding dates are accepted and replace the previously saved future date.
    - Actual: With "Sep 4, 2027" saved, entering 2020-03-15 (past) and saving closed the dialog with no error; dashboard shows "Mar 15, 2020" and the countdown element disappeared entirely.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: "View Details" / "View Profile" buttons are inert — no detail view can be opened for any venue or vendor.
    - Actual: Clicking venues.grid.item.v3.details (The Grand Ballroom) left the page identical — full accessibility snapshot showed no modal, no route change, no expanded content. Clicking vendors.grid.item.vn1.profile likewise produced 0 dialog elements, URL still /vendors and unchanged body content.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancelled draft is not discarded; editor reopens with abandoned edits instead of last saved values.
    - Actual: Saved values were Alicia Bennett / 52500 / 165. Edited to "DISCARD ME" / 999999 / 7 and clicked Cancel. Dashboard correctly stayed at "Alicia Bennett & Marc Dubois", "$52,500", "165", but reopening the editor showed partnerOne="DISCARD ME", budget="999999", guests="7" — the cancelled draft, not the saved values.


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.