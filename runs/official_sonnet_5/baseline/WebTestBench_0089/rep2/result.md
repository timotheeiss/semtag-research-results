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
    - Actual: Searching 'Resme' (missing letter from 'Resume') and 'Meetign' (transposed letters from 'Meeting') both returned 'Showing 0 of 6 templates' / 'No templates found', with no suggestions and no matching templates returned, instead of tolerating the small typo.

- [X] FT-11: Users can add or remove a template from favorites and open a favorites-only list.

- [X] FT-12: Within the favorites-only view, users can search and filter their saved templates.

- [X] FT-13: Users can copy the generated result to the clipboard in the selected template's declared format, with visible success feedback.

- [X] FT-28: Favorites and custom templates remain available after reloading the app in the same browser.


## Constraint
- [ ] CS-14: Fields with a declared format, such as email or date, reject invalid values or keep preview unavailable until the value is valid.
  - Bug Report:
    - Issue: Email field does not validate format
    - Actual: Entered 'invalidemail' (no @ or domain) into the required Email field; the Preview tab became enabled and the preview rendered the invalid string literally ('| invalidemail |') instead of rejecting the value or keeping the preview unavailable.

- [X] CS-16: Custom templates cannot be submitted without a nonblank title, description, author, content, and at least one defined field.

- [ ] CS-17: Values entered into template fields reject or neutralize special characters that would alter generated Markdown structure or formatting.
  - Bug Report:
    - Issue: Markdown-altering special characters not fully neutralized
    - Actual: Entering '# Test Heading **Bold Text**' in the Full Name field: the app blocked characters < > { } [ ] \\ | ` with an error, but allowed '**' through unescaped. In the generated preview, '**Bold Text**' was actually rendered as bold (strong) text inside the H1 heading, meaning user-supplied Markdown syntax altered the generated formatting/structure instead of being neutralized or escaped.

- [X] CS-19: The preview remains unavailable until every required field of the selected template has a nonblank, valid value.

- [ ] CS-27: A custom template can be submitted only when every declared dynamic field maps to a placeholder in its content and every content placeholder maps to a declared field; generated previews contain no unresolved placeholders.
  - Bug Report:
    - Issue: Field/placeholder mapping not validated on submit
    - Actual: Submitted a template with a declared field 'Age' ({{age}}) that has no matching placeholder in the content, and content containing {{name}} twice with no declared 'name' field. Submission succeeded ('Template submitted!'). Opening the template and viewing Preview Result showed the literal unresolved placeholders '{{name}}' in the heading and body instead of being rejected at submission or substituted.

- [ ] CS-29: Every field defined for a custom template remains independently fillable; duplicate labels are rejected or disambiguated before submission.
  - Bug Report:
    - Issue: Duplicate field labels are silently accepted and collide into a single value
    - Actual: Submitted a custom template with two fields both labeled 'Name' (one required, one optional), both mapping to the same {{name}} placeholder, with no warning or rejection at submission. In the Fill Template form, both fields render as separate inputs, but they share the same underlying state: typing 'Alice' into the first Name field and then 'Bob' into the second Name field silently overwrote the first value system-wide - both input boxes displayed 'Bob' afterward. The Preview Result confirmed only 'Bob' was used everywhere ('Memo from Bob', 'Bob says: Team Update'), with 'Alice' silently discarded with no error or disambiguation.


## Interaction
- [X] IX-20: Typing at least two matching characters shows real-time suggestions; choosing a suggestion fills the search and narrows the library to matching templates.

- [X] IX-21: Search queries, category and scenario filters, favorites-only mode, and sort choices update the visible results and count without a reload.

- [X] IX-22: When required selected-template fields are empty, the UI lists the missing fields and keeps preview unavailable.

- [X] IX-24: Favoriting or unfavoriting a template provides immediate visual feedback and updates the favorites count and list.


## Content
- [X] CT-25: Valid plain-text templates preview as preserved text, and valid Markdown templates render expected Markdown after field substitution.

- [X] CT-26: Submitting a valid custom template immediately updates the library and visible result count without a reload.