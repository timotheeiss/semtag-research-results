# Test Checklist

## Functionality
- [ ] FT-1: Users can add new entries to the database.

- [ ] FT-2: Users can create different types of fields: text, number, date, and checkbox.

- [ ] FT-3: Users can input and save data for each field type (text string, number, date value, checkbox status).

- [ ] FT-4: Users can switch between table layout and card grid layout view modes.

- [ ] FT-5: Users can sort entries by any field in the database.

- [ ] FT-6: Users can apply filters to display only entries that meet specified criteria.

- [ ] FT-7: Users can manually adjust the order of items, and the result after adjustment will be consistent with the result of manual operation.

## Constraint
- [ ] CS-8: Numeric fields must accept only valid numeric input and reject non-numeric characters.

- [ ] CS-9: Date fields must accept only valid date formats and reject invalid dates.

- [ ] CS-10: At least one field is not defined in the database, so an entry cannot be added.

- [ ] CS-11: The sorting process must maintain data integrity; no input data may be modified or lost.

- [ ] CS-12: Filters must accurately include/exclude entries according to specified criteria, and must not lose data.

- [ ] CS-13: All entries and their field values ​​must be displayed completely and accurately in both table view and card grid view.

- [ ] CS-14: In both view modes, field labels and types must be clearly distinguishable.

- [ ] CS-15: The checkbox status must be visually distinguishable as either selected or unselected.

## Interaction
- [ ] IX-16: When deleting a data entry, a confirmation button will pop up to prevent users from accidentally deleting it.

- [ ] IX-17: When a user enters a text field to search, the page displays the search results.

- [ ] IX-18: Users can click to sort, and the page will respond promptly.