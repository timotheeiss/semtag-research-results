# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entry does not persist after page reload
    - Actual: Created entry "Integration test entry" appeared in view (6 entries shown), but after page reload, entry disappeared and count returned to 5 entries. Entry is not retained in storage.

- [ ] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.
  - Bug Report:
    - Issue: Field configuration does not persist after page reload
    - Actual: Created "Status" text field appeared in forms and both views before reload, but disappeared after reload. Field configuration not persisted to storage.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [X] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering does not work when no sort is active
    - Actual: Cleared sort successfully (entries returned to original order, drag action appeared in table). Attempted to drag entry-1 to entry-3 position, but entries did not reorder. No visible change in entry order after drag operation.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import functionality not found or not implemented
    - Actual: After thorough exploration of the UI including toolbar, fields configuration dialog, entry forms, and main view, no import button or import functionality was discovered. No file upload control or import menu option visible in the interface.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [X] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Checkbox field (Completed) displayed inconsistently between views
    - Actual: In table view, the Completed checkbox field is not displayed in the table columns (only showing Task Name, Priority, Due Date). In card view, the Completed field IS displayed as "Yes" or "No". Same entry data displayed differently across views.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [ ] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.
  - Bug Report:
    - Issue: Checkbox values not displayed in table view, only in card view
    - Actual: In entry form: checkbox shown as toggle control with clear checked (true) vs unchecked (false) state. In card view: checkbox states displayed as "Yes" (checked) or "No" (unchecked). In table view: checkbox column (Completed field) not displayed at all, so states not visible.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search does not match formatted dates or checkbox states as displayed to users
    - Actual: Search for "Jan 15" (formatted date) returned 0 results, even though "Design new landing page" entry displays "Jan 15, 2026". Search for "Yes" (checkbox state) returned 0 results, even though 2 entries have Completed checkbox checked and display as "Yes" in card view.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.