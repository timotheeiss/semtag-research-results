# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entries do not persist across page reload
    - Actual: Added entry 'Buy groceries' appeared immediately (6 entries), but after reloading the page the entry count reverted to 5 and the new entry, along with the newly added 'Notes' field, were gone. localStorage and sessionStorage were both empty, confirming no persistence mechanism.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: Filter feature is not implemented
    - Actual: No filter UI exists anywhere in the app. Checked toolbar, column headers, Fields dialog, and searched the full DOM for any element with 'filter' in text/class/aria-label/title or a filter icon (lucide-filter) — none found. Only Sort (by field) and quick Search are available; there is no way to define field-specific filter criteria.

- [X] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: Import feature is not implemented
    - Actual: No import button, file-upload control, or any import UI exists anywhere in the app. The Fields dialog only contains a textual note ('Configure fields before importing data') but provides no actual import functionality. Searched entire DOM/page text for 'import', 'upload', 'csv', 'export' controls — none found.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Filter feature is not implemented
    - Actual: No filter mechanism exists in the app (confirmed via DOM search for filter UI/icons), so filter criteria cannot be applied or cleared.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Empty field value has no clear representation in either view
    - Actual: Created entry 'Empty field test' with an empty Due Date value. In Table view the Due Date cell rendered completely blank (no placeholder text). In Card view the 'Due Date' label appeared with no value row at all, unlike other fields. Other fields with missing values (e.g. a newly-added text field for existing entries) displayed a '—' placeholder, so representation is inconsistent and the empty date has no clear indicator at all.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Quick search only matches raw internal values, not displayed formatted values
    - Actual: Searching for the displayed date text 'Jan 18' (shown as 'Jan 18, 2026' in both views) returned 0 results, while searching the raw ISO value '2026-01-18' correctly returned the matching entry. Searching for the displayed checkbox state text 'Yes' (Card view shows 'Yes'/'No') returned 0 results even though entries with Completed=Yes exist. This confirms search matches only internal raw values, not the formatted values users see.


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.