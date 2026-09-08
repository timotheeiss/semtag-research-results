# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Data not persisted across page reload
    - Actual: Added entry "Test Entry Alpha" (and a "Notes" field, and another entry "Number Test Entry") appeared immediately in the UI. After navigating to the same URL (reload), the app reverted to only the original 5 seed entries and the original 4 fields; all newly created entries and the new field were gone.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature implemented
    - Actual: No UI control, button, icon, or menu for defining filter criteria exists anywhere in the app. Only a sort menu and a search box are present.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature implemented
    - Actual: No import button, file input, or import UI exists anywhere in the app despite the Fields dialog note stating "Configure fields before importing data."


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature implemented
    - Actual: Filtering criteria cannot be defined, applied, or cleared since no filter UI exists in the app.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Inconsistent empty-value representation
    - Actual: Empty values are not given a consistent, clear representation. Entries whose Notes field was never set (e.g. pre-existing entries after the field was added later) show a "—" placeholder in both Table and Cards views. However, an entry created via the Add Entry form with the Notes field left blank ("Number Test Entry") shows a completely blank cell/attribute value with no placeholder at all, in both views.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches only raw internal values, not displayed formatted values
    - Actual: Searching for the displayed date format "Jan 15" returned 0 results even though an entry's Due Date is displayed as "Jan 15, 2026". Searching for the raw ISO value "2026-01-15" did return that entry (1 match). Searching for the displayed checkbox state "Yes" (shown in Cards view for Completed=true entries) returned 0 results.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.