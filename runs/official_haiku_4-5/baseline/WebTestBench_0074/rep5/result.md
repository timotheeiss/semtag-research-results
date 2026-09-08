# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Entry not persisted after page reload
    - Actual: Created entry "Test Entry QA" appeared immediately in table (6 entries shown), but after page reload, it disappeared and entry count returned to 5.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [ ] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.
  - Bug Report:
    - Issue: Entry values not persisted after page reload
    - Actual: Entry "Test All Fields" displayed correctly in both table and card views with all values (text, numbers, dates, checkbox) before reload. After reload, entry is gone, entry count returned to 5, and newly added fields disappeared.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [X] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering via drag does not change entry order
    - Actual: Attempted to drag 'Review marketing materials' row to first position. Rows remained in original order after drag. UI shows 'Drag entries to reorder' but drag action has no effect.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import feature not accessible or not implemented
    - Actual: Fields dialog mentions 'Configure fields before importing data' but no import button or option is visible in the dialog or main UI. Cannot locate import functionality to test validation of imported data against field types.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [X] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Checkbox values displayed inconsistently between table and card views
    - Actual: For entry 'Design new landing page': Table shows checkmark icon for Completed, but Card view shows 'No'. Same entry has different value representation in different views. Table shows icons, cards show text.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search does not match formatted date values shown to users
    - Actual: Searched for 'Jan' which appears in all 5 entries' Due Date display (Jan 14, 15, 18, 20, 25). Search returned 0 entries. Quick search does not match formatted dates, only text field values.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.