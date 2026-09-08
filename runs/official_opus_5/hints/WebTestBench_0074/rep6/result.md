# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Created entry 'Alpha review task' — it appeared immediately and count went 5→6 entries. After reloading http://localhost:7074/, the table shows only the 5 original entries (entry-1..entry-5); the created entry and the two added fields (Notes, Score) are gone.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filtering feature
    - Actual: The app exposes only toolbar.fields, search.query, toolbar.sort, view toggles, toolbar.add-entry and status bar. DOM scan for any 'filter' control/text returned 0 hits, and no per-column/per-field filter criteria UI exists (only a global quick search), so filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import feature exists
    - Actual: No import/upload/CSV/JSON/file control anywhere: toolbar has only Fields, Search, Sort, Table, Cards, Add Entry; the Configure Fields dialog contains only field rows, an add-field row, a Close button and the text 'Configure fields before importing data.' DOM scan for import/upload/csv/json/file controls returned 0 hits, so imported data and type-mismatch rejection cannot be tested.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filter criteria feature absent, so filter inclusion/clearing cannot be satisfied
    - Actual: No filter UI exists anywhere in the app (0 matches for 'filter' in DOM controls or body text); only a global quick search box is available, so there are no active criteria to include entries by nor filters to clear.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not displayed formatted values
    - Actual: Entry 'Alpha review task' displays Due Date 'Mar 5, 2026' and Completed 'Yes' (card view) / check icon. Searching 'Mar 5, 2026' → '0 entries matching'; searching the internal raw value '2026-03-05' → '1 entry matching'. Searching the displayed checkbox state 'Yes' → '0 entries matching' although 3 entries display 'Yes'.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.