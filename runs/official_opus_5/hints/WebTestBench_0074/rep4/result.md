# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted across page reload
    - Actual: Adding entry "Zebra QA Test Task" (Priority 7, Mar 5 2026, Completed=Yes) worked immediately: count went 5 -> 6 entries and the row appeared in the table. After reloading http://localhost:7074/, the count returned to "5 entries" and the new entry was gone (only the 5 seeded entries remained).

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filtering feature absent
    - Actual: No filter UI exists anywhere in the app. Full DOM scan of both table and card views lists only these controls: toolbar.fields, search.query, toolbar.sort, toolbar.view.table, toolbar.view.cards, toolbar.add-entry, per-row edit/delete and sortable column headers. No data-semtag-id containing "filter", no button labelled Filter, and the word "filter" does not appear in the page text. The Configure Fields dialog also offers no filter criteria. Therefore field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import feature absent
    - Actual: No import control exists: no button/menu labelled Import, no file input, and no data-semtag-id containing "import" in the toolbar, header, status bar, or the Configure Fields dialog. The Fields dialog only shows the hint text "Configure fields before importing data." but provides no way to import, so importing data after defining a schema and type-mismatch rejection cannot be performed.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering feature absent, so criteria/clearing cannot be evaluated
    - Actual: No filter controls exist in either view (only a quick search box), so there is no way to apply criteria-based filtering or clear filters. Untestable because the feature is missing.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw values, not the displayed formatted values
    - Actual: Searching the displayed date "Jan 18, 2026" returned '0 entries matching "Jan 18, 2026"', while the raw value "2026-01-18" returned the matching entry. Searching the displayed checkbox state "Yes" returned 0 entries, while the raw value "true" returned the 3 checked entries (Zebra QA Test Task, Team sync meeting notes, Review marketing materials).


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.