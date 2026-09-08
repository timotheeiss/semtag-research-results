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
    - Issue: No date filter control exists on the dashboard
    - Actual: The dashboard only offers a title search box (dashboard.search) and a sort dropdown (Newest/Oldest/Title A-Z/Title Z-A). There is no control to filter/narrow resumes by an exact creation date or a date range anywhere on the page (full accessibility snapshot confirms no such input exists).


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No validation for invalid experience date ranges
    - Actual: Set experience start date to 2020-01 and end date to 2019-01 (end before start). No error message or validation feedback appeared; the invalid value was accepted, persisted across tab switches, and rendered in preview as 'Jan 2020 - Jan 2019' without rejection.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data is not persisted across page reloads
    - Actual: Created 'QA Test Resume' with full contact/summary/experience/education/skills content, edited it, renamed it, duplicated it (renaming and editing the copy), and deleted the copy. After reloading the application (browser_navigate to the same URL), 'QA Test Resume' had completely disappeared from the dashboard; only the original 3 seed resumes (Product Designer, Marketing Manager, Software Engineer) remained. All created/edited/renamed/duplicated work was lost on reload, and it could not be verified whether the deletion itself would have persisted since the whole resume vanished regardless.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when renaming to whitespace-only title
    - Actual: Editing the resume title to '   ' (whitespace only) and pressing Enter correctly kept the title as 'Untitled Resume' (did not save the empty title), but no error message, toast, or any visible validation feedback was shown to the user; the field simply silently reverted to display mode.


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
    - Issue: Contact section's customizable heading is never rendered in preview
    - Actual: The Sections editor allows customizing the Contact section heading (editor.sections.item.contact.title, changed to 'MY CONTACT DETAILS'). This custom text never appeared in the preview for any of the three styles (Modern, Classic, Minimal), even though the Contact section itself is visible with content. Other section headings (Summary, Education, Experience, Skills) did render their customized text correctly in Modern/Classic, but Minimal also omitted the Summary section heading entirely while still showing the summary text.