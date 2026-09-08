# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist across page reload
    - Actual: Added a new entry 'Write QA report' which appeared immediately (6 entries) with all field values correct. After reloading the page via navigation to the same URL, the entry count reverted to 5 and the new entry was gone. localStorage is empty, confirming no persistence mechanism.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature exists in the application
    - Actual: Searched the entire UI (header controls, Sort menu, Fields dialog, column headers, search box) and the page DOM/HTML for any filter control (button, icon, popover). No element with 'filter' text or a filter/funnel icon exists anywhere; only a quick-search box and a Sort menu are present. Users cannot define field-specific filter criteria.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the application
    - Actual: Searched the entire UI (header, Fields dialog, table/card views) and DOM for an import control. No 'Import' button, file-upload input (input[type=file] count = 0), or any import UI was found. The Fields dialog only contains a note mentioning 'Configure fields before importing data' but provides no actual mechanism to import data.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filtering feature exists in the application
    - Actual: Same as FT-6: no filter UI/control found anywhere in the app, so filter criteria cannot be applied or cleared.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search only matches raw underlying values, not the formatted values shown to users
    - Actual: Searching 'Jan 15' (matching the displayed 'Jan 15, 2026' Due Date text) returned 0 entries. Searching 'Yes' (matching displayed Completed checkbox state) also returned 0 entries, despite multiple entries visibly showing 'Yes'. Search appears to only match text-type field raw values (e.g. Task Name), not formatted dates or checkbox labels.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.