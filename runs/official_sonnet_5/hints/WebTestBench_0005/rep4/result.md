# Test Result

## Functionality
- [X] FT-1: Users can create a new resume with blank contact, summary, experience, education, and skills content and begin editing it immediately.

- [X] FT-2: Users can choose among the Modern, Classic, and Minimal resume styles; changing the style updates the preview without losing the resume's content.

- [X] FT-3: Users can enter and edit contact details and a professional summary, add, edit, remove, and reorder work and education entries, and add, change, or remove skills.

- [X] FT-4: Users can change the order of resume sections and hide or show individual sections, and the preview reflects the resulting layout.

- [X] FT-5: Users can view the active resume in a preview, hide the preview, and show it again without losing editor content or the selected template.

- [X] FT-6: Users can duplicate an existing resume, give the copy a different title, and edit the copy without changing the source resume.

- [X] FT-7: Users can create and manage multiple distinct resume variations for different applications during the current session.

- [X] FT-8: Users can search saved resumes by title without case sensitivity and order the results by creation date from newest to oldest or oldest to newest; clearing the search restores all resumes.

- [ ] FT-19: Users can narrow saved resumes by an exact creation date or a creation-date range and clear the date criterion to restore all matching titles.
  - Bug Report:
    - Issue: No date-based filtering feature exists
    - Actual: The dashboard only offers a title search box and a sort dropdown (Newest/Oldest/Title A-Z/Z-A). There is no UI control, hidden or visible, to filter resumes by an exact creation date or a date range; a full-page text/DOM scan found no date-filter inputs anywhere on the dashboard.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid date periods are not rejected or flagged
    - Actual: Setting an experience end date (2019-01) before its start date (2020-01) produced no validation error, no aria-invalid, no styling change, and no message anywhere on the page. Setting a start date in the future (2027-01) likewise produced no feedback. Values were silently accepted.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No data persistence across reload
    - Actual: After creating 'Software Engineer Resume v2', editing its content, duplicating it, and renaming the copy to 'Product Manager Application Resume', reloading the app (navigating to http://localhost:6005/) reset the dashboard to only the 3 original seed resumes (Software Engineer Resume, Marketing Manager Resume, Product Designer Resume). Both localStorage and sessionStorage are empty, confirming no persistence mechanism is used; all created/edited/duplicated/renamed resumes were lost on reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback on empty/whitespace rename
    - Actual: Attempting to rename the resume to a whitespace-only string or an empty string is rejected — the title reverts to 'Software Engineer Resume v2' — but no error message, toast, inline text, or any other validation feedback is shown to the user anywhere on the page.


## Interaction
- [X] IX-12: Changes to resume fields and the selected template appear in the visible preview immediately, without requiring a manual save or refresh.

- [X] IX-13: Changing a section's order or visibility updates the preview immediately and preserves the new layout while navigating within the current session.

- [X] IX-14: Resume edits are retained automatically when the user moves between editor areas, returns to the dashboard, and reopens the resume during the current session; the dashboard reflects that the resume was updated.


## Content
- [X] CT-15: For the selected style, the preview renders every visible section that has content and keeps the complete resume readable through scrolling when it exceeds the available preview area.

- [X] CT-16: Contact details, summary, work experience, education, and skills entered for a resume remain present when switching editor areas, returning to the dashboard, and reopening that resume during the current session.

- [X] CT-17: After a resume is duplicated, editing any content in the copy does not alter the corresponding content in the original, and editing the original does not alter the copy.

- [X] CT-18: After a resume is renamed, the same non-empty title is shown in its editor and on its dashboard card.

- [ ] CT-20: Every section heading that the editor allows the user to customize is shown with that customized text in each resume style where the section is visible.
  - Bug Report:
    - Issue: Customized section heading not rendered in one style
    - Actual: Renamed Summary heading to 'Career Summary' (and other headings similarly). Modern and Classic templates correctly render 'Career Summary' as the Summary section heading, but the Minimal template renders the Summary text without any heading at all (no preview.summary.heading element present), even though the section is visible and has content.