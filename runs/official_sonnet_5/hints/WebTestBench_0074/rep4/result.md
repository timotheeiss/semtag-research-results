# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across page reload
    - Actual: Added entry 'Test Entry FT1' with all field values via Add Entry dialog; it appeared immediately (6 entries). After navigating/reloading the app URL, the entry list reverted to the original 5 seed entries; 'Test Entry FT1' and the previously added 'Notes' field were both gone, and count showed '5 entries'.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature implemented
    - Actual: No filter UI control exists anywhere in the app. semantic_snapshot (including hidden elements) shows no filter action/input, and a DOM-wide search (browser_evaluate) for any element with 'filter' in attributes, class name, or text content returned zero matches.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature implemented
    - Actual: No import button/control found anywhere in the app, including the 'Configure Fields' dialog which only contains field name/type editors and an 'Add' field control. A DOM-wide search for any element with 'import' in text/attributes returned zero matches, despite the dialog's note text: 'Configure fields before importing data.'


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature implemented, so filter criteria/behavior cannot be exercised
    - Actual: Same as FT-6: no filter control exists in the UI or DOM, so entries cannot be filtered by field criteria at all.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw field values instead of the formatted/displayed values
    - Actual: Searching 'Jan 18' (the displayed formatted date for entry-2's Due Date, shown as 'Jan 18, 2026') returned '0 entries matching'. Searching the raw ISO value '2026-01-18' (never shown to the user) correctly matched entry-2. Searching 'Yes' (the displayed Completed value in card view) also returned '0 entries matching' despite 3 entries showing 'Yes'.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.