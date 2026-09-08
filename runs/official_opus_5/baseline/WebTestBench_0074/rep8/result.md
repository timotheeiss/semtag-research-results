# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries are not persisted across page reload
    - Actual: Created entry "Zebra QA test entry" (Priority 9, Mar 7 2026, Completed=Yes, Notes="QA note alpha"). It appeared immediately and the counter went 5 -> 6 entries. After reloading http://localhost:6074/, the table showed only the original 5 entries ("5 entries") and the new entry was gone. The added "Notes" field was also lost. Object.keys(localStorage) returned [] — no state is persisted anywhere.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Field-specific filtering feature is entirely absent
    - Actual: The app exposes no filter UI. Toolbar contains only: search box, Sort menu, Table/Cards toggle, Add Entry; header has only "Fields". The only icon-only buttons in the page are per-row pencil (edit) and trash (delete). A DOM-wide text scan for /filter/i matched only CSS <style> blocks — no filter control, no filter panel, and no per-column filter affordance. There is therefore no way to define field-specific filter criteria or to clear them.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import feature is entirely absent
    - Actual: There is no import control in the app: no input[type=file] elements (count 0), no Import/Upload/CSV/JSON/Paste button or menu item anywhere in the DOM. The Configure Fields dialog only contains field rows, an add-field row, a Close button, and the advisory text "Note: Changing field types may affect how existing data is displayed. Configure fields before importing data." — but no import entry point exists, so importing data and type-mismatch rejection cannot be performed.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Cannot verify filter inclusion/clearing because filtering does not exist
    - Actual: No filter criteria can be applied at all (no filter control anywhere in the DOM), so there is no active-criteria matching to evaluate and no "clear filters" action to restore data.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw values instead of the formatted values shown to users
    - Actual: Dates: the UI displays "Mar 7, 2026", but searching "Mar 7" and "Mar 7, 2026" each returned "0 entries", while the raw ISO value "2026-03-07" returned 1 entry (Zebra QA test entry). Likewise displayed "Jan 15" returned 0 entries. Checkboxes: the UI displays "Yes"/"No", but searching "Yes" returned "0 entries" while the raw boolean "true" returned 3 entries (the three checked rows). Searching "No" returned 2 entries that are merely text substring hits ("Team sync meeting notes", "Zebra QA test entry" via note text) and excluded the actual unchecked entries (Design new landing page, Update API documentation, Prepare quarterly report).


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.