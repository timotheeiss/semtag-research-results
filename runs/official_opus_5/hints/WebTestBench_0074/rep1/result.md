# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted across page reload
    - Actual: Created entry "Alpha QA Task" (Priority 7, Mar 5 2026, Completed checked, Notes "Zebra note"); it appeared immediately and count went 5 -> 6 entries. After reloading http://localhost:7074/, the count returned to "5 entries" and the new entry was gone (table rows back to the original 5 seed entries). The added "Notes" field was also lost.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature exists
    - Actual: The app exposes only Fields, Sort, Table/Cards toggle, Add Entry and a free-text Search box. There is no filter control anywhere: full DOM scan of the page and of the Configure Fields dialog found no element with "filter" text or a filter-related data-semtag-id, so field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability
    - Actual: The Configure Fields dialog states "Configure fields before importing data", but no import control exists anywhere: the dialog contains only field name inputs, type selects, delete buttons, Add and Close; the toolbar has no import/upload button and the page contains no file input. Import of data (and therefore type-mismatch rejection) cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so filter inclusion/clearing cannot hold
    - Actual: No filter UI exists (toolbar contains only Fields, Search, Sort, Table/Cards, Add Entry). With no way to activate filter criteria, the requirement that filtering include exactly matching entries and clearing restore all entries cannot be satisfied.

- [X] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not the displayed formatted values
    - Actual: Entry "Alpha QA Task" displays Due Date "Mar 5, 2026" and Completed "Yes". Searching the displayed text returns nothing: "Mar 5" -> '0 entries matching "Mar 5"', "Yes" -> '0 entries matching "Yes"'. Searching raw values does match: "2026-03-05" -> 1 entry, "true" -> 3 entries.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.