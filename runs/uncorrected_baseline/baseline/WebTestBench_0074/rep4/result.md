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
    - Actual: Explored the entire UI (toolbar, column headers, Fields dialog, search box) and found no dedicated filtering mechanism to show entries matching specific field criteria (e.g., Priority > X, Completed = true). The word "filter" does not appear anywhere in the page DOM. Only a free-text search box (matches any field as substring) and column sorting are available; neither allows constraining results by specific field criteria/values as a filter would.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required field validation not enforced
    - Actual: Opened Add Entry dialog, left Task Name (text field) empty, and clicked "Add Entry". The entry was added successfully to the table with a blank Task Name cell instead of being blocked. Entry count went from 5 to 6.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature to verify
    - Actual: Since no filter feature exists in the application (see FT-6), there is no mechanism to test criteria-based include/exclude filtering accuracy or verify it preserves data without loss.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation dialog
    - Actual: Clicked the delete (trash) button on a row once; the entry was removed immediately (entry count went from 6 to 5) with no confirmation prompt/dialog appearing.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.