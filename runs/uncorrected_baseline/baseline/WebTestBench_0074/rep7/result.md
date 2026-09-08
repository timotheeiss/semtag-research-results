# Test Result

## Functionality
- [X] FT-1: Users can add new entries to the database.

- [X] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [X] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [X] FT-4: Users can switch between table layout and card grid layout view modes.

- [X] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.
  - Bug Report:
    - Issue: Missing filter feature
    - Actual: No filter UI exists anywhere in the app. Searched the full header/toolbar (Fields, Search box, Sort, Table/Cards toggle, Add Entry) and DOM (no elements with 'filter' in class/id/aria-label/title/data-testid), and right-clicked column headers - no filter control or criteria-based filtering mechanism was found. Only a free-text search box exists, which does not let users filter by specific field criteria (e.g., checkbox status, numeric/date range).

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Opened "Add New Entry" dialog, left the Task Name field completely blank (undefined), and clicked "Add Entry". The entry was still added successfully (entry count went from 6 to 7) with an empty Task Name cell (verified via DOM: cell text was ""), Priority "0", Score "0". The app does not prevent adding an entry when a field's value is left undefined/blank, contradicting the requirement that "an entry cannot be added" if at least one field is not defined.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: Missing filter feature
    - Actual: Since no filter functionality is implemented in the application (see FT-6), it is impossible to verify that filters accurately include/exclude entries without data loss - the feature does not exist to test.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation dialog
    - Actual: Clicked the delete (trash) icon button on a table row. The entry was removed immediately (entry count dropped from 7 to 6) with no confirmation dialog, prompt, or undo option appearing at any point - contradicting the requirement that a confirmation button/dialog should appear before deletion to prevent accidental deletes.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.