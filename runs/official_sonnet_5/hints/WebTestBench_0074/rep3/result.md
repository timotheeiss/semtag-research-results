# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across reload
    - Actual: Added entry 'Test Entry Alpha' appeared immediately in table/card views (6 entries), but after navigating/reloading the app, state reset to the original 5 seed entries and the new field 'Notes' was also gone. localStorage/sessionStorage were empty, confirming no persistence mechanism.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature present in the app
    - Actual: Full DOM scan of interactive elements (buttons/inputs) on the home screen and Fields dialog found only: Fields, Search, Sort, Table, Cards, Add Entry, field name/type inputs, Add, Close. No filter control, filter dialog, or per-field criteria UI exists anywhere in the app.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature present in the app
    - Actual: No import button, dialog, or file-upload control exists anywhere in the app despite the Fields dialog note stating 'Configure fields before importing data.' Full DOM scan found no import-related control.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature not implemented
    - Actual: No filter UI exists in the app (see FT-6), so filter-criteria matching and restore-on-clear behavior cannot be exercised.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw internal values instead of displayed formatted values
    - Actual: Searching 'Jan 15' (the displayed due-date text) returned '0 entries matching', while searching the raw ISO value '2026-01-15' returned 1 match. Searching 'Yes' (the displayed checkbox label) returned '0 entries matching' despite two entries having a true/'Yes' checkbox value.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.