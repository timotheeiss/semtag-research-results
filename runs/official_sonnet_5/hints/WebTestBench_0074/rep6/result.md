# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across page reload
    - Actual: Added entry 'Test Entry FT1' (count went from 5 to 6 entries, visible in table). After navigating/reloading http://localhost:6074/, entry count reverted to 5 and 'Test Entry FT1' was no longer present in the table.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature exists in the app
    - Actual: Searched the entire toolbar, table headers, card view, and full page DOM for any filter control (button, icon, or panel). No 'Filter' text, no funnel/filter icon (lucide-filter), and no filter-related data-semtag elements were found anywhere. Only Sort, Search, view toggles, Add Entry, and Fields controls exist. There is no way to define field-specific filter criteria.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the app
    - Actual: The Fields configuration dialog only contains a note stating 'Configure fields before importing data', but there is no actual import button, file picker, or data-paste control anywhere in the dialog or the rest of the app. A full-page HTML search for 'import' only found this note text, no functional import control. Therefore importing data and validating type-mismatch rejection cannot be performed.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to test inclusion/exclusion or clearing
    - Actual: No filter UI is present anywhere in the app (toolbar, table headers, card view). Since there is no way to define filter criteria, the app cannot include/exclude entries by criteria or provide a 'clear filters' action to verify.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches only raw internal values, not the formatted values shown to users
    - Actual: Searching for the displayed due-date text 'Jan 15' (as shown 'Jan 15, 2026' in both views) returned '0 entries matching', while searching the raw ISO value '2026-01-15' returned '1 entry matching'. Searching the displayed checkbox label 'Yes' (shown in card view) returned '0 entries matching' even though 2 entries are checked/'Yes'.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.