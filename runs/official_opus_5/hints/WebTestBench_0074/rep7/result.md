# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted across page reload
    - Actual: Created entry "Zebra QA Test Entry" appeared immediately (count went 5 -> 6 entries). After reloading http://localhost:7074/, the table shows only the 5 seed entries and count reads "5 entries"; the created entry and the added "Score" field are gone. localStorage and sessionStorage are both empty (no persistence layer).

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filtering feature exists
    - Actual: In both table and card views the entire UI contains no filter control: all buttons are Fields / Sort / Table / Cards / Add Entry, the only input is the global "Search entries..." box, no element has a filter-related data-semtag-id, and the word "filter" does not appear anywhere in the page text. The Configure Fields dialog only offers name/type/delete. Field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability in the app
    - Actual: After defining a field schema (Task Name/Priority/Due Date/Completed/Notes), no import entry point exists: zero input[type=file] elements, no element with an import/upload/csv/paste data-semtag-id, and no "import"/"CSV"/"upload" text anywhere in the page. Only manual "Add Entry" is available, so imported values and their type-mismatch rejection cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filter inclusion/clearing cannot be satisfied - no filter feature
    - Actual: No filtering UI exists (see FT-6), so no active criteria can be set or cleared. Only the global quick search narrows entries; it restores all 6 entries when cleared, but it is not a field-specific filter.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw values instead of displayed values for date and checkbox fields
    - Actual: Card/table show "Mar 9, 2026" but searching "Mar 9" returns "0 entries matching \"Mar 9\""; searching the raw ISO value "2026-03-09" returns 1 entry. Cards show Completed "Yes"/"No" but searching "Yes" returns "0 entries matching \"Yes\"" while searching the raw "true" returns 3 entries.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.