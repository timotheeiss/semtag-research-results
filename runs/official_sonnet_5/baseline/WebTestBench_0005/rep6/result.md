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
    - Issue: No UI control exists to filter resumes by exact creation date or date range
    - Actual: The dashboard only offers a title-text search box and a sort combobox (Newest First/Oldest First/Title A-Z/Title Z-A). Entering a date value (e.g. '2024-02-10' or 'Feb 10, 2024', matching an actual resume's displayed 'Updated' date) into the search box returns 'No resumes found' rather than narrowing to matching resumes - the search only matches title text, not dates. No separate date-range or exact-date filter control is present anywhere on the dashboard.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid experience date ranges are not rejected
    - Actual: Setting an experience End Date (2017-01) earlier than its Start Date (2018-06) was accepted with no error/validation message; preview rendered 'Jun 2018 - Jan 2017'. Setting End Date to a future date (2030-01) was likewise accepted with no rejection or feedback.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data does not persist across a full application reload
    - Actual: Before reload, the dashboard had 5 resumes including two created during the session ('Startup Application Resume', created via New Resume then renamed, and 'Copy Person Resume', a duplicate with an edited Full Name). After reloading the application (browser navigation to http://localhost:6005/), the dashboard reverted to only the original 3 seed resumes (Product Designer Resume, Marketing Manager Resume, Software Engineer Resume) - all newly created, duplicated, and renamed resumes were lost, and any edits made to those resumes during the session did not survive the reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when renaming to blank title
    - Actual: Editing the resume title to a whitespace-only value and pressing Enter silently reverted the title back to 'Untitled Resume' with no visible error, toast, or inline validation message indicating why the change was rejected.


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