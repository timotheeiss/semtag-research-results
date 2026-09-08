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
    - Issue: No budget validation; invalid budgets are accepted and overwrite the saved value
    - Actual: Saved budget was $52,000. Entering 0 and clicking Save closed the editor with no error and the dashboard showed "Budget $0". Entering -5000 and saving likewise succeeded, dashboard showed "Budget $-5,000". No validation message appeared in either case and the previously saved budget was replaced.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: No guest-count validation; negative and fractional counts are accepted and overwrite the saved value
    - Actual: Saved guest count was 150. Saving -10 was accepted with no error and dashboard showed "Guest Count -10". Saving 12.7 was also accepted and dashboard showed "Guest Count 12.7". No validation message shown; previously saved count was replaced in both cases.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past wedding dates are accepted; no future-date validation
    - Actual: Saved date was Sep 18, 2027. Entering the past date 2020-01-15 and clicking Save was accepted with no error; the dashboard replaced the saved date and displayed "Wedding Date Jan 15, 2020" (today is 2026-08-28).


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: Listing detail views cannot be opened — the detail buttons are inert for both venues and vendors
    - Actual: Venues: clicking "View Details" (Rosewood Garden Estate) left URL at /venues, added no [role="dialog"] (count 0), no new body portal (children remain DIV#root, SCRIPT), and body innerHTML length was unchanged at 48686. Clicking the card title also did nothing. Vendors: clicking "View Profile" (Eternal Moments Photography) likewise left URL at /vendors, dialogs 0, body portals unchanged, bodyLen 63677 unchanged. No complete-detail view is reachable for any listing.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancelled draft is not discarded from the editor form state
    - Actual: Saved values were Emma Stone / 45000 / 120. Changed to "DRAFT ONE" / 99999 / 555 and clicked Cancel. Dashboard correctly still showed "Emma Stone & Liam Carter", $45,000, 120 (this half passed). But reopening via "Update Details" showed the cancelled draft values in the inputs: ["DRAFT ONE","Liam Carter","2027-06-12","99999","555"] instead of the last saved values.


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.