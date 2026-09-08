# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted across page reload
    - Actual: Created entry 'Zebra QA Task' (Priority 42, Mar 9 2026, Completed, Owner Alice) appeared immediately and count went 5→6 entries. After reloading http://localhost:7074/, count reverted to '5 entries', the new entry was gone, and the added 'Owner' field/column was also gone — all state reset to defaults.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filtering feature absent
    - Actual: No filter UI exists anywhere in the app: toolbar contains only Fields, Sort, Table, Cards, Add Entry and a 'Search entries...' text input. Table column headers only toggle sorting; the Fields dialog and Add/Edit Entry dialog contain no filter controls. Page text contains no occurrence of 'filter'. Field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import feature absent
    - Actual: No import capability found: no import/upload button or file input anywhere (only one input, the search box); Fields dialog offers only field add/rename/type/delete; page text contains no occurrence of 'import'. Importing data against a defined schema and type-mismatch rejection cannot be performed.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so criteria-based inclusion cannot be verified
    - Actual: No filter controls exist (no 'filter' text or control in the DOM), so no active criteria can be set or cleared; filtering behavior is untestable/unavailable.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: No clear representation for an empty date value
    - Actual: Entry 'Alpha empty test' saved with empty Priority and empty Due Date. Empty number renders the placeholder '—' in both table and card views, but the empty date cell renders as a completely blank string ("") in the table and as an empty area under the 'DUE DATE' label in the card — no placeholder, so an empty date is indistinguishable from a rendering fault. Other values are otherwise consistent across views.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not displayed values
    - Actual: Entry 'Zebra QA Task' displays Due Date 'Mar 9, 2026' and Completed 'Yes'. Searching 'Mar 9' → '0 entries matching "Mar 9"'; searching 'Yes' → '0 entries matching "Yes"'. Searching the internal raw value '2026-03-09' → '1 entry matching "2026-03-09"' (Zebra QA Task), proving search operates on raw values only.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.