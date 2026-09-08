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
    - Actual: Inspected the dashboard (including hidden elements) — only a title search box and a sort-order dropdown (Newest/Oldest/Title A-Z/Z-A) are present. There is no control to filter by an exact creation date or a date range, and no way to clear such a criterion, since it doesn't exist.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid date range (end before start) is accepted without rejection or feedback
    - Actual: Set Start Date=2020-01, End Date=2019-01 on a work experience entry. No validation message, error styling, or aria-invalid appeared; the value was accepted and the preview rendered "Jan 2020 Jan 2019" as-is instead of rejecting the invalid period.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No changes persist across a page reload
    - Actual: Before reload, dashboard had 4 resumes: original 3 defaults minus deleted "Product Designer Resume", plus created "Software Engineer - TechCo Application" and its renamed duplicate "Software Engineer - Beta Corp Application". After reloading http://localhost:6005/, the dashboard reverted to exactly the original 3 default resumes (Software Engineer Resume, Marketing Manager Resume, Product Designer Resume) — the newly created resume and its duplicate/rename were gone, and the deleted "Product Designer Resume" reappeared. None of create, edit, duplicate, rename, or delete operations survived a reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only title is rejected silently with no validation feedback
    - Actual: Entered "   " (whitespace only) into the resume title field and confirmed with both Tab and Enter. The title correctly reverted to "Untitled Resume" (previous value preserved), but no toast, inline error message, or any other validation feedback was shown to the user (Notifications regions remained empty).


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
    - Issue: Customized section heading not rendered in Minimal style
    - Actual: Renamed Summary heading to "About Me" and Experience heading to "Career History". In Modern and Classic styles both customized headings render correctly. In Minimal style, "Career History" renders but the Summary section shows only the paragraph text with no heading at all — the customized "About Me" title is missing even though the section is visible.