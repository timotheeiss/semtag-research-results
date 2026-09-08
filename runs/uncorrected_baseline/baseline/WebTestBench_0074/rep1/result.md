# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No filter feature exists in the application
    - Actual: Exhaustively inspected the UI (banner, toolbar, Sort menu, Fields dialog) and the full page DOM (searched for the string "filter" case-insensitively) — no filter control, button, or dropdown exists anywhere. Only Sort (by field, whole-list reordering) and a free-text Search box are available; there is no way to apply field-specific inclusion/exclusion criteria (e.g. Priority > 2, Completed = true).

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Opened Add Entry dialog, left Task Name (text field) empty, and clicked "Add Entry" without filling any other custom data. The entry was added successfully with an empty Task Name (row showed blank name, Priority 0, today's date, unchecked, empty Notes) and entry count incremented from 6 to 7. No validation error or block occurred, so an entry with an undefined field was allowed to be added, violating the requirement that field configuration/definition must be complete for an entry to be added.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature exists to evaluate accuracy against
    - Actual: Since no dedicated filter feature exists in the app (see FT-6), filter accuracy/data-loss cannot be verified. The only narrowing mechanism is the free-text Search box, which is a separate feature (IX-17) and does not support field-specific include/exclude criteria.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation prompt
    - Actual: Clicked the delete (trash) button on two different entry rows; both times the entry was removed immediately from the table with no confirmation dialog, alert, or "Are you sure" prompt appearing at any point. Entry count decremented instantly (7→6 and 6→5) without any user confirmation step.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.