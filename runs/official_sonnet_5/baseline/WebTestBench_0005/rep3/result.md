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
    - Actual: Searched the dashboard UI (search box + sort dropdown) and the full DOM for any date filter, date-range picker, or calendar control. None exists - only a title search textbox and a sort-order combobox (Newest/Oldest/Title A-Z/Title Z-A) are available. There is no way to narrow resumes by an exact creation date or a date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid date periods are accepted without rejection or feedback
    - Actual: Set Start Date 2020-01 and End Date 2019-01 (end before start) for a work experience entry - no error/validation message appeared, and the preview happily rendered "Jan 2020 - Jan 2019". Also set End Date to 2027-01 (future date) - also accepted without any warning, preview rendered "Jan 2020 - Jan 2027".

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Application state is not persisted across reloads - resets to initial seed data
    - Actual: Before reload: dashboard had 4 resumes - 'Marketing Manager Application Copy' (created via duplicate+rename), 'QA Engineer - TechCorp Application' (newly created+edited+renamed), 'Product Designer Resume', 'Marketing Manager Resume' (Software Engineer Resume had just been deleted). After navigating to http://localhost:6005/ again (reload), the dashboard reverted to exactly the original 3 seeded resumes ('Product Designer Resume', 'Marketing Manager Resume', 'Software Engineer Resume') - the newly created resume, its duplicate/rename, and the deletion were all lost, and the deleted resume reappeared. All created/edited/duplicated/renamed/deleted changes are NOT retained after reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when renaming to empty/whitespace title
    - Actual: Clearing the resume title to empty string or whitespace-only and pressing Enter silently reverted the title back to 'Untitled Resume' (the previous value) with no toast, inline error, or any visible validation message. Requirement expects 'clear validation feedback' which was absent - the title revert happened silently.


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