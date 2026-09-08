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
    - Issue: Date filter not implemented
    - Actual: No date filter UI controls found on dashboard. Dashboard shows: (1) Search input (text-based filtering), (2) Sort dropdown (Newest/Oldest/Title A-Z/Z-A), (3) Resume cards showing "Updated [date]" text. No input fields or controls for filtering by exact date or date range exist. Semantic hints show only: dashboard.create, dashboard.search, dashboard.sort, dashboard.resumes - no date-related controls.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date validation - invalid dates accepted without error
    - Actual: Tested with experience entry: (1) Changed end date from 2026-08 to 2023-12 (before start date 2024-01) - accepted silently, preview shows "Jan 2024 Dec 2023". (2) Changed end date to 2027-12 (future date, today is Aug 25, 2026) - accepted silently, preview shows "Jan 2024 Dec 2027". No error messages, validation dialogs, or feedback provided for either invalid scenario. Invalid dates persist in the resume.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume changes not persisted across page reload
    - Actual: Created and edited "Untitled Resume" (with contact info, summary, experience, education, skills, changed template to Minimal) and duplicated/renamed to "Financial Analyst Resume" during session. Before reload, dashboard showed 5 resumes (3 original + 2 new). After reload, only 3 original resumes remain (Product Designer, Marketing Manager, Software Engineer). Untitled Resume and Financial Analyst Resume completely disappeared. No persistence of session changes.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Empty title validation not implemented
    - Actual: Opened rename dialog for "Software Engineer Resume", cleared the title field to empty string, clicked Save. The empty title was accepted and saved. Resume now displays with empty name ("") on dashboard. No validation error or feedback provided. No mechanism to reject empty/whitespace-only titles. Current title was overwritten with empty string instead of being preserved.


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