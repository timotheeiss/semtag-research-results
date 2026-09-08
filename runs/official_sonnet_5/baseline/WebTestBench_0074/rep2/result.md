# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist across page reload
    - Actual: Created entry "Test Entry QA" (with values for Task Name, Priority, Due Date, Completed, and a custom "Notes" field) which appeared immediately in both views. After reloading the page (http://localhost:6074/), the entry count reverted to the original 5 entries, "Test Entry QA" was gone, and the custom "Notes" field/column disappeared entirely — none of the data was persisted.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filtering feature is missing
    - Actual: No filter control exists anywhere in the UI. Searched the toolbar (Sort, Table/Cards, Add Entry, Search), Configure Fields dialog, column headers, and card field labels for a filter affordance; none exists. A DOM-wide search for the word "filter" found zero matches. Only quick text search is available, which is a different feature (IX-17).

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import feature is missing
    - Actual: No import control/button exists anywhere in the UI (toolbar or Configure Fields dialog). The Configure Fields dialog only contains a note text mentioning "Configure fields before importing data" but there is no actual import mechanism to upload or paste data. A DOM-wide search for "import" found only this note text, no functional import UI.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature is missing, so criteria matching cannot be verified
    - Actual: No filter UI or control exists in the application (verified via DOM inspection - no "filter" related elements found), so entries cannot be filtered by field criteria and the clear/restore behavior cannot be tested.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search matches only raw internal values, not the formatted values shown to users
    - Actual: Searching "Jan 15" (the displayed date format) returned 0 results, but searching the raw ISO value "2026-01-15" returned the matching entry. Searching "Yes" (the displayed checkbox state) returned 0 results despite 3 entries showing "Yes" for Completed.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.