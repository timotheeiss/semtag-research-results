# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Adding "QA Test Entry Alpha" (Priority 7, Mar 5 2026, Completed=Yes) appeared immediately and count went 5→6 entries. After reloading the page, the table showed only the 5 original seed entries ("6 entries" back to "5 entries") and localStorage was empty ([]), so the new entry was lost.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature exists
    - Actual: The app exposes only Fields, Sort, Table/Cards toggle, Add Entry and a quick-search box. No filter control is present anywhere: full button inventory is Fields, column-header sort buttons, Table, Cards, Add Entry, per-row edit/delete; body text contains no occurrence of "filter"; the only input besides the entry dialog is the "Search entries..." box. Field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability
    - Actual: The Configure Fields dialog only says "Note: Changing field types may affect how existing data is displayed. Configure fields before importing data." but provides no import control. There is no import button, no file input (document.querySelectorAll('input[type=file]').length === 0) and no paste/CSV/JSON UI anywhere in the app, so data cannot be imported or type-validated on import.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so criteria-based inclusion cannot be verified
    - Actual: No filter UI exists in table or card view (no filter button, no per-field criteria controls, no "clear filters" action), so there is no way to apply active criteria or restore data by clearing them.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not the displayed formatted values
    - Actual: Entry "Zebra audit task" displays Due Date "Feb 9, 2026" and Completed "Yes". Searching "Feb 9" returned '0 entries matching "Feb 9"' while searching the raw value "2026-02-09" returned that entry ('1 entry matching "2026-02-09"'). Searching "Yes" returned 0 entries even though three entries display Completed = Yes.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.