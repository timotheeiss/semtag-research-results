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
    - Actual: Explored the entire UI (toolbar, column headers, Fields dialog, DOM search for "filter") and found no dedicated filter control to display only entries matching specified criteria (e.g., filter by field value/range). Only a free-text Search box (matches any field) and column-based Sort exist; there is no way to apply criteria-based filters such as "Priority > 2" or "Completed = Yes only".

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Submitted the Add Entry form with the Task Name (text) and Notes fields left blank; the entry was still added successfully to the table (row shows empty Task Name, Priority 0, today's date, empty Notes) instead of being blocked with a validation message.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: Filter feature absent, cannot verify accurate inclusion/exclusion
    - Actual: Since no filter UI exists in the application (confirmed via UI inspection and DOM search), it is impossible to apply or verify criteria-based filtering. This is a dependent failure of FT-6.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation prompt
    - Actual: Clicking the trash/delete icon on an entry row immediately deleted the entry (count dropped from 7 to 6) with no confirmation dialog, button, or any prompt shown to prevent accidental deletion.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.