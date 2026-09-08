# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry does not persist across page reload
    - Actual: Added entry 'QA Test Entry' appeared immediately in the table (6 entries), but after navigating/reloading the page, the app reset to its initial default state: only the original 5 entries remained and the new entry was gone. Additionally, the 'Urgent' field added earlier also disappeared. Checked localStorage/sessionStorage after reload - both are empty, confirming the app keeps no persisted state at all.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature is implemented/exposed in the UI
    - Actual: Searched the entire visible UI (header controls: Fields, Sort, Table/Cards toggle, Add Entry, Search box) and inspected the full DOM/HTML for any element, button, icon, or attribute referencing 'filter'. No filter control exists anywhere - no per-field filter criteria UI, no filter panel, no filter button on column headers. Only Sort (which reorders but does not exclude entries) and quick Search (free-text) are available. There is no way to define field-specific filter criteria or view only matching entries.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature is implemented/exposed in the UI
    - Actual: Inspected the full UI (header, Fields configuration dialog, entry rows/cards, action buttons) and the DOM/HTML for any 'import' or 'export' related control. The Fields dialog only shows a note stating 'Configure fields before importing data' but provides no actual import button, file picker, or paste-data control anywhere in the app. There is no way to import data at all.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature does not exist, so filter correctness cannot be verified
    - Actual: As found under FT-6, there is no filter UI or mechanism in the application. Since filtering criteria cannot be defined, entries cannot be included/excluded per criteria, and there is nothing to 'clear' - the required behavior is entirely absent.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches only raw/internal field values, not the formatted values shown to users
    - Actual: Searching for the displayed date text 'Jan 18' (as shown in both Table and Card views for 'Review marketing materials') returned 0 results ('0 entries matching \"Jan 18\"'). Searching instead for the raw internal ISO date '2026-01-18' correctly returned that entry. Similarly, searching for 'Yes' (the displayed Completed checkbox state shown for multiple entries) returned 0 results, even though 'Yes' is visibly displayed in the UI for those entries. This confirms search only matches raw/internal values, not the formatted/displayed values as required.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.