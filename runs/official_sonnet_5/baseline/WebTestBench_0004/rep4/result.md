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
    - Issue: Zero budget is accepted instead of being rejected
    - Actual: Set Budget field to "0" and clicked Save. The dashboard budget updated to "$0" (previously $30,000), replacing the valid saved value instead of rejecting the invalid input.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Negative guest count is accepted instead of being rejected
    - Actual: Set Expected Guests field to "-5" and clicked Save. The dashboard guest count updated to "-5" (previously "200"), replacing the valid saved value instead of rejecting the invalid input.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Current date is accepted as wedding date instead of being rejected
    - Actual: Set Wedding Date field to today's date (2026-08-24, the current date) and clicked Save. The dashboard date updated to "Aug 24, 2026" (previously "Sep 20, 2027"), replacing the valid saved future date instead of rejecting the invalid input. Note the "X days until your special day" countdown text also disappeared.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: Clicking "View Details" on a venue does not open its full details
    - Actual: Clicked "View Details" button on the "Rosewood Garden Estate" venue card (and also tried clicking the venue's heading/card). No modal, dialog, or detail page appeared; the page URL remained /venues and the accessibility tree/DOM showed no dialog element created. No console errors were logged either.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancel does not discard the draft in the editor form
    - Actual: After changing Partner One to "ChangedName" and Budget to "99999" and clicking Cancel, the dashboard display correctly kept the saved values ($30,000, Alex & Jamie). However, reopening the "Update Details" editor showed the previously entered cancelled draft values ("ChangedName" and "99999") instead of the last saved values ("Alex" and "30000").


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.