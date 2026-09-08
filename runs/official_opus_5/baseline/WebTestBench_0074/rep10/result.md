# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Creating entry "Zebra QA task" (Priority 7, Mar 9 2026, Completed checked) appeared immediately as a 6th row. After reloading http://localhost:6074/, the table showed only the 5 original seed rows and localStorage was empty (Object.keys(localStorage) === []).

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filtering feature exists
    - Actual: The app exposes only Fields, Sort, Table/Cards, Add Entry and a single free-text "Search entries..." box. Column headers contain only sort buttons (no filter menu/icon), the Sort menu has no filter entries, and the only input in the whole page is the search box (document.querySelectorAll('input,select,textarea') returns just the search input). There is no way to define per-field criteria or to clear filters.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability exists
    - Actual: The app has no import control: toolbar offers only Fields, Sort, Table, Cards, Add Entry; the Configure Fields dialog contains only field rows, an Add-field row, a note and Close; document.querySelectorAll('input[type=file]').length === 0 and the only occurrence of the word "import" on the page is the static note "Configure fields before importing data." Therefore importing after defining a schema, and rejection of type-mismatched imported values, cannot be performed.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so criteria-based inclusion cannot be satisfied
    - Actual: No filter criteria UI exists anywhere in the app (no filter controls in toolbar, column headers, Sort menu or Fields dialog), so entries cannot be filtered by criteria nor can filters be cleared.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not the formatted values shown to users
    - Actual: Searching "Feb 28" (the displayed Due Date text "Feb 28, 2026") returned "0 entries matching", while searching the raw value "2026-02-28" returned 1 entry (Alpha task). Searching "Yes" (the checkbox state shown in card view, and green check in table) returned "0 entries matching" although two entries display Completed = Yes.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.