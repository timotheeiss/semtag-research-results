# Test Result

## Functionality
- [X] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature is present in the application
    - Actual: Searched the entire UI (toolbar, column headers via click and right-click, Configure Fields dialog, Add Entry dialog) and the full DOM for any "filter" affordance. Toolbar only exposes Search, Sort, Table/Cards view toggle, and Add Entry — no way to define field-specific filter criteria or view only matching entries.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import feature exists in the application
    - Actual: Searched the Configure Fields dialog (which references importing data in a note), the main toolbar, and the full DOM (no input[type=file], no button/link/menuitem containing "import") — there is no UI control to import data at all, so field-schema-gated import validation cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature exists to verify inclusion/exclusion criteria or restoration behavior
    - Actual: No filter UI could be located anywhere in the app (toolbar, headers, dialogs, or DOM scan for "filter"-related attributes), so field-specific filter criteria cannot be applied or cleared.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search does not match displayed/formatted field values for date and checkbox fields
    - Actual: Searching "Jan 15" (the exact due-date text shown for "Design new landing page") returned "0 entries matching \"Jan 15\"" instead of matching that entry. Searching "Yes" (the exact Completed value shown for several entries) also returned 0 results. Search appears to only match raw/internal values (e.g. ISO date strings or text/number fields), not the human-readable values rendered in the UI.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.