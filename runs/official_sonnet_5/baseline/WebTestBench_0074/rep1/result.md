# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist across page reload
    - Actual: Added a new entry "QA Test Entry" which appeared immediately in the table. After reloading the page (navigating to the same URL), the entry list reverted to the original 5 seed entries and the new entry, along with a newly added "Notes" field, was gone. localStorage was empty, confirming no persistence mechanism is used.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature exists in the application
    - Actual: Searched the entire UI (toolbar, column headers via click and right-click, Fields dialog, row action buttons) for a way to define field-specific filter criteria. Only Search, Sort, Table/Cards toggle, and Add Entry controls exist. A full-page DOM scan confirmed the word "filter" does not appear anywhere in the rendered HTML. There is no way to filter entries by field value.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the application
    - Actual: Searched the entire UI (toolbar, Configure Fields dialog, Add Entry dialog, row action buttons) for a way to import data. Only the Configure Fields dialog contains a text note stating "Configure fields before importing data," but no actual import button, file upload control, or import dialog exists anywhere. A full-page DOM scan confirmed the word "import" does not appear in any interactive element — it's only referenced in that static help text. There is no way to import data into the app.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to verify inclusion/exclusion criteria or restore-on-clear behavior
    - Actual: Since no filtering UI is present anywhere in the app (confirmed via full DOM scan and exhaustive UI exploration), it is impossible to apply field-specific filter criteria, so the constraint that filtering includes exactly matching entries and restores all entries when cleared cannot be satisfied or verified — the feature simply does not exist.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches only internal raw values, not the formatted values shown to users
    - Actual: Searching "Jan 18" (the exact formatted date text displayed as "Jan 18, 2026" in the UI) returned 0 results. Searching the internal raw ISO date "2026-01-18" (not shown anywhere in the UI) correctly matched the "Review marketing materials" entry. Similarly, searching "Yes" (the displayed checkbox state text used in card view and entry form) returned 0 results despite two entries showing "Yes". This shows search only matches raw underlying data, not the formatted/displayed values.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.