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
    - Issue: Budget validation does not reject invalid values. Zero and negative values are accepted and saved.
    - Actual: Attempted to save budget value "0" (zero) and "-50000" (negative). Both were accepted and replaced the previously saved budget ($75,000). According to constraint CS-8, these values should be rejected without replacing the saved budget.

- [ ] CS-9: The dashboard accepts only a non-negative whole-number guest count; negative or fractional values are rejected without replacing the previously saved count.
  - Bug Report:
    - Issue: Guest count validation does not reject invalid values. Fractional and negative values are accepted and saved.
    - Actual: Attempted to save guest count values "50.5" (fractional) and "-30" (negative). Both were accepted and replaced the previously saved count (200). According to constraint CS-9, fractional and negative values should be rejected without replacing the saved count. Only non-negative whole numbers should be accepted.

- [X] CS-10: Favorite venues, favorite vendors, and saved checklists remain available after navigating away and after reloading the application.

- [ ] CS-11: A saved wedding date must be a valid future calendar date; current or past dates are rejected without replacing the previously saved date.
  - Bug Report:
    - Issue: Wedding date validation does not reject past or current dates. Only future dates should be accepted.
    - Actual: Attempted to save wedding date values "2020-01-01" (past) and "2026-08-25" (current/today). Both were accepted and replaced the previously saved future date (2027-08-25). According to constraint CS-11, these dates should be rejected without replacing the saved date, as only valid future calendar dates should be accepted.


## Interaction
- [ ] IX-12: Users can open any venue or vendor listing to view its complete details.
  - Bug Report:
    - Issue: Venue and vendor detail views do not appear to open or are not properly implemented.
    - Actual: Clicked on "View Details" button for venue v1 (Rosewood Garden Estate). No details modal or page appeared. The action appears to not be functional or the details view lacks proper semantic markup.

- [X] IX-13: Users can mark and unmark checklist tasks; the checklist percentage updates immediately, aggregate dashboard progress updates accordingly, and completion state remains after reload.

- [X] IX-14: Visible venue or vendor results and their count update immediately as search or filter criteria change, without a separate submit action; clearing all criteria restores the full list.

- [ ] IX-19: If a user changes wedding details and selects Cancel, the saved dashboard values remain unchanged and reopening the editor shows the last saved values rather than the cancelled draft.
  - Bug Report:
    - Issue: Dialog retains cancelled draft values instead of reloading last saved values upon reopening.
    - Actual: Changed Partner One from "Alicia" to "Charlie" and Budget from "60000" to "80000", then clicked Cancel. Dashboard correctly retained saved values ("Alicia & Bob", "$60,000"). However, when reopening the dialog, it showed the cancelled draft values ("Charlie", "80000") instead of reloading the last saved values ("Alicia", "60000").


## Content
- [X] CT-15: After valid wedding details are saved, the dashboard displays the couple's names, formatted wedding date, budget, guest count, planning progress, favorite counts or previews, and saved-checklist counts or previews.

- [ ] CT-16: Each venue and vendor listing presents its name, location, category or service type, price range, rating, and summary; venues also show guest capacity and vendors show offered specialties.
  - Bug Report:
    - Issue: Venue and vendor listings are missing required content fields: location and summary/description for venues; location, summary, and offered specialties for vendors.
    - Actual: Venue listings display: name, type, price range, rating, capacity. Missing: location, summary. Vendor listings display: name, service type, price range, rating. Missing: location, summary, offered specialties. According to CT-16, all these fields should be presented in each listing.

- [X] CT-17: The checklist collection covers planning, ceremony, reception, attire, and honeymoon topics, with at least one checklist per category and a title and explanatory description for every task.

- [X] CT-18: When search and filter criteria are combined, every visible result satisfies all active criteria, the displayed result count is accurate, a clear no-match state appears for zero results, and clearing restores the full list.