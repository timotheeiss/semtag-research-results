# Test Result

## Functionality
- [X] FT-1: Couples can create their locally saved wedding dashboard by entering both partner names, a future wedding date, a positive budget, and a non-negative whole-number guest count; the saved values are shown on the dashboard.

- [X] FT-2: Couples can reopen their wedding details, change any saved partner name, date, budget, or guest count, save the changes, and see the updated values on the dashboard.

- [X] FT-3: Users can filter venues by venue type and price range and vendors by service type and price range, individually or in combination; clearing the criteria restores the full corresponding list.

- [ ] FT-4: Keyword search matches relevant venue and vendor information, including names, descriptions, locations, and vendor specialties, without case sensitivity and updates the visible results and count.
  - Bug Report:
    - Issue: Vendor search returns irrelevant results not matching the search term in any visible field
    - Actual: Searching vendors for 'photo' (and 'PHOTO', case-insensitively) correctly returned 'Eternal Moments Photography' but also incorrectly returned 'DJ Spark Entertainment', whose name, location (Atlanta, GA), description ('High-energy DJ services...'), service type (Music & DJ), and specialties (DJ Services, Lighting, MC Services) contain no occurrence of 'photo'. Venue keyword search (e.g. 'GARDEN') worked correctly and case-insensitively, but vendor search has false-positive matches.

- [X] FT-5: Users can add or remove any venue or vendor from their favorites and view the current favorite venues and vendors together in the saved-favorites area.

- [X] FT-6: Users can browse planning checklists by category, open a checklist, and view every task with its explanatory guidance.

- [X] FT-7: Users can save or unsave a whole planning checklist and the dashboard updates its saved-checklist count and preview accordingly.


## Constraint
- [ ] CS-8: The dashboard accepts only a positive numeric wedding budget; zero, negative, empty, or otherwise invalid values are rejected without replacing the previously saved budget.
  - Bug Report:
    - Issue: Invalid budget (zero) accepted and replaced saved value
    - Actual: Entered budget '0' and saved; dashboard budget updated to $0, replacing the previously saved $45,000. Expected zero to be rejected without changing the saved budget.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Invalid guest count (negative) accepted and replaced saved value
    - Actual: Entered guest count '-5' and saved; dashboard guest count updated to -5, replacing the previously saved 200. Expected negative value to be rejected without changing the saved count.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Past date accepted and replaced saved date
    - Actual: Entered wedding date '2020-01-01' (a past date) and saved; dashboard date updated to Jan 1, 2020, replacing the previously saved Sep 20, 2027. Expected past date to be rejected without changing the saved date.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: View Details/View Profile buttons do not open any detail view
    - Actual: Clicking 'View Details' on venue 'Rosewood Garden Estate' and 'View Profile' on vendor 'Eternal Moments Photography' produced no navigation, no modal, and no additional content beyond the card summary already shown in the grid. No console errors were logged. Users cannot access any 'complete details' beyond what is already visible on the listing card.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Cancelled draft persists in editor form state
    - Actual: After changing partner name to 'CancelledName' and budget to 99999 then clicking Cancel, the dashboard correctly kept saved values (Alicia/Robert, $45,000). However, reopening the editor again showed the cancelled draft values ('CancelledName', 99999) instead of the last saved values (Alicia, $45,000).


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [X] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [ ] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.
  - Bug Report:
    - Issue: Combined search+filter can show a result that does not satisfy the active search criterion
    - Actual: With vendor filter 'Music & DJ' + search 'photo', the result set showed 'DJ Spark Entertainment' as the sole match (1 of 10), but this vendor's name, location, description, and specialties contain no occurrence of 'photo', violating the requirement that every visible result satisfy all active criteria. No-match state and accurate counts were otherwise verified correctly on venues (searching 'vineyard' with Beach filter showed 'Showing 0 of 8 venues' and 'No venues match your search.').