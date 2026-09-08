# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entries not persisted to storage
    - Actual: Created "Test Entry 1" entry with values. Entry appeared in table immediately (6 entries shown). After page reload, entry disappeared and count returned to 5 entries. Data is not persisted between page loads.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filter UI not found in application
    - Actual: No filter-related UI elements found in the application. Searched DOM for filter buttons, menus, and text - returned 0 results. Only sorting, search, and view switching controls are present. Filter feature appears to not be implemented.

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering drag not functioning correctly
    - Actual: Attempted to drag first entry to different positions. Drag handle buttons are present and accept drag operations (status shows "Draggable item entry-1 was dropped over droppable area entry-1"), but entry order does not change. Entry remains in original position after drop.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import UI not found in application
    - Actual: No import-related UI elements found in the application. Searched DOM for import buttons, menus, and text - returned 0 results. Only Add Entry, Fields, Sort, Search, and view switching controls are present. Import feature appears to not be implemented.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature not implemented
    - Actual: No filter UI found in application, so filtering cannot be tested. Filtering feature is not implemented. Only search functionality is available, which is different from filtering by field-specific criteria as required by CS-12.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [X] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.