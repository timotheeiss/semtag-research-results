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
    - Issue: No fuzzy/typo-tolerant matching in search
    - Actual: Typing "resme" (one missing letter from "resume", the "Professional Resume" template) character-by-character produced no suggestion dropdown and "Showing 0 of 6 templates" with an empty-state "No templates found". The intended template was neither returned nor suggested.

- [X] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [X] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [X] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [X] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.


## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.
  - Bug Report:
    - Issue: Declared email-format field accepts invalid value with no validation
    - Actual: Entered "not-an-email" in the required Email field of Professional Resume. No inline error or warning appeared, the "Preview Result" tab became enabled, and the preview rendered the line "Software Engineer | not-an-email | +1 555 123 4567" containing the invalid email.

- [X] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [ ] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.
  - Bug Report:
    - Issue: Markdown-structural characters are neither rejected nor escaped, allowing injected formatting
    - Actual: The blocklist only covers < > { } [ ] \ | ` ("Special characters like < > { } [ ] \ | ` are not allowed"). Entering "Normal summary line.\n\n## Injected Heading\n\n---\n\n**injected bold** and # H1 inject" into Professional Summary raised no error and the preview HTML rendered it as real structure: <h2>Injected Heading</h2><hr><p><strong>injected bold</strong>...</p>, altering the generated document's Markdown structure.

- [ ] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.
  - Bug Report:
    - Issue: Preview gating enforces nonblank but not validity of required fields
    - Actual: Blank gating works (setting Company Name to "   " disabled the Preview tab and listed "Please fill in: Company Name"), but a required Email field holding the invalid value "not-an-email" (input type=email, checkValidity()===false) still enabled the Preview Result tab and produced output, so preview does not stay unavailable until every required field is valid.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.
  - Bug Report:
    - Issue: No placeholder/field consistency validation; preview shows unresolved placeholder
    - Actual: Submitted "QA Daily Note" whose content contained {{name}} and {{status}} while only a "Name" field was declared. Submission succeeded (library grew to 7). Opening it and filling Name produced a preview containing the literal unresolved text "Status: {{status}}".

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.
  - Bug Report:
    - Issue: Duplicate field labels neither rejected nor disambiguated; fields not independently fillable
    - Actual: Added two fields both labeled "Name"; both were accepted and stored with the same id "name" (fields:[{id:"name",placeholder:"Your name"},{id:"name",placeholder:"Duplicate name"}]). In the fill form, typing "Alice" into the first Name input immediately set the second input to "Alice" as well, so they are not independent. A React duplicate-key console error was also emitted.


## Interaction
- [X] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [X] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [X] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [X] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.


## Content
- [X] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [X] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.