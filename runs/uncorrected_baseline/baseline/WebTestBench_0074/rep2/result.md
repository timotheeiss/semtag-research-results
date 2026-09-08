# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No dedicated filter feature exists
    - Actual: Searched the entire header toolbar and full page DOM (case-insensitive scan for "filter") — only Search, Sort, Table/Cards toggle, and Add Entry controls exist. There is no UI to apply field-specific filter criteria (e.g., show only Completed=Yes, or Priority>2); the search box only performs a text substring match across fields, which is a separate feature (already tested as IX-17), not a criteria-based filter.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Entry can be added even when no fields are defined
    - Actual: Deleted all fields via Fields dialog (Task Name, Priority, Due Date, Completed, Notes), leaving zero fields defined. Opened "Add New Entry" dialog — it showed no input fields but the "Add Entry" button was still enabled. Clicking it successfully created a new (empty) entry, increasing entry count from 5 to 6, instead of being blocked/disabled.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: Filter feature does not exist to evaluate
    - Actual: Since no dedicated filter UI/functionality is present in the app (confirmed via DOM scan), there is no mechanism to test criteria-based include/exclude filtering; the constraint cannot be satisfied because the feature is absent.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation dialog appears
    - Actual: Clicked the delete (trash) icon button on the "Test Entry Alpha" row; the entry was immediately removed from the table with no confirmation prompt/dialog — entry count dropped from 6 to 5 instantly in the same action.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.