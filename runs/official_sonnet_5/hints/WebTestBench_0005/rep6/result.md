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
    - Issue: Date filter feature not implemented
    - Actual: Dashboard only exposes a title search box and a sort dropdown (Newest/Oldest/Title A-Z/Title Z-A). No control exists to filter resumes by an exact creation date or a creation-date range (checked semantic_snapshot with includeHidden and full browser_snapshot - no date filter UI present).


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date validation for experience periods
    - Actual: Setting End Date (2019-01) before Start Date (2020-01) was accepted silently - preview showed 'Jan 2020 – Jan 2019' with no error message and aria-invalid was null. Setting Start Date to a future month (2027-01, current date is Aug 2026) was also accepted without any rejection or feedback (preview showed 'Jan 2027 – Jun 2022').

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload
    - Actual: After reloading the app, all session changes were lost: the newly created resume ('Software Engineer Resume v2'), its duplicate/rename ('Software Engineer - Startup Application'), and the deletion of 'Marketing Manager Resume' were all reverted. Dashboard showed only the original 3 seed resumes (Product Designer Resume, Marketing Manager Resume, Software Engineer Resume) with their original data, as if no session changes had occurred.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Missing validation feedback for empty/whitespace title rename
    - Actual: Entering a whitespace-only title and submitting correctly reverted the title to its previous non-empty value ('Software Engineer Resume v2'), but no toast, inline error, or any other validation message was shown to the user (checked notifications region and DOM for role=status/alert/toast elements - none present).


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