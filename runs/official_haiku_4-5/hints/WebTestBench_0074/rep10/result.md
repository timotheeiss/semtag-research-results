# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entry does not persist after page reload
    - Actual: Created entry "Persistence Test Entry" appears immediately (6 entries), but after page reload entry count returns to 5 and entry is no longer present

- [ ] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.
  - Bug Report:
    - Issue: Custom field does not persist after page reload
    - Actual: Added "Status" text field appears in table and forms, but after page reload the custom field is no longer present, only original 4 fields remain

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filter UI found
    - Actual: Application provides search bar for text filtering, but no UI for defining field-specific filter criteria. No filter button, panel, or dialog visible in toolbar or menus. Cannot filter by specific field values (e.g., Priority > 2).

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering does not change entry order
    - Actual: Dragged entry from position 1 to position 3, but entry order remained unchanged (entry-1, entry-2, entry-3, entry-4, entry-5). The drag action was registered but did not reorder entries.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import functionality found
    - Actual: No import UI, button, or menu option visible in the application. No file upload dialog or import wizard. Cannot import data even though field schema is configured (4 fields defined with text, number, date, checkbox types).


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
    - Issue: Search does not match checkbox displayed values
    - Actual: Search matches text ('review') and date numbers ('15' from 'Jan 15, 2026'), but does not match checkbox states ('Yes'/'No'). Entries 2 and 5 with Completed=Yes are not found when searching 'Yes' or 'yes'.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.