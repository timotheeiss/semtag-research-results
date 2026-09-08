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
    - Actual: Entering "not-a-date" into the required "Start Date" field (and "also-not-a-date" into End Date) was accepted without any validation error; fields are free text with no date format enforcement.

- [ ] CS-15: The start date entered must be earlier than the end date.
  - Bug Report:
    - Issue: No start/end date order validation
    - Actual: Set Start Date="Jan 2025" and End Date="Jan 2020" (start after end); no validation error or warning appeared, form did not block this input.

- [X] CS-16: Users cannot submit templates with empty required fields (template content, title, author name).

- [ ] CS-17: Form input fields restrict special characters (such as ￥#@%^*, etc.), automatically filtering or displaying a "Special characters are prohibited" message when inputting.
  - Bug Report:
    - Issue: Special characters not restricted
    - Actual: Typing "John@#$%Doe" into the required "Full Name" field was accepted verbatim with no filtering, error message, or "Special characters are prohibited" warning.

- [ ] CS-18: When submitting a Markdown template, validate its basic syntax to prevent rendering errors.
  - Bug Report:
    - Issue: No Markdown syntax validation on submission
    - Actual: Submitted a template with deliberately malformed Markdown (unclosed code fence, unclosed bold marker, broken link syntax, malformed table) and it was accepted without any warning — toast showed "Template submitted! Your template has been added to the library." and it appeared in the library as "Broken Markdown Test".

- [X] CS-19: If the required form fields of the selected template are not filled in, the user will not be able to generate a preview.


## Interaction
- [ ] IX-20: As users enter keywords in the search box, the webpage displays suggested results in real time. Clicking on a suggested result will redirect to the corresponding template.
  - Bug Report:
    - Issue: Suggestion click does not redirect to template
    - Actual: Real-time suggestions do appear while typing, but clicking a suggestion ("Professional Resume") only filled the search box with that text and filtered the grid to show it; it did not open/redirect to the template's detail/fill dialog.

- [X] IX-21: Search results are displayed in real time as users filter.

- [X] IX-22: If the required form information is not filled in, provide visual feedback indicating that submission is not possible.

- [ ] IX-23: When entering a special character, provide visual feedback indicating that the input is not possible or that special characters cannot be entered.
  - Bug Report:
    - Issue: No visual feedback for special character input
    - Actual: Entering special characters (@#$%) into the Full Name field produced no visual feedback (no error styling, no toast, no blocked input); characters were simply accepted.

- [X] IX-24: Provide visual feedback when clicking the "Favorite" button.


## Content
- [X] CT-25: The template content (plain text or Markdown) renders correctly during preview and use, without any formatting errors.

- [X] CT-26: Once a new template is submitted, the template library will update and display it immediately, and the list status will remain synchronized.