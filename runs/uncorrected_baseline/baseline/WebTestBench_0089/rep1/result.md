# Test Result

## Functionality
- [X] FT-1: Users can browse the template library list, where each template displays basic information such as author and description.

- [X] FT-2: Users can select templates

- [X] FT-3: Users can fill in the form related information.

- [X] FT-4: Users can preview the completed form.

- [X] FT-5: Users can submit custom templates (plain text or Markdown format) and basic information through a form.

- [X] FT-6: Once a user-submitted custom template is successfully saved, it will appear in the public template library for others to use.

- [X] FT-7: Each template displays basic information: template name, author, description, type tag, and update time.

- [ ] FT-8: The result displayed after the user fills out the form is consistent with the expected result in Markdown.
  - Bug Report:
    - Issue: Markdown output has formatting defect when optional field is empty
    - Actual: When End Date was left empty, the preview rendered the literal text '*Jan 2020 - *' (unprocessed asterisks) instead of italicized 'Jan 2020 - ' or omitting the trailing dash, indicating the generated Markdown is not always valid/consistent with expected rendering.

- [X] FT-9: Users can filter by template type, applicable scenarios, popularity, and update time.

- [X] FT-10: The web search box supports widgets for fuzzy matching.

- [X] FT-11: The website supports template saving function.

- [X] FT-12: Users can view and search for saved templates in the collection list.

- [X] FT-13: Supports copying the previewed results (one-click copying of plain text/Markdown formats).


## Constraint
- [ ] CS-14: The information entered must conform to the format requirements; for example, the date must be a correct date.
  - Bug Report:
    - Issue: No date format validation
    - Actual: Entered 'not-a-date' into the required 'Start Date' field; form accepted it, Preview tab enabled, and preview rendered 'not-a-date - 2019' without any validation error.

- [ ] CS-15: The start date entered must be earlier than the end date.
  - Bug Report:
    - Issue: No start/end date ordering validation
    - Actual: Entered Start Date 'not-a-date' and End Date '2019' (chronologically before/inconsistent with start); no validation error was raised and preview generated successfully with 'not-a-date - 2019'.

- [X] CS-16: Users cannot submit templates with empty required fields (template content, title, author name).

- [ ] CS-17: Form input fields restrict special characters (such as ￥#@%^*, etc.), automatically filtering or displaying a "Special characters are prohibited" message when inputting.
  - Bug Report:
    - Issue: No special character restriction
    - Actual: Typed 'JavaScript ￥#@%^*' into the Skills field; all special characters were accepted without filtering, no warning/error message was shown, and the field allowed the form to become valid for preview.

- [ ] CS-18: When submitting a Markdown template, validate its basic syntax to prevent rendering errors.
  - Bug Report:
    - Issue: No Markdown syntax validation on submission
    - Actual: Submitted template content containing an unclosed code fence, an unclosed markdown link, and unclosed bold markers; the app accepted it without any syntax warning. On preview, this malformed content caused broken rendering: the {{title}} placeholder was left unreplaced, the intended heading/paragraph text collapsed into a single garbled inline-code block ('unclosed code block [bad link]( **unclosed bold Hello'), and the {{name}} field placeholder vanished — a clear rendering error that should have been caught before submission.

- [X] CS-19: If the required form fields of the selected template are not filled in, the user will not be able to generate a preview.


## Interaction
- [ ] IX-20: As users enter keywords in the search box, the webpage displays suggested results in real time. Clicking on a suggested result will redirect to the corresponding template.
  - Bug Report:
    - Issue: Suggestion click does not redirect to the template
    - Actual: Typing 'resu' showed a 'Professional Resume' suggestion button; clicking it only auto-filled the search box with the full title and kept the user on the filtered list view. It did not redirect/open the corresponding template's fill/preview dialog as expected.

- [X] IX-21: Search results are displayed in real time as users filter.

- [X] IX-22: If the required form information is not filled in, provide visual feedback indicating that submission is not possible.

- [ ] IX-23: When entering a special character, provide visual feedback indicating that the input is not possible or that special characters cannot be entered.
  - Bug Report:
    - Issue: No visual feedback for special character input
    - Actual: After entering special characters (￥#@%^*) into the Skills field, no visual indication (error border, message, or blocked input) was shown; characters were accepted silently.

- [X] IX-24: Provide visual feedback when clicking the "Favorite" button.


## Content
- [ ] CT-25: The template content (plain text or Markdown) renders correctly during preview and use, without any formatting errors.
  - Bug Report:
    - Issue: Formatting artifact in generated preview
    - Actual: With End Date left blank, the Experience date line rendered as literal '*Jan 2020 - *' (raw asterisks visible) rather than properly formatted italic text, i.e. a formatting error in the generated content.

- [X] CT-26: Once a new template is submitted, the template library will update and display it immediately, and the list status will remain synchronized.