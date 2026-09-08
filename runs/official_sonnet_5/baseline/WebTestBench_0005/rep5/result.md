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
    - Issue: Missing date-filter feature
    - Actual: Inspected the dashboard's full interactive control set (buttons/inputs) via DOM query: only "New Resume" button, "Search resumes..." text input, and a sort combobox (Newest/Oldest First, Title A-Z/Z-A) exist. There is no date-range or exact-date filter control. Typing a date string (e.g. "2024-02-10") into the search box does not match resumes by date — it is treated as a title substring search and returns "No resumes found" even though a resume with that exact updated date exists. The app provides no way to narrow resumes by creation date or date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid experience date range (end before start) is silently accepted with no validation
    - Actual: Set an experience entry's Start Date to 2021-06 and End Date to 2020-01 (end before start). No error message, inline warning, or toast appeared (verified via DOM query for toast/alert/error elements — none found). The Live Preview rendered the invalid range as-is: "Jun 2021 - Jan 2020" without any rejection or feedback.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Data not persisted across reload
    - Actual: Before reload, the dashboard listed 6 resumes: "Data Analyst Resume Variant" (newly created), "Software Engineer Application - TechCo (Copy)" (duplicated), "Software Engineer Application - TechCo" (edited with new experience entries/skills), "Product Designer Resume", "Marketing Manager Resume", "Software Engineer Resume". After navigating/reloading http://localhost:6005/, the dashboard shows only 3 resumes: "Product Designer Resume", "Marketing Manager Resume", "Software Engineer Resume" — the newly created resume, its duplicate, and the previously-edited "Software Engineer Application - TechCo" resume (with all its Experience/Contact edits) are gone entirely. Created/edited/duplicated resumes do not persist after a page reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when renaming to whitespace-only title
    - Actual: Editing the resume title to "   " (whitespace only) and pressing Enter silently reverted to the original title "Untitled Resume" with no toast, inline error, or any visible message. DOM inspection found no toast/alert elements and the title heading showed no error styling. The title is correctly preserved, but the user receives no clear validation feedback as required.


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