# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry is not persisted across page reload
    - Actual: Added entry "Zebra QA test entry" (Priority 7, Mar 9 2026, Completed) appeared immediately and count went 5 -> 6 entries. After reloading http://localhost:6074/, the table shows only the original 5 seed entries and "5 entries"; localStorage and sessionStorage are both empty, so no persistence layer exists.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filtering feature is entirely missing
    - Actual: No filter control exists anywhere: toolbar has only Fields, Search entries..., Sort, Table/Cards, Add Entry; the Sort menu lists only field names; column headers only toggle sort; the Fields dialog only configures fields. A DOM-wide scan for the string "filter" in document.body.innerHTML returned false. Field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability exists
    - Actual: No import entry point anywhere: main toolbar buttons are only Fields/Sort/Table/Cards/Add Entry (+ column sort buttons); no input[type=file], no textarea/paste area, and the string "import" appears 0 times in the rendered DOM. The Fields dialog only lists field rows, an Add-field row, and the advisory note "Configure fields before importing data." — but provides no import control. Type-mismatch rejection on import therefore cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter criteria available, so filter inclusion/clearing cannot be satisfied
    - Actual: The app exposes no filtering UI (no filter button, no per-field filter inputs, no "filter" text in the DOM), so there is no set of active criteria to include entries by and nothing to clear. Only a free-text quick search exists.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not the displayed formatted values
    - Actual: Entry "Alpha review draft" displays Due Date "Feb 28, 2026" and Completed "Yes". Searching "Feb 28" returns "0 entries matching \"Feb 28\"", while searching the raw value "2026-02-28" returns that entry. Searching "Yes" (checkbox state shown in cards/forms) returns "0 entries matching \"Yes\"", and "No" likewise does not match unchecked entries.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.