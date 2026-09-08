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
    - Issue: No budget validation; zero and negative budgets are accepted and overwrite the previously saved budget
    - Actual: Saved budget was $55,000. Entering 0 and saving closed the dialog with no error and dashboard showed "$0". Entering -500 and saving showed "$-500". No validation message appeared in either case.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: No guest-count validation; negative and fractional values are accepted and overwrite the saved count
    - Actual: Saved count was 85. Entering -10 and saving produced dashboard guest count "-10" with no error. Entering 12.5 and saving produced "12.5". Neither was rejected.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past wedding dates are accepted and replace the previously saved future date
    - Actual: Saved date was 2027-09-04 ("Sep 4, 2027"). Entering the past date 2020-01-15 and saving closed the dialog without any error, and the dashboard date became "Jan 15, 2020". The countdown element also disappeared entirely.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: "View Details" / "View Profile" buttons are non-functional; no listing detail view can be opened
    - Actual: Clicked venues.grid.item.v1.details ("View Details", action=view-venue-details): URL stayed http://localhost:7004/venues, no dialog/modal in DOM (0 elements matching [role=dialog]/dialog/[aria-modal=true]), and the semantic snapshot was byte-for-byte the grid list with no detail region. Same for vendors.grid.item.vn1.profile ("View Profile"): URL stayed /vendors, 0 dialogs, still 10 grid cards. No detail content is reachable for any listing.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancelled draft is not discarded; editor reopens with the cancelled values instead of last saved values
    - Actual: Saved state was Alicia/Robert/2027-09-04/55000/85. Edited to partnerOne=ZZZ, budget=999, guests=7 then clicked Cancel. Dashboard correctly still showed "Alicia & Robert" / "$55,000" / "85", but reopening the editor showed partnerOne=ZZZ|partnerTwo=Robert|date=2027-09-04|budget=999|guests=7 — the cancelled draft, which would be committed on any later Save.


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.