# Test Checklist

## Functionality
- [ ] FT-1: Users can create an entry with values for configured fields; the new entry appears immediately and remains present after the page is reloaded.

- [ ] FT-2: Users can add fields and choose text, number, date, or checkbox as each field's type; the new field appears in entry forms and both data views.

- [ ] FT-3: Users can enter and save a text string, number, calendar date, and checkbox status for an entry, and the saved values are retained and shown in both data views.

- [ ] FT-4: Users can switch between table and card views without changing the current entries or their field values.

- [ ] FT-5: Users can sort the current view by any configured field in ascending or descending order; table and card views retain independent sort selections while the user switches between them.

- [ ] FT-6: Users can define field-specific filter criteria, view only matching entries, and clear the filters to restore the unfiltered data.

- [ ] FT-7: When no field sort is active, users can manually reorder entries in table and card views; each view retains its own manual order while the user switches between them.

- [ ] FT-19: Users can import data after defining a field schema, and the app rejects imported values that do not match the configured field types without corrupting existing data.

## Constraint
- [ ] CS-8: A number field accepts valid numeric values and prevents a non-numeric value from being saved as that field's value.

- [ ] CS-9: A date field accepts valid calendar dates and prevents an invalid date value from being saved as that field's value.

- [ ] CS-11: Sorting changes only the order of entries; it preserves the entry count and every saved field value.

- [ ] CS-12: Filtering includes exactly the entries that satisfy the active criteria; clearing the filters restores all entries without modifying or deleting data.

- [ ] CS-13: Every entry and each configured field value are displayed consistently in both table and card views, including a clear representation for an empty value.

- [ ] CS-14: Field-type choices are clearly labeled during configuration, and entry forms provide type-appropriate controls for text, number, date, and checkbox fields.

- [ ] CS-15: Checkbox values have clearly distinguishable checked and unchecked states in entry forms and both data views.

- [ ] CS-20: Quick search matches the field values shown to users, including formatted dates and checkbox states, rather than only internal raw values.

## Interaction
- [ ] IX-17: Quick search returns entries whose text values contain the query case-insensitively, updates the visible result count, and restores all entries when cleared.

- [ ] IX-18: Selecting a sort field immediately reorders the visible entries, identifies the active field and direction, and leaves the app usable.