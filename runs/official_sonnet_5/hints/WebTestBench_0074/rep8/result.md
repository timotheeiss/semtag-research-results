# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across page reload
    - Actual: Added entry 'Test Entry Alpha' appeared immediately (count went 5->6), but after reloading http://localhost:6074/ the entry count reverted to 5 and the new entry was gone. The newly added 'Notes' field was also lost.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature exists in the UI
    - Actual: Searched the entire toolbar, table headers, and DOM (all data-semtag-id attributes and all button labels) for a filter control; only Fields, Sort, Table/Cards view toggle, Add Entry, and Search were found. No UI element allows defining field-specific filter criteria or restricting displayed entries to matching criteria (separate from quick search).

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import feature exists in the app
    - Actual: Searched the entire toolbar, main view, and Fields Configuration dialog (including full DOM/body HTML) for an import control or file upload. Only a static note text 'Configure fields before importing data' exists in the Fields dialog, but no actual button, file input, or UI flow to import data was found anywhere in the app.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to test criteria matching
    - Actual: No filter UI is present anywhere in the app (verified via DOM scan), so field-specific filter criteria cannot be defined, and therefore filtering behavior (matching entries, restoring all on clear) cannot be exercised.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches internal raw values instead of formatted displayed values
    - Actual: Searching displayed date text 'Jan 18' returned 0 results ('0 entries matching \"Jan 18\"') even though the table clearly shows 'Jan 18, 2026' for that entry. The same entry WAS found when searching the raw ISO value '2026-01-18'. Searching the displayed checkbox state 'Yes' (shown in card view and used for Completed) returned 0 results despite two entries showing 'Yes'.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.