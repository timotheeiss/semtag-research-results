# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: No filter feature present
    - Actual: Inspected the entire UI (header controls, table column headers, card view) and the full DOM for any filter control (dropdown, criteria builder, per-field filter icon, etc.). Only a free-text Search box and a Sort menu exist; there is no way to apply a filter (e.g., by numeric range, checkbox status, or exact field value) distinct from the plain text search.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Clicked "Add Entry" with the Task Name field left blank (all other fields at default values). The app accepted the submission and created a new row with an empty Task Name cell instead of blocking submission, so an entry was added despite a field being undefined.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature to verify accuracy
    - Actual: Since no filter control exists in the application (only text search and sort), filter accuracy/inclusion-exclusion behavior described in CS-12 cannot be exercised or verified; the requirement is unimplemented.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation dialog
    - Actual: Clicked the delete (trash) icon on a table row. The entry was removed immediately (entry count dropped from 6 to 5) with no confirmation dialog or button appearing to confirm the deletion.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.