# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Created entries are not persisted across page reload
    - Actual: Adding entry "QA Test Entry Alpha" (Priority 7, Mar 5 2026, Completed=Yes) worked immediately: row appeared and counter went 5 -> 6 entries. After reloading http://localhost:6074/, the entry was gone, counter back to "5 entries", and localStorage was empty (Object.keys(localStorage) === []), so no persistence layer exists.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Field-specific filtering feature is entirely missing
    - Actual: No filter control exists anywhere in the UI. The complete set of controls is: Fields, "Search entries..." box, Sort, Table/Cards toggle, Add Entry, and per-column sort buttons. The word "filter" appears nowhere in the rendered page (/filter/i test on document.body.innerText === false), and the Fields dialog and Sort menu contain only field config and sort options. Fetching /src/pages/Index.tsx shows no "filter" token either. Therefore filter criteria cannot be defined, applied, or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import feature does not exist
    - Actual: No import entry point anywhere: no "import" text in the rendered page, no input[type=file] (count 0), and Index.tsx imports only button, SearchBar, ViewToggle, FieldConfigDialog, EntryDialog, SortDropdown, TableView, CardView — no import/CSV/upload component or handler. The Fields dialog even states "Configure fields before importing data", but no import mechanism is provided, so importing after defining a schema and type-mismatch rejection cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Cannot be satisfied — no filtering capability exists
    - Actual: There is no way to define active filter criteria, so inclusion of matching entries and restoration on clearing cannot occur. No filter UI is present in the toolbar, Fields dialog, Sort menu, or column headers, and no "filter" token exists in the page text or Index.tsx source.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values instead of displayed values for dates and checkboxes
    - Actual: Date: the table displays "Jan 15, 2026", but searching "Jan 15, 2026" returned "0 entries"; searching the internal raw value "2026-01-15" returned the matching entry "Design new landing page" (1 entry). Checkbox: card view displays "Yes"/"No" for Completed, but searching "Yes" returned "0 entries" despite 3 entries being checked. Only text/number values are searchable by their shown form.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.