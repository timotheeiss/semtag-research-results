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
    - Issue: No date filter feature exists on the dashboard
    - Actual: Inspected the entire dashboard UI (search box, sort dropdown, and full DOM query for all inputs/buttons). Only a text 'Search resumes...' input and a 'Newest First' sort combobox (with Newest First/Oldest First/Title A-Z/Title Z-A options) are present. There is no date picker, date range selector, filter-by-date button, or any other control allowing users to filter saved resumes by exact creation date or a date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date validation for invalid experience periods
    - Actual: Set experience end date (2020-01) earlier than start date (2021-03), and separately set start date to a future month (2027-01); in both cases no error message, alert, or aria-invalid state appeared anywhere on the page, and the invalid values were silently accepted into the field.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume changes are not persisted across a page reload
    - Actual: Renamed 'Software Engineer Resume' to 'Reload Persistence Test Title' (confirmed applied in editor header). Performed a full page reload (navigating to http://localhost:6005/). After reload, the resume reverted entirely to its original title 'Software Engineer Resume' and original 'Updated Jan 20, 2024' timestamp - the rename was completely lost. Separately, a newly created resume ('Untitled Resume' renamed to 'Senior Engineer Resume - TechCorp App' with full contact/summary/experience/education/skills content) disappeared entirely from the dashboard after a page reload, confirming state is held only in-memory/session and not persisted to any storage (localStorage/backend) across reloads.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback when renaming to empty/whitespace title
    - Actual: Clearing the resume title to whitespace-only and pressing Enter silently reverted the title back to 'Untitled Resume' (the previous title was preserved, which is correct), but no error message, toast notification, or any other validation feedback was shown to the user. Checked for [role=alert], .text-red-500, .text-destructive, [class*=error] elements and body text for 'cannot'/'required'/'empty' - none found.


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