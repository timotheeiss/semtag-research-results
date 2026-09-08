# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry is not persisted across page reload
    - Actual: Created entry "Zebra test task" (Priority 5, Mar 10 2026, Completed=yes) appeared immediately in the table (6 entries). After reloading http://localhost:7074/ the table shows only the original 5 entries (entry-1..entry-5); the new entry is gone.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature exists in the app
    - Actual: The UI offers only Fields config, quick search, Sort menu, view toggle and Add Entry. No element with a filter-related data-semtag-id exists and the page text contains no "filter" affordance, so field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import feature exists
    - Actual: DOM scan found no import/upload/CSV/JSON controls, no input[type=file], and the Fields config dialog only offers add/rename/retype/delete field. Top-level controls are limited to toolbar.fields, search.query, toolbar.sort, toolbar.add-entry. Importing data after defining a schema is therefore impossible.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, cannot be verified
    - Actual: No filter controls exist anywhere in the app (DOM scan for filter-related semtag ids returned none), so no active criteria can be applied or cleared.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw values, not the displayed formatted values
    - Actual: Searching "Jan 18" (the value shown in table/cards for entry-2's Due Date "Jan 18, 2026") returns "0 entries matching"; searching the raw stored value "2026-01-18" returns 1 entry. Searching "Yes" (the checkbox state shown in card view) returns 0 entries although two entries display Completed = Yes.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.