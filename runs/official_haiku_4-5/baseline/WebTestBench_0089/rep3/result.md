# Test Result

## Functionality
- [X] FT-1: Users can browse the template library and see the available templates with their authors and descriptions.

- [X] FT-2: Selecting a template opens its customizable form and template details.

- [X] FT-3: Users can enter values for every field defined by the selected template.

- [X] FT-4: After all required fields are valid, users can preview the generated result with their values substituted into the template.

- [X] FT-5: Users can submit a custom plain-text or Markdown template with a title, description, author, category, scenario, content, and at least one defined field.

- [X] FT-6: After a custom template is successfully submitted, it appears in the current browser's template library and can be selected and used.

- [X] FT-7: Each template card displays its title, author, description, category, scenario, usage count, and update date.

- [X] FT-8: For a built-in Markdown template, filling all required fields produces a preview with every declared placeholder replaced and Markdown headings, emphasis, separators, and line breaks rendered correctly.

- [X] FT-9: Users can filter templates by category and scenario and sort results by popularity, update recency, creation age, or title; the visible result count and order update accordingly.

- [ ] FT-10: Search tolerates a small typo in a template keyword and still returns or suggests the intended matching template.
  - Bug Report:
    - Issue: Search does not tolerate typos in template keywords
    - Actual: Searched for "Profesional" (missing 's' in Professional) and got "Showing 0 of 8 templates" with "No templates found" message. Should have returned Professional Resume or suggested the correct spelling.

- [X] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [X] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [X] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [X] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.


## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.
  - Bug Report:
    - Issue: Email format validation is not enforced - preview remains available with invalid email
    - Actual: Entered invalid email format "invalid-email" in the Email field. The Preview Result tab became enabled and remains clickable, indicating no format validation is preventing preview generation for invalid email values. Expected: Preview should remain unavailable or field should show validation error for invalid email format.

- [X] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [X] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.

- [X] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.
  - Bug Report:
    - Issue: Template with field-placeholder mismatch was submitted and shows unresolved placeholder in preview
    - Actual: Submitted template "Test Mismatch Template" with content "Hello {{name}}, this is a test" and only "Email" field defined. Preview shows unresolved {{name}} placeholder. Content has {{name}} with no matching field, and Email field has no matching {{email}} placeholder in content.

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.
  - Bug Report:
    - Issue: Duplicate field labels are neither rejected nor disambiguated before submission
    - Actual: Created two fields with the same label "Name". Both were accepted without error. The form now shows two identical fields labeled "Name" both mapping to {{name}} placeholder. No rejection message or disambiguation was applied.


## Interaction
- [X] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [X] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [X] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [X] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.


## Content
- [X] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [X] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.