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
    - Issue: Search does not tolerate typos
    - Actual: Typing 'Resme' (single-letter-deletion typo of 'Resume') and 'Meetign' (transposition typo of 'Meeting') both returned 'Showing 0 of 6 templates' with 'No templates found' and no suggestions, instead of matching Professional Resume / Meeting Minutes.

- [X] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [X] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [X] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [X] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.


## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.
  - Bug Report:
    - Issue: Email field format not validated
    - Actual: Entering 'not-an-email' as the Email* value (a field with a declared email format) did not block the Preview Result tab or prevent generation; preview.result rendered 'Jane Doe Software Engineer | not-an-email | ...' with the invalid string embedded, instead of rejecting the value or keeping the preview unavailable.

- [X] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [ ] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.
  - Bug Report:
    - Issue: Markdown-structural characters (#, *, -) in field values are not rejected/neutralized, allowing them to alter generated Markdown formatting
    - Actual: The app does block characters < > { } [ ] \\ | ` with error 'Special characters like < > { } [ ] \\ | ` are not allowed' (e.g. entering '<script>alert(1)</script>' in Full Name triggered this validation). However, entering '# Injected Heading\\n\\n---\\n\\n**Bold Injected**' (using only #, *, - which are not in the blocked set) was accepted with no error, and the generated Markdown preview rendered it as an actual h1 heading containing raw '# Injected Heading ---' text plus a bold <strong>Bold Injected</strong> element inside the name heading, and elsewhere the Start Date value '2020' was wrapped in literal asterisks '*2020 - *' — demonstrating the user-entered Markdown syntax altered the document structure instead of being neutralized/escaped.

- [ ] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.
  - Bug Report:
    - Issue: Preview becomes available with an invalid (non-blank but malformed) required field value
    - Actual: With Email set to the invalid value 'not-an-email' (all other required fields non-blank), the Preview Result tab became enabled and generated content immediately, rather than remaining unavailable until the value was valid. Blank-required-field case correctly kept the tab disabled, but validity was not enforced.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.
  - Bug Report:
    - Issue: Custom template submission accepted with unmapped placeholder; generated preview contains unresolved placeholder
    - Actual: Submitted a custom template whose content included {{hiringManager}} with no corresponding declared field (only 'name' and 'company' fields were defined). Submission succeeded (library went from 6 to 7 templates) instead of being rejected. Using the template and filling name/company, the generated preview literally showed 'Dear {{hiringManager}},' — an unresolved placeholder — instead of blocking submission or resolving it.

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.
  - Bug Report:
    - Issue: Duplicate field labels are silently accepted, not rejected or disambiguated
    - Actual: Adding two custom-template fields both labeled 'Recipient' resulted in two entries in submit.fields both with id/key 'recipient' and label 'Recipient' — no error, warning, or automatic disambiguation (e.g. renaming to 'Recipient 2') occurred; the Add Field action succeeded both times.


## Interaction
- [X] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [X] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [X] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [X] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.


## Content
- [X] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [X] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.