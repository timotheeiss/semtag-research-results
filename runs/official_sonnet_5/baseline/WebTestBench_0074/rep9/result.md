# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries and field/schema changes are not persisted across page reload
    - Actual: Created entry "Write test cases" (visible immediately, entry count went 5→6) and added a "Score" field. After reloading the page (http://localhost:6074/), the app reverted to the original 5 seed entries with no "Score" field and no "Write test cases" entry. All added data, field configuration, sort state, and manual reorder were lost — state exists only in memory, not persisted (e.g., no localStorage/backend save).

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filter feature not implemented/discoverable
    - Actual: Inspected the entire toolbar (Fields, Sort, Table/Cards, Add Entry, Search) and all column headers via DOM; no "Filter" button, icon, or per-field filter UI exists anywhere in the app. The word "filter" does not appear in the rendered DOM at all. Only Quick Search (text substring match) and Sort are available; there is no way to define field-specific filter criteria (e.g., number range, checkbox state, date range) to show only matching entries.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the application
    - Actual: Searched the entire UI (toolbar, Configure Fields dialog, and full DOM) for an import control. No import button, file input, or drag-and-drop import area exists anywhere. The only reference to "import" is a static note in the Configure Fields dialog ("Configure fields before importing data"), but no actual import functionality is implemented, so data cannot be imported or validated against the field schema.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to test inclusion/clearing behavior
    - Actual: No filter UI is present in the application (confirmed via DOM inspection - no filter button, icon, or panel). Since field-specific filtering cannot be configured, filtering behavior (matching entries, clearing to restore all) cannot be exercised or verified.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches only raw internal values, not the formatted values shown to users
    - Actual: Searching "Jan 15" (the displayed Due Date format) returned 0 entries, while searching the raw ISO value "2026-01-15" correctly matched the "Design new landing page" entry. Similarly, searching "Yes" (the displayed Completed checkbox state) returned 0 entries despite 3 entries showing "Yes". Search therefore only matches internal raw field values, not the user-visible formatted date or checkbox text.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.