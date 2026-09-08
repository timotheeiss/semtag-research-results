# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No dedicated filter feature to display entries by specified field criteria
    - Actual: Inspected the entire UI (banner, toolbar, column headers, DOM for inputs/selects/buttons with "filter" in label or content) and found no filter control besides the free-text "Search entries..." box (already tested as IX-17) and per-column Sort buttons. There is no way to specify structured criteria (e.g., Completed = Yes, Priority > 2, date range) to filter entries.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Clicked "Add Entry" then immediately clicked the submit "Add Entry" button without filling Task Name or Notes (leaving them blank/undefined). The entry was still created and added to the table (entries count went from 5 to 6) with an empty Task Name and empty Notes, instead of being blocked.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature exists to verify accuracy against
    - Actual: Same root cause as FT-6: there is no dedicated filter-by-criteria mechanism in the app (only free-text search and sort). Since no filter feature exists, its accuracy/data-safety requirement cannot be met.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation prompt
    - Actual: Clicked the delete (trash) icon button on a table row once; the row was deleted immediately (entries count went 6 to 5) with no confirmation dialog or button shown beforehand.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.