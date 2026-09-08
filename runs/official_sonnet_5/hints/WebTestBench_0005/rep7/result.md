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
    - Issue: Date filtering feature is missing entirely
    - Actual: The dashboard only provides a title search box and a sort-by-date dropdown (Newest/Oldest/Title A-Z/Title Z-A). There is no UI control anywhere (including hidden elements) to filter resumes by an exact creation date or a date range. Users cannot narrow resumes by creation date at all.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No validation for invalid experience date ranges
    - Actual: Set start=2020-01, end=2019-01 (end before start): no error message appeared anywhere in the UI, and the preview happily rendered 'Jan 2020 - Jan 2019' with no rejection or feedback. Then changed end date to a future value 2030-01: the editor input accepted and displayed '2030-01', but the preview silently kept showing the stale 'Jan 2019' value instead of updating or showing an error - i.e. inconsistent state with no clear validation feedback in either case.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload - all session changes are lost
    - Actual: Created a new resume ('Backend Engineer Resume'), edited its content, duplicated it, renamed the duplicate to 'Backend Engineer Resume - Startup App', edited the duplicate, then deleted the original. After reloading the page (http://localhost:6005/), the dashboard reverted to only the original 3 seed resumes (Product Designer, Marketing Manager, Software Engineer) - the created/duplicated/renamed resume was gone entirely, and none of the session's create/edit/duplicate/rename/delete actions survived. localStorage and sessionStorage are both empty, confirming the app keeps all resume data only in in-memory state with no persistence.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback shown when rejecting empty/whitespace title
    - Actual: Editing the title field and submitting whitespace-only text ('   ') correctly reverted to the prior title 'Untitled Resume' (title itself preserved as required), but no error/toast/inline validation message was shown to the user anywhere on the page (Notifications region remained empty) to explain why the change was rejected.


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
    - Issue: Customized section heading not rendered in Minimal style
    - Actual: Renamed the Summary section heading to 'About Me' via Sections tab. It correctly appeared as 'About Me' in Modern and 'ABOUT ME' in Classic style, but the Minimal template renders the summary text as a plain paragraph with no heading element at all - the customized (or any) section title is completely missing from the Minimal preview for that visible section.