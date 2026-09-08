# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist across a page reload
    - Actual: Added "QA Test Entry" via Add Entry dialog; it appeared immediately (6 entries) and was visible across view switches. However, after reloading the page (http://localhost:6074/), the app reverted to the original 5 seed entries — "QA Test Entry" and the "Notes" field configured earlier were both gone, confirming that entries/fields are only kept in memory and are lost on reload.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature exists in the application
    - Actual: Searched the entire toolbar (Search box, Sort, Table/Cards toggle, Add Entry), the Sort dropdown menu (Task Name/Priority/Due Date/Completed/Notes/Clear sort only), table column headers, and the Configure Fields dialog for any filter control. No "Filter" button, icon, or panel exists anywhere; a full-page DOM text/aria-label scan for "filter" found zero matches.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the application
    - Actual: The Configure Fields dialog contains only a note stating "Configure fields before importing data," but no actual Import button, file upload control, or import UI exists anywhere in the dialog or the rest of the app (toolbar, headers). A full-page DOM scan for "import"/"csv" text found zero matches, so imported data cannot be tested for type-mismatch rejection.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to verify inclusion/clearing behavior
    - Actual: Since no filter UI exists anywhere in the app (toolbar, sort menu, column headers, Fields dialog all inspected via DOM), filtering criteria cannot be defined, so the filtering behavior described (matching entries, clearing to restore all) cannot be exercised or verified.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search only matches raw internal field values, not the formatted values displayed to users
    - Actual: Searching "Jan 15" (the exact displayed date text for an entry) returned 0 results, but searching the raw ISO value "2026-01-15" returned 1 matching entry. Searching "Yes" (the displayed Completed checkbox state, present on 3 entries) returned 0 results despite entries visibly showing "Yes".


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.