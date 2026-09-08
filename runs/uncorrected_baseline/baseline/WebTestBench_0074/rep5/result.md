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
    - Actual: Explored the entire UI (toolbar, Sort menu, column header clicks/right-clicks, Configure Fields dialog) and inspected the DOM/HTML source for any filter control or the word "filter"/"criteria"/"facet"/"where" — none exists. The Sort menu only offers sort-by-field and "Clear sort"; the search box performs full-text search (covered separately by IX-17) but does not support field-specific criteria filtering (e.g. Priority > 2, Completed = true, date range). There is no way to apply a filter to show only entries meeting specified criteria.

- [X] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.


## Constraint
- [X] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [X] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.
  - Bug Report:
    - Issue: Missing required-field validation on Add Entry
    - Actual: Opened Add Entry dialog, left Task Name (a required text field) empty, and submitted. The app accepted the submission and added a new row with an empty Task Name and empty Notes value (Priority defaulted to 0, Due Date defaulted to today). Entry count increased from 6 to 7. Expected: entry should not be addable when a defined field is left blank/undefined.

- [X] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.
  - Bug Report:
    - Issue: No filter feature implemented (dependent on FT-6)
    - Actual: Since no filter feature exists in the application (confirmed via UI exploration and DOM inspection, see FT-6), filter accuracy for including/excluding entries per criteria cannot be verified — there is no mechanism to apply such filters at all.

- [X] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [X] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [X] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.


## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.
  - Bug Report:
    - Issue: No delete confirmation dialog
    - Actual: Clicked the delete (trash) icon button on a table row once; the entry was immediately removed from the table (entry count dropped from 7 to 6) with no confirmation prompt, dialog, or undo option shown at any point.

- [X] IX-17: When a user enters a text field to search, the page displays the search results.

- [X] IX-18: Users can click to sort, and the page will respond promptly.