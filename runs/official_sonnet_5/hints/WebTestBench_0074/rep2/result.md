# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted across page reload
    - Actual: Created entry "QA Test Entry" with Priority 42, Due Date Sep 1 2026, Notes "Hello World", Completed=Yes; it appeared immediately (entries count went from 5 to 6) and also a custom "Notes" text field was added. After navigating/reloading the app at http://localhost:6074/, the entry list reverted to the original 5 seed entries and the "Notes" field configuration also disappeared, i.e. no server-side or local persistence occurred.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature exists in the application
    - Actual: Searched the entire UI (toolbar, table headers, card view, semantic snapshot with hidden elements included, and a DOM-wide scan for any element with "filter" in aria-label/title/text) and found no filter control of any kind. Only a free-text "Search entries..." quick-search box exists; there is no field-specific filter criteria UI, so users cannot define filter criteria or view only matching entries via a filter mechanism.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature exists in the application
    - Actual: Searched the entire UI (toolbar, Fields dialog, table/card views) and performed a DOM-wide scan for any element with "import" in aria-label/title/text; no import control, button, or file-upload mechanism was found anywhere in the app. Users cannot import data at all, so schema-validated import behavior cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to evaluate against this constraint
    - Actual: As found for FT-6, there is no filter UI anywhere in the app (toolbar, table, card view, or DOM-wide search for "filter"-labeled elements all came up empty). Since no filtering functionality exists, entries cannot be filtered by field criteria, so the constraint cannot be satisfied.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches only raw internal values, not the formatted values shown to users
    - Actual: Searching "Sep 10" (the date format displayed in both table and card views as "Sep 10, 2026") returned 0 matches, while searching the raw internal ISO value "2026-09-10" returned 1 match. Similarly, searching "Yes" (the checkbox state text shown in Card view / meaning of the check icon in Table view for Completed=true, which 3 entries have) returned 0 matches. This shows search only queries raw stored values, not the user-visible formatted date or checkbox-state text.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.