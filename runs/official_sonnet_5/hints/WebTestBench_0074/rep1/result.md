# Test Result

## Functionality
- [X] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filtering feature is not implemented
    - Actual: No filter UI exists anywhere in the app. Toolbar only offers Fields, Sort, Table/Cards view toggle, Add Entry, and a quick-search box. Table column headers only expose sort-by-column buttons (verified via DOM inspection: header <th> only contain sort buttons, no filter icons/inputs). No element with "filter" in its attributes, class, or text was found in the DOM.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import feature is not implemented in the UI
    - Actual: No import control (button, file input, or menu option) exists anywhere in the app. Searched the toolbar, Configure Fields dialog (which only references "Configure fields before importing data" in a note, with no actual import affordance), and the full DOM for any file input or import-related control — none found. There is no way to import data to test schema-based rejection of mismatched values.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature is not implemented, so filter criteria cannot be applied or cleared
    - Actual: No filter controls exist in the UI (confirmed via DOM inspection); there is no way to define field-specific filter criteria or verify inclusion/exclusion of entries based on criteria.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches only raw internal field values, not the formatted values displayed to users
    - Actual: Searching "Jan 15" (the displayed date format shown in both table and card views) returned 0 entries, while searching the raw internal ISO value "2026-01-15" returned 1 matching entry. Searching "Yes" (the displayed checkbox state) returned 0 entries even though 3 entries display "Yes" for Completed.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.