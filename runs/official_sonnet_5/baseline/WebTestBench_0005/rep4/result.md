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
    - Issue: No date-based filtering feature exists on the dashboard
    - Actual: Inspected dashboard for a date filter control (exact date or date range): only a title search textbox and a sort-order combobox (Newest First/Oldest First/Title A-Z/Title Z-A) exist. A DOM query for date/filter/calendar-related inputs or buttons returned no elements. Typing the exact 'Updated' date string of an existing resume ('Feb 10, 2024', matching Marketing Manager Resume's displayed date) into the search box returned 'No resumes found' - confirming the search only matches titles, not dates, and there is no separate mechanism to filter resumes by creation/updated date or date range as required by the checklist.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid experience date range is silently accepted with no validation feedback
    - Actual: Set Start Date=2022-07 and End Date=2020-01 (end before start) on an experience entry; the field accepted the value, no error/warning message appeared anywhere on the page, and the live preview rendered the nonsensical range 'Jul 2022 - Jan 2020' without any indication of invalidity.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data is not persisted across page reloads - all created/edited/duplicated resumes are lost
    - Actual: Prior to reload, the dashboard had 5 resumes: the 3 original seed resumes plus a newly created 'QA Engineer Application' (extensively edited: renamed, contact/summary/experience/education/skills filled, section reordered, heading renamed to 'Career History', location changed to 'Denver, CO') and its duplicate 'Marketing Role Application' (renamed and independently edited). After reloading the app via browser_navigate to the same URL, the dashboard reverted to showing only the original 3 seed resumes (Product Designer Resume, Marketing Manager Resume, Software Engineer Resume) - both newly created resumes and all their edits were completely lost. A DOM check confirmed localStorage and sessionStorage are both empty, indicating the app holds resume data only in transient in-memory state with no persistence layer, so any created, edited, duplicated, or renamed resume does not survive a page reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when attempting to rename resume to whitespace-only title
    - Actual: Clicked the resume title to edit, entered '   ' (whitespace only) and pressed Enter. The title correctly reverted to the previous 'Untitled Resume' (title was not changed to blank), but no toast, inline error, or any other validation feedback message was displayed to inform the user why the change was rejected.


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