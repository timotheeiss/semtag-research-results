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
    - Actual: Inspected the dashboard's full DOM for any filter-by-date control (date input, date-range picker, calendar icon, or "filter" text) - none exists. The only controls present are the title search textbox and the sort-order combobox (Newest First/Oldest First/Title A-Z/Title Z-A). There is no way to filter saved resumes by an exact creation date or date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid experience date ranges are not rejected
    - Actual: Setting an experience End Date (2019-01) earlier than its Start Date (2020-01) was accepted with no error, and preview rendered "Jan 2020 - Jan 2019". Setting the End Date to a future date (2027-01) was also accepted with no validation feedback and rendered "Jan 2020 - Jan 2027". No error message, highlighting, or rejection occurred in either case.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No changes persist across a page reload
    - Actual: After reloading the application (full page navigate to http://localhost:6005/), all session changes were lost: the newly created "QA Test Resume A" and the duplicated "Marketing Manager Resume V2" both disappeared from the dashboard entirely (only the original 3 seed resumes remained), the renamed original resumes reverted to defaults, and "Software Engineer Resume" reverted to its original state - its Updated date reverted to "Jan 20, 2024" (from "Aug 25, 2026"), its phone number reverted to "(555) 123-4567" (losing the edited "(555) 999-0000"), and the added "GraphQL" skill was gone. No created, edited, duplicated, or renamed resumes persisted after reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown for whitespace-only rename
    - Actual: Attempting to rename the resume to a whitespace-only title ("   ") was correctly rejected: the title reverted to the previous valid value "QA Test Resume A" and the button/heading displayed it unchanged. However, no error message, toast, or other visible feedback was shown - the Notifications region's list element was confirmed empty (`<ol>...</ol>` with no child nodes) immediately after submission, and no error/invalid/required text appeared anywhere in the page body. The rejection is silent with no user-facing explanation.


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
    - Issue: Customized Summary section heading not rendered in Minimal template
    - Actual: Renamed the Summary section heading (via Sections tab) to "Career Summary". In Modern and Classic templates, the preview correctly shows an h2 "Career Summary" heading above the summary paragraph. In the Minimal template, no heading is rendered for the Summary section at all (only the paragraph text) - "Career Summary" does not appear anywhere in the DOM. Renamed Work Experience → "Employment History", Education → "Academic Background", and Skills → "Core Competencies" all rendered correctly in all three templates.