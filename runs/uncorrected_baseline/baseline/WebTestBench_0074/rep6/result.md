# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No filter feature implemented
    - Actual: Inspected the full toolbar (Search, Sort, Table/Cards toggle, Add Entry) and DOM (searched entire body innerHTML for the word "filter" — zero matches). No dedicated filter control (e.g., filter by field value/range, checkbox status, date range) exists anywhere in the app; only a free-text search bar and column sort are available. Users cannot apply criteria-based filters to display only matching entries.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing validation for undefined/empty required fields when adding an entry
    - Actual: Opened "Add New Entry" dialog and clicked "Add Entry" immediately without filling the Task Name field (left blank) — the app still created a new entry (entries count went from 5 to 6) with an empty Task Name, default Priority "0", today's date, unchecked Completed, and Budget "0". No validation error or block occurred, so an entry with an undefined field was successfully added, contradicting the expected constraint.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature exists to evaluate
    - Actual: Since no dedicated filter functionality is implemented anywhere in the app (confirmed via full toolbar inspection and DOM search), there is no filter mechanism whose accuracy or data integrity could be verified. The requirement cannot be satisfied because the feature is absent.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No confirmation prompt shown before deleting an entry
    - Actual: Clicked the delete (trash) icon button on a table row. The entry was immediately removed (entry count dropped from 6 to 5) with no confirmation dialog, popup, or "Are you sure?" prompt appearing at any point.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.