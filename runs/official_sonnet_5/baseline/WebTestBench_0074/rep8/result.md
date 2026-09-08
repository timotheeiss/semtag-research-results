# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist after page reload
    - Actual: Added entry "Test Entry Alpha" (Priority 42, Due Date Sep 10 2026, Completed Yes, Notes "Sample note text") appeared immediately in the table (6 entries). After reloading http://localhost:6074/, the entry list reverted to the original 5 seed entries and the new entry was gone; the field configuration itself also reset, confirming no persistence layer.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filter feature is exposed in the UI
    - Actual: Searched the entire toolbar and table/card views for any filter control (button text, aria-label, icon, class name, or on-hover affordance containing "filter"). Only Fields, Sort, Table/Cards view toggle, Add Entry, and a free-text Search box exist. No UI element lets a user define field-specific filter criteria (e.g., "Priority > 2" or "Completed = Yes"). The feature described by FT-6 could not be located or exercised.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import feature is exposed in the UI
    - Actual: Searched the whole app (toolbar, Configure Fields dialog, table/card views) for an import control. The Configure Fields dialog only shows a note stating "Configure fields before importing data" but provides no actual Import button, file picker, or paste-data control anywhere in the app. A DOM-wide text search for "import" (case-insensitive) found no matches besides that static note. The import capability described by FT-19 could not be located or exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: No filter feature is exposed in the UI
    - Actual: Since no filter control exists anywhere in the app (see FT-6 finding), it is impossible to define active filter criteria, so the requirement that filtering include exactly the matching entries and clearing restore all entries cannot be exercised or verified.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Empty field value not given a clear placeholder in either view
    - Actual: Entries with an unset (undefined) Notes value show a "—" placeholder consistently in both Table and Card views. However, when a new entry is created leaving the Notes text field blank (empty string ""), the cell/card row renders with no text and no "—" placeholder at all (blank), differing from the undefined case. So an explicit empty value has no clear representation, unlike the unset case.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search does not match displayed/formatted field values for date or checkbox fields
    - Actual: Searching "Jan 15" (the exact formatted Due Date text shown for "Design new landing page": "Jan 15, 2026") returned 0 results ("0 entries matching \"Jan 15\""). Searching "Yes" (the exact Completed text shown for several entries) also returned 0 results. Search appears to only match the Task Name/Notes text fields and internal raw values, not the human-readable formatted date or checkbox labels users see in the views.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.