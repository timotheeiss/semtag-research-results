# Test Result

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.
  - Bug Report:
    - Issue: New entry not persisted after page reload
    - Actual: Created entry "Test Entry" appeared immediately (6 entries total), but after page reload, only 5 original entries remained. The new entry was lost.

- [X] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [X] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [X] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [X] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.
  - Bug Report:
    - Issue: No field-specific filter UI found
    - Actual: Searched for filter-related elements (buttons, inputs, actions) and found none. Available actions are: open-field-config, sort-entries, switch-to-table-view, switch-to-card-view, add-entry, sort-by-column, edit-entry, delete-entry. No filter action or element exists.

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.
  - Bug Report:
    - Issue: Manual reordering via drag-and-drop not verified as functional
    - Actual: Drag UI elements appear when no sort is active (drag handles visible, "reorder-entry" actions present in semantic hints). However, drag operations using Playwright's dragTo method did not result in visible reordering of entries. Unable to confirm the feature works as intended.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.
  - Bug Report:
    - Issue: No import functionality visible
    - Actual: No import button, menu option, or related text found in UI. Searched for "import" in page content and button labels - none found. Available actions do not include any import-related functionality.


## Constraint
- [X] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [X] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [X] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.
  - Bug Report:
    - Issue: Field-specific filtering not implemented
    - Actual: No filter UI, buttons, or actions found for field-specific filtering. Searched all semantic actions - none include "filter". The only filtering mechanism available is global text search (which is different from field-specific filtering). Field-specific filter criteria cannot be defined or applied.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.
  - Bug Report:
    - Issue: Completed field values not displayed in table view
    - Actual: Card view displays Completed checkbox as Yes/No (e.g., entry-1 is Yes, entry-2 is Yes, entry-3 is No). Table view does not display Completed field column values at all. Same entry data should display identically in both views.

- [X] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [X] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.
  - Bug Report:
    - Issue: Search doesn't match formatted dates or checkbox states
    - Actual: Search matches text ("Design" finds "Design updated") and numbers ("10" finds priority 10). But search does NOT match formatted dates (searching "Feb 20" returns 0 results even though entry-1 shows "Feb 20, 2026") or checkbox states (searching "Yes" returns 0 results even though entries show "Yes").


## Interaction
- [X] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [X] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.