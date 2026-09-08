# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Created entry "QA Test Entry" (Priority 7, Mar 5 2026, Completed checked, Score 42); it appeared immediately and count went 5→6 entries. After reloading http://localhost:7074/, the count returned to "5 entries", the new entry was gone, and the newly added "Score" field was also gone.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No filtering feature exists
    - Actual: The app exposes only Fields, Table/Cards toggle, Add Entry, a Sort select and a quick-search box. No filter button, panel or per-field criteria control is present in either view (page text contains no occurrence of "filter"), so field-specific filter criteria cannot be defined or cleared.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No data import capability
    - Actual: No import control exists anywhere: toolbar offers only Fields / Sort / Table / Cards / Add Entry / search; page text contains no "import", "CSV", "JSON" or "upload" affordance and there is no input[type=file]. The Configure Fields dialog even states "Configure fields before importing data", but provides no import entry point, so importing data and type-mismatch rejection cannot be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filtering not implemented, so filter correctness cannot be satisfied
    - Actual: There is no filter UI to define criteria or clear (only a global quick search). Additionally the only available narrowing mechanism (search) cannot match displayed date/checkbox values, so entries satisfying such criteria are excluded (e.g. searching "Mar 5" or "Yes" returns 0 entries).

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: No clear representation of empty values; card view omits the empty field entirely
    - Actual: Non-empty values match across views (e.g. "Zebra QA Task / 9 / Mar 5, 2026 / checked"), but an entry with an empty Due Date renders as a completely blank table cell and a blank card row, and an entry with an empty Task Name renders as a blank table cell while the card view drops the title element altogether (no placeholder such as "—" or "Untitled"). By contrast an empty number field did render "—", so empty-value display is inconsistent and unclear.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches raw stored values, not the displayed formatted values
    - Actual: Entry "Zebra QA Task" displays Due Date "Mar 5, 2026" and Completed "Yes", but searching "Mar 5" returned '0 entries matching "Mar 5"' and searching "Yes" returned '0 entries matching "Yes"'. Searching the internal raw value "2026-03-05" returned 1 entry, confirming matching is against raw values only.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.