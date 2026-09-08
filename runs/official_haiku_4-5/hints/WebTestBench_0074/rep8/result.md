# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entry not persisted after page reload
    - Actual: Created entry "Test Entry Task" with all field values appeared immediately in the table (count: 5→6), but after page reload the entry count reverted to 5 and the entry was gone.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filter configuration UI found
    - Actual: The app has a search bar that performs general text search matching across all fields. It can filter entries (searched "Design" showed 1 match) and filters can be cleared to restore all entries. However, there is no UI to define field-specific filter criteria (e.g., "Priority = 1" or "Due Date > 2026-01-15").

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Drag and drop reordering not functional
    - Actual: When sort is cleared, drag handles appear with "reorder-entry" action. However, dragging entry-2 to entry-5 and entry-1 to entry-3 did not change the entry order. Entries remained in original positions despite drag operations completing without errors.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import feature not implemented or not accessible
    - Actual: No import button, menu, or import functionality found in the interface. Checked toolbar, Fields dialog, and entire page layout. Application has Field schema configuration (8 fields defined), but no way to import data.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [X] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches raw values, not displayed formatted values
    - Actual: Searched for "Jan 15" (displayed date format in "Design new landing page") - 0 matches. Searched for "Yes" (displayed checkbox state in multiple entries) - 0 matches. Search does not match formatted dates or checkbox states shown to users, only raw internal values.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.