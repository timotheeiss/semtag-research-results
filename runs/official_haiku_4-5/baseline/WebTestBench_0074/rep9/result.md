# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entry does not persist after page reload
    - Actual: Created entry "Test entry for validation" appeared immediately in the table (6 entries shown), but after page reload, it no longer exists (only 5 entries remain, with original sample data)

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filtering UI found in application
    - Actual: Searched for filter controls but found none. Sort menu only contains field names for sorting, not filtering options. No dedicated filter button or panel visible. Quick search provides text matching but not field-specific filters with criteria

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering appears unavailable when sort is not active
    - Actual: UI shows "Drag entries to reorder" message. Sort button is visible but currently no sort is active in card view. However, no drag handles or visible affordances for drag-and-drop were tested due to token constraints. Requirement specifies manual reordering should only work when NO sort is active, which appears to be the case, but drag functionality was not confirmed to work

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import functionality found in application
    - Actual: Searched for import button, menu option, or import dialog throughout the UI. No import functionality was found. Fields configuration dialog exists, but no way to import data into the configured fields was discovered


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering functionality not implemented or UI not accessible
    - Actual: No field-specific filtering UI was found. Without filter capability, cannot verify that filtering includes exactly matching entries or that clearing filters restores all data. Search feature exists but provides text matching only, not field-specific filter criteria

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search does not match formatted display values for dates and checkbox states
    - Actual: Search for "Oct" (formatted date display) returned 0 results; search for "Yes" (checkbox state display) returned 0 results; search for "2026-10" (raw date value) returned 1 match. This shows search only matches raw internal date values, not formatted display, and does not match checkbox state text at all


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.