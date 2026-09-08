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
    - Issue: Date filtering feature not found in UI. Only search and sort controls are available, no date filter controls visible.
    - Actual: Dashboard shows search box and sort dropdown. No date filter UI element found for filtering by exact creation date or date range. FT-19 requirement specifies this capability but it is not implemented.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Date validation not enforced. End date (2019-12) was accepted even though it precedes the start date (2020-01). No validation error or feedback was shown, and the invalid period was displayed in the preview.
    - Actual: Able to set end date December 2019 for an entry that starts January 2020. Preview displayed "Jan 2020 to Dec 2019" (backwards period) without any error messages or rejection.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resumes not persisting after page reload. The "Untitled Resume" and "Untitled Resume (Copy)" created during the session disappeared after reloading the page, leaving only the original 3 pre-loaded resumes.
    - Actual: After reload, only 3 original resumes visible (Product Designer, Marketing Manager, Software Engineer). The 2 new resumes created in the session are no longer present on the dashboard.

- [X] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.


## Interaction
- [X] IX-12: Changes to resume fields and the selected template appear in the visible preview immediately, without requiring a manual save or refresh.

- [X] IX-13: Changing a section's order or visibility updates the preview immediately and preserves the new layout while navigating within the current session.

- [X] IX-14: Resume edits are retained automatically when the user moves between editor areas, returns to the dashboard, and reopens the resume during the current session; the dashboard reflects that the resume was updated.


## Content
- [X] CT-15: For the selected style, the preview renders every visible section that has content and keeps the complete resume readable through scrolling when it exceeds the available preview area.

- [X] CT-16: Contact details, summary, work experience, education, and skills entered for a resume remain present when switching editor areas, returning to the dashboard, and reopening that resume during the current session.

- [X] CT-17: After a resume is duplicated, editing any content in the copy does not alter the corresponding content in the original, and editing the original does not alter the copy.

- [ ] CT-18: After a resume is renamed, the same non-empty title is shown in its editor and on its dashboard card.
  - Bug Report:
    - Issue: Resume rename changes not persisting. Attempted to rename "Untitled Resume (Copy)" to "Finance Manager Resume" via rename dialog, but the title remained unchanged on dashboard and in editor.
    - Actual: Opened rename dialog, changed title to "Finance Manager Resume", clicked Save, but the resume still displayed as "Untitled Resume (Copy)" on dashboard and in editor. Rename changes did not persist.

- [ ] CT-20: Every section heading that the editor allows the user to customize is shown with that customized text in each resume style where the section is visible.
  - Bug Report:
    - Issue: Custom section heading not displayed in preview. Changed "Professional Summary" heading text to "About Me" in the Sections tab, but the preview still showed "Professional Summary" instead of the customized text.
    - Actual: Edited section heading name field in Sections tab from "Professional Summary" to "About Me", but the preview heading remained as "Professional Summary" without reflecting the custom text.