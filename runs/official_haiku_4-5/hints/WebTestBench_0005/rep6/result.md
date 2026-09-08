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
    - Issue: Date filtering UI not found
    - Actual: No date filter controls, date input fields, or filter buttons are present on the dashboard. The search input only filters by resume title (case-insensitive), not by creation date. Searching for a date string (e.g., "Feb 25, 2024") returns "No resumes found" rather than filtering by date.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date validation for work/education entries
    - Actual: Application accepts invalid date combinations without validation or error feedback: (1) End date before start date (Jan 2020 - Dec 2019) accepted; (2) Future end dates accepted (Dec 2027 when current date is Aug 2026); (3) Both dates in future accepted (Jun 2027 - Dec 2028). No error messages, warnings, or rejection of invalid periods.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data not persisted after page reload
    - Actual: Before reload: 5 resumes visible (Tech Lead Resume, Untitled Resume, + 3 originals). After reload: only 3 original resumes (Product Designer, Marketing Manager, Software Engineer). Created resume "Untitled Resume" and duplicated/renamed resume "Tech Lead Resume" were lost after page reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Empty title accepted without validation
    - Actual: Attempted to rename "Software Engineer Resume" to empty string - rename was accepted. Resume now shows empty name ("") in dashboard. No validation error or feedback was shown. Original title was NOT preserved as required.


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