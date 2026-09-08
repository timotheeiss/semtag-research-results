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
    - Issue: Invalid zero budget accepted
    - Actual: Entered budget "0" and clicked Save. The dashboard budget changed to "$0", replacing the previously saved $35,000, instead of being rejected.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Invalid negative guest count accepted
    - Actual: Entered guest count "-5" and clicked Save. Dashboard guest count changed to "-5", replacing the previously saved 175, instead of being rejected.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past date accepted as wedding date
    - Actual: Entered date "2025-01-01" (a past date relative to current date 2026-08-25) and clicked Save. Dashboard date changed to "Jan 1, 2025", replacing the previously saved future date "Jun 15, 2027", instead of being rejected. Also the countdown text disappeared entirely.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: View Details action non-functional
    - Actual: Clicked "View Details" on multiple venue cards (Rosewood Garden Estate, Sunset Beach Resort). No modal, dialog, or navigation occurred; the page URL and DOM state remained unchanged, and no dialog element was added. Users cannot open a listing to view any additional/complete details beyond what's already on the card.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancelled draft persists in editor state
    - Actual: After changing Partner One to "Changed" and budget to "99999" then clicking Cancel, the dashboard display correctly retained "Alex & Jordan" and "$30,000". However, reopening the editor afterward showed the cancelled draft values ("Changed" / "99999") instead of the last saved values ("Alex" / "30000"), violating the requirement that reopening should show last saved values rather than the cancelled draft.


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.