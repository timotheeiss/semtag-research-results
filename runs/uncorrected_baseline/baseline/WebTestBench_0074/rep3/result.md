# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No filter feature to display only entries matching specified criteria
    - Actual: Thorough inspection of the toolbar, Sort menu, and DOM (no button/element with 'filter' in its text, aria-label, class, or title anywhere in the app) shows the app only offers Sort (by field) and a free-text Search box. The Search box performs plain substring matching only against text-type field values (e.g., searching 'Yes' to filter Completed=Yes entries returned 0 results even though 3 entries have Completed=Yes), so it cannot filter by field criteria such as checkbox state, numeric range, or date range. There is no dedicated filter UI (e.g., 'show only Completed=Yes', 'Priority > 2', etc.).

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Entry with an undefined/blank required field was allowed to be added
    - Actual: Opened Add Entry dialog and left the Task Name field completely empty, then clicked 'Add Entry'. The app did not block submission or show a validation error; instead it created a new entry (entry count went from 6 to 7) with a blank/undefined Task Name (rendered with no heading text in Card view, unlike all other entries which show their task name as a heading).

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter mechanism exists to verify inclusion/exclusion accuracy
    - Actual: Since no dedicated filter-by-criteria feature exists in the app (only sort and free-text search, confirmed via DOM inspection), there is no way to apply field-based filters, so accurate inclusion/exclusion of entries per criteria cannot be validated or exercised.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No confirmation prompt shown before deleting an entry
    - Actual: Created a test entry 'Delete Test Entry' and clicked its delete (trash) icon. The entry was immediately removed (entry count dropped from 7 to 6) in the same action with no confirmation dialog, popup, or 'Are you sure?' prompt appearing at any point — verified via full-page snapshot taken right after the click.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.