# Test Checklist

## Functionality
- [ ] FT-1: Users can browse the template library and see the available templates with their authors and descriptions.

- [ ] FT-2: Selecting a template opens its customizable form and template details.

- [ ] FT-3: Users can enter values for every field defined by the selected template.

- [ ] FT-4: After all required fields are valid, users can preview the generated result with their values substituted into the template.

- [ ] FT-5: Users can submit a custom plain-text or Markdown template with a title, description, author, category, scenario, content, and at least one defined field.

- [ ] FT-6: After a custom template is successfully submitted, it appears in the current browser's template library and can be selected and used.

- [ ] FT-7: Each template card displays its title, author, description, category, scenario, usage count, and update date.

- [ ] FT-8: For a built-in Markdown template, filling all required fields produces a preview with every declared placeholder replaced and Markdown headings, emphasis, separators, and line breaks rendered correctly.

- [ ] FT-9: Users can filter templates by category and scenario and sort results by popularity, update recency, creation age, or title; the visible result count and order update accordingly.

- [ ] FT-10: Search tolerates a small typo in a template keyword and still returns or suggests the intended matching template.

- [ ] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [ ] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [ ] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [ ] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.

## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.

- [ ] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [ ] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.

- [ ] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.

## Interaction
- [ ] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [ ] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [ ] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [ ] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.

## Content
- [ ] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [ ] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.