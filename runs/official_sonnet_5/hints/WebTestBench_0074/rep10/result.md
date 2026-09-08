# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across page reload
    - Actual: Added entry 'QA Test Entry' (Priority 7, Due Sep 1 2026, Completed) appeared immediately (count went 5->6), but after reloading the page (navigating to the same URL) the entry list reverted to the original 5 entries and the count read '5 entries' again.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature present in the application
    - Actual: Searched the entire toolbar, table headers, and page DOM (button list and full innerHTML text search for 'filter') and found no filter control, filter dialog, or field-specific filter criteria UI of any kind. Only Sort, Search, view toggle, Add Entry, and Fields config controls exist.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import feature exists in the application
    - Actual: Searched the Fields dialog, toolbar, and entire page DOM for an import control; found no file input (input[type=file] count = 0), no 'Import' button, and no import dialog. The only reference to importing is a static note text: 'Configure fields before importing data.' No actual import functionality is implemented, so schema-based import validation cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature present to test inclusion criteria
    - Actual: No filter UI exists in the app (confirmed via DOM search), so no active criteria can be defined, and therefore filtering cannot include/exclude entries or be cleared as described.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches internal raw values, not the formatted values shown to users
    - Actual: Searching 'Jan 15' (the displayed Due Date format for 'Design new landing page') returned '0 entries matching'. Searching 'Yes' (the displayed Completed state for 3 entries) also returned '0 entries matching'. However, searching the raw internal ISO value '2026-01-15' returned '1 entry matching'. This shows the search indexes raw stored values (ISO date, boolean) instead of the formatted text ('Jan 15, 2026', 'Yes'/'No') visible to the user.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.