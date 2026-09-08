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
    - Issue: Date filtering functionality not implemented. No date picker, date range input, or date filter controls found on the dashboard.
    - Actual: Dashboard shows only title search and sort options. No date filtering UI elements are available to narrow resumes by creation date or date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Date validation not enforced. System accepts invalid date ranges (end before start) and future dates without any error feedback.
    - Actual: Changed Position 2 end date from 2023-12 to 2019-12 (before start date 2020-01) - accepted without error. Changed to 2027-01 (future date) - also accepted. No validation error messages displayed, and preview renders both invalid ranges.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: New resumes created and edited during session are not persisted after page reload. Only original resumes persist.
    - Actual: Before reload: 5 resumes visible (Tech Company Resume, Untitled Resume, Product Designer Resume, Marketing Manager Resume, Software Engineer Resume). After reload: only 3 original resumes remain (Product Designer, Marketing Manager, Software Engineer). The newly created Tech Company Resume and Untitled Resume are missing.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Resume can be renamed to empty string without validation. No error feedback is provided and the empty title is accepted.
    - Actual: Opened rename dialog for "Product Designer Resume", cleared the title field to empty string, clicked Save. Dialog closed and resume title became empty (no text displayed). No validation error message or clear feedback was provided. Empty title was persisted on dashboard.


## Interaction
- [X] IX-12: Changes to resume fields and the selected template appear in the visible preview immediately, without requiring a manual save or refresh.

- [X] IX-13: Changing a section's order or visibility updates the preview immediately and preserves the new layout while navigating within the current session.

- [X] IX-14: Resume edits are retained automatically when the user moves between editor areas, returns to the dashboard, and reopens the resume during the current session; the dashboard reflects that the resume was updated.


## Content
- [X] CT-15: For the selected style, the preview renders every visible section that has content and keeps the complete resume readable through scrolling when it exceeds the available preview area.

- [X] CT-16: Contact details, summary, work experience, education, and skills entered for a resume remain present when switching editor areas, returning to the dashboard, and reopening that resume during the current session.

- [X] CT-17: After a resume is duplicated, editing any content in the copy does not alter the corresponding content in the original, and editing the original does not alter the copy.

- [X] CT-18: After a resume is renamed, the same non-empty title is shown in its editor and on its dashboard card.

- [X] CT-20: Every section heading that the editor allows the user to customize is shown with that customized text in each resume style where the section is visible.