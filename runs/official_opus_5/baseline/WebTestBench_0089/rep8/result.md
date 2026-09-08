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
    - Issue: Search has no typo tolerance / fuzzy matching
    - Actual: Query "resme" (typo of "resume") → "Showing 0 of 6 templates", "No templates found", no suggestions. Query "meting" (typo of "meeting") → same 0 results with no suggestion of "Meeting Minutes".

- [X] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [X] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [X] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [X] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.


## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.
  - Bug Report:
    - Issue: Declared email-format field does not validate value
    - Actual: Email input (type=email) accepted "not-an-email": no error, no aria-invalid, Preview tab became enabled and preview rendered "Software Engineer | not-an-email | +1 234 567 8900".

- [X] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [ ] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.
  - Bug Report:
    - Issue: Markdown-structural characters (#, *) not rejected or escaped
    - Actual: Field validation only blocks < > { } [ ] \ | `. Entering "## Injected Heading and **bold**" in Professional Summary was accepted, preview stayed enabled, and the preview rendered an injected H2 element plus STRONG — generated Markdown structure altered by user input.

- [X] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.
  - Bug Report:
    - Issue: No placeholder/field consistency validation on custom template submission
    - Actual: Submitted a template whose content uses {{heading}} and {{who}} while the only declared field was "Nonexistent" ({{nonexistent}}). Submission succeeded (library 6→7) and the generated preview shows unresolved placeholders: "{{heading}} / Hello {{who}}, / Regards."

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.
  - Bug Report:
    - Issue: Duplicate field labels neither rejected nor disambiguated; fields not independently fillable
    - Actual: Adding two fields both labeled "Subject" was accepted (both mapped to {{subject}}) and the template submitted. In the fill form both inputs share id "subject": typing "First Subject Value" into one instantly set the same value in the other; console also logged errors.


## Interaction
- [X] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [X] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [X] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [X] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.


## Content
- [X] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [X] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.