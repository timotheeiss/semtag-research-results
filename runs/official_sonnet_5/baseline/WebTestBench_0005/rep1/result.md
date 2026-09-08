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
    - Issue: No date-based filtering control exists on dashboard
    - Actual: Inspected all interactive controls on the dashboard (DOM query of input/button/select elements): only a text search box ("Search resumes...") and a sort-order combobox (Newest First/Oldest First/Title A-Z/Title Z-A) are present, plus per-card kebab menus. There is no date picker, date-range input, or any control allowing users to narrow saved resumes by exact creation date or a date range. Sorting by date only reorders results; it does not filter/narrow them. Therefore the feature to filter by creation date or date range is not implemented.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid experience date ranges are accepted without rejection or feedback
    - Actual: Setting End Date (2019-01) earlier than Start Date (2020-01) was accepted and rendered in preview as "Jan 2020 - Jan 2019" with no error message. Setting Start Date to a future month (2030-01, current date 2026-08) was also accepted with no validation feedback (rendered "Jan 2030 - Jan 2019"). No error/invalid styling or alert elements appeared in the DOM.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data does not persist across page reload
    - Actual: Prior to reload, dashboard had 5 resumes including two created/edited/duplicated/renamed during this session ("QA Engineer Resume - StartupXYZ App" and "QA Engineer Resume - TechCorp App"). After browser_navigate reload of http://localhost:6005/, dashboard shows only the original 3 seeded resumes (Product Designer Resume, Marketing Manager Resume, Software Engineer Resume) — the created/edited/duplicated/renamed resumes are gone. Inspection confirms localStorage and sessionStorage are both empty, indicating the app holds resume state only in-memory (React state) with no persistence layer, so all edits/creates/duplicates/renames/deletes are lost on reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when rejecting empty/whitespace-only rename
    - Actual: Entering a whitespace-only title ("   ") and submitting (Enter) correctly reverted the displayed title back to "Untitled Resume" (title was not actually changed to blank), but no toast, inline error message, or other visible feedback was shown to the user. The notifications region and all live regions remained empty of any message after the action.


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