# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Created entry "Zeta QA Audit" (Priority 5, Mar 10 2026, Completed checked, Score 42) appeared immediately and count went 5→6 entries. After reloading http://localhost:6074/, the table shows only the 5 original seed entries ("5 entries") and the new entry is gone (the added "Score" field also disappeared).

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filtering feature exists
    - Actual: The app exposes only Fields, a free-text "Search entries..." box, a Sort menu, Table/Cards toggle and Add Entry. No filter control is present in either view: document.body.innerHTML contains no occurrence of "filter", column headers only trigger sorting, and right-clicking a header opens no menu. Therefore field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability
    - Actual: The only occurrence of "import" in the app is the advisory note in the Configure Fields dialog ("Configure fields before importing data."). There is no import button, no input[type=file] (0 found), and no paste/CSV UI in either view, so data cannot be imported after defining a schema and type-mismatch rejection cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so inclusion/clearing behavior cannot hold
    - Actual: No filter UI exists anywhere in the app (no "filter" string in the DOM, no filter control in table or card view), so there are no active criteria to satisfy and no filters to clear.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not displayed formatted values
    - Actual: Entry displays date "Mar 10, 2026" and checkbox "Yes", but searching "Mar 10" returns "0 entries matching" and "Yes" returns "0 entries matching". Searching the internal raw values instead works: "2026-03-10" → 1 entry (Zeta QA Audit) and "true" → 3 entries (the three checked ones).


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.