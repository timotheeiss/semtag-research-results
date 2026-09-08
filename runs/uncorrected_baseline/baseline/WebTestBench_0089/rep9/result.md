# Test Result

## Functionality
- [X] FT-1: Users can browse the template library list, where each template displays basic information such as author and description.

- [X] FT-2: Users can select templates

- [X] FT-3: Users can fill in the form related information.

- [X] FT-4: Users can preview the completed form.

- [X] FT-5: Users can submit custom templates (plain text or Markdown format) and basic information through a form.

- [X] FT-6: Once a user-submitted custom template is successfully saved, it will appear in the public template library for others to use.

- [X] FT-7: Each template displays basic information: template name, author, description, type tag, and update time.

- [X] FT-8: The result displayed after the user fills out the form is consistent with the expected result in Markdown.

- [X] FT-9: Users can filter by template type, applicable scenarios, popularity, and update time.

- [X] FT-10: The web search box supports widgets for fuzzy matching.

- [X] FT-11: The website supports template saving function.

- [X] FT-12: Users can view and search for saved templates in the collection list.

- [X] FT-13: Supports copying the previewed results (one-click copying of plain text/Markdown formats).


## Constraint
- [ ] CS-14: The information entered must conform to the format requirements; for example, the date must be a correct date.
  - Bug Report:
    - Issue: No date format validation
    - Actual: Entered "2025-13-45" (invalid month/day) into Start Date field; no error was shown and the Preview tab became enabled, indicating no date format validation.

- [ ] CS-15: The start date entered must be earlier than the end date.
  - Bug Report:
    - Issue: No start/end date order validation
    - Actual: Entered Start Date "2025-13-45" and End Date "2020-01-01" (end before start); no validation error appeared and Preview tab was enabled.

- [X] CS-16: Users cannot submit templates with empty required fields (template content, title, author name).

- [ ] CS-17: Form input fields restrict special characters (such as ￥#@%^*, etc.), automatically filtering or displaying a "Special characters are prohibited" message when inputting.
  - Bug Report:
    - Issue: Special characters not filtered or blocked
    - Actual: Typing "John#@%Doe" into the Full Name field was accepted verbatim with no filtering and no "Special characters are prohibited" message.

- [ ] CS-18: When submitting a Markdown template, validate its basic syntax to prevent rendering errors.
  - Bug Report:
    - Issue: No Markdown syntax validation before submission
    - Actual: Submitted template content included malformed Markdown ("[Broken link(http://example.com" - invalid link syntax), yet the submission succeeded with toast "Template submitted!" and no syntax validation error/warning was shown.

- [X] CS-19: If the required form fields of the selected template are not filled in, the user will not be able to generate a preview.


## Interaction
- [ ] IX-20: As users enter keywords in the search box, the webpage displays suggested results in real time. Clicking on a suggested result will redirect to the corresponding template.
  - Bug Report:
    - Issue: Suggestion click does not redirect to the template
    - Actual: Clicking the "Professional Resume" suggestion only filled the search box and filtered the list to show that one template; it did not open/navigate to the template's detail or fill page (URL and view remained the library page).

- [X] IX-21: Search results are displayed in real time as users filter.

- [X] IX-22: If the required form information is not filled in, provide visual feedback indicating that submission is not possible.

- [ ] IX-23: When entering a special character, provide visual feedback indicating that the input is not possible or that special characters cannot be entered.
  - Bug Report:
    - Issue: No visual feedback when entering special characters
    - Actual: Entering special characters (#@%) in Full Name field produced no warning, highlight, or blocked input.

- [X] IX-24: Provide visual feedback when clicking the "Favorite" button.


## Content
- [X] CT-25: The template content (plain text or Markdown) renders correctly during preview and use, without any formatting errors.

- [X] CT-26: Once a new template is submitted, the template library will update and display it immediately, and the list status will remain synchronized.