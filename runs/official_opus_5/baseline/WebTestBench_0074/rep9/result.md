# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: Created entries are not persisted across page reload
    - Actual: Entry "Alpha Release Checklist" (Priority 7, Mar 10 2026, Completed=yes, Score 42.5) was created and appeared immediately, and the count went 5→6 entries. After reloading http://localhost:6074/, the table showed only the 5 original seed entries ("5 entries") and the new entry was gone. The user-added "Score" field/column was also lost. localStorage is empty (Object.keys(localStorage) === []), so no state is saved.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Field-specific filtering is not implemented — no filter UI exists
    - Actual: The app provides no way to define field-specific filter criteria. The complete set of controls is: Fields, quick-search box ("Search entries..."), Sort (field list + Clear sort), Table/Cards toggle, Add Entry, and per-row edit/delete/drag buttons. The only <input> on the page is the search box; there are no filter controls in the toolbar, in column headers (clicking a header only sorts), or in the Fields dialog. Therefore filter criteria cannot be defined, matching entries cannot be isolated, and there are no filters to clear.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Data import is not implemented — no import mechanism exists
    - Actual: Field schema definition works (Configure Fields dialog), but there is no way to import data. The full button set is Fields, Sort, Table, Cards, Add Entry, and column-header sort buttons; there are zero input[type=file] elements and zero textareas anywhere in the document (main page and Fields dialog both checked). The Fields dialog only displays the advisory text "Configure fields before importing data." with no accompanying import control, file picker, paste area, or CSV/JSON option. Consequently type-mismatch rejection on import cannot occur and could not be exercised.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Cannot verify filter inclusion/clearing because filtering does not exist
    - Actual: No filtering capability is present in the app (no filter controls in toolbar, column headers, or Fields dialog; the only input is the quick-search box). With no active criteria to satisfy and no filters to clear, this constraint cannot be met.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Empty value has no clear representation and is rendered inconsistently between views and across field types
    - Actual: An entry saved with an empty Task Name (text) renders with no representation at all: in table view the Task Name cell innerHTML is "" (completely blank, no placeholder), and in card view the card has no h3 heading element at all (last card innerText = "PRIORITY\n4\nDUE DATE\nJun 1, 2026\nCOMPLETED\nNo"), so the entry appears unlabeled/anonymous. By contrast an empty Number field (the added "Score" column) did render a clear "—" placeholder in table view, so empty handling is inconsistent across field types too.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search matches internal raw values instead of the displayed formatted dates and checkbox states
    - Actual: Dates: the table displays "Feb 28, 2026" for Zebra Deployment Plan, but searching "Feb 28, 2026" returns "0 entries matching" and "Feb" returns "0 entries matching"; searching the raw ISO value "2026-02-28" returns 1 entry. Checkboxes: several entries display "Yes" (Completed), yet searching "Yes" returns "0 entries matching". Searching "No" returned only "Team sync meeting notes" — a substring match on the text "notes", not the four entries whose Completed displays "No". So formatted dates and checkbox states shown to users are not searchable.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.