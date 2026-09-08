# Test Checklist

## Functionality
- [ ] FT-1: Users can browse the template library list, where each template displays basic information such as author and description.

- [ ] FT-2: Users can select templates

- [ ] FT-3: Users can fill in the form related information.

- [ ] FT-4: Users can preview the completed form.

- [ ] FT-5: Users can submit custom templates (plain text or Markdown format) and basic information through a form.

- [ ] FT-6: Once a user-submitted custom template is successfully saved, it will appear in the public template library for others to use.

- [ ] FT-7: Each template displays basic information: template name, author, description, type tag, and update time.

- [ ] FT-8: The result displayed after the user fills out the form is consistent with the expected result in Markdown.

- [ ] FT-9: Users can filter by template type, applicable scenarios, popularity, and update time.

- [ ] FT-10: The web search box supports widgets for fuzzy matching.

- [ ] FT-11: The website supports template saving function.

- [ ] FT-12: Users can view and search for saved templates in the collection list.

- [ ] FT-13: Supports copying the previewed results (one-click copying of plain text/Markdown formats).

## Constraint
- [ ] CS-14: The information entered must conform to the format requirements; for example, the date must be a correct date.

- [ ] CS-15: The start date entered must be earlier than the end date.

- [ ] CS-16: Users cannot submit templates with empty required fields (template content, title, author name).

- [ ] CS-17: Form input fields restrict special characters (such as ￥#@%^*, etc.), automatically filtering or displaying a "Special characters are prohibited" message when inputting.

- [ ] CS-18: When submitting a Markdown template, validate its basic syntax to prevent rendering errors.

- [ ] CS-19: If the required form fields of the selected template are not filled in, the user will not be able to generate a preview.

## Interaction
- [ ] IX-20: As users enter keywords in the search box, the webpage displays suggested results in real time. Clicking on a suggested result will redirect to the corresponding template.

- [ ] IX-21: Search results are displayed in real time as users filter.

- [ ] IX-22: If the required form information is not filled in, provide visual feedback indicating that submission is not possible.

- [ ] IX-23: When entering a special character, provide visual feedback indicating that the input is not possible or that special characters cannot be entered.

- [ ] IX-24: Provide visual feedback when clicking the "Favorite" button.

## Content
- [ ] CT-25: The template content (plain text or Markdown) renders correctly during preview and use, without any formatting errors.

- [ ] CT-26: Once a new template is submitted, the template library will update and display it immediately, and the list status will remain synchronized.