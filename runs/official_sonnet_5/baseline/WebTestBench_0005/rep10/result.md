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
    - Issue: Date filter feature missing
    - Actual: Inspected the dashboard's full control set (search textbox, sort combobox, and all buttons) via DOM query — no date filter control (exact date picker or date-range filter) exists anywhere on the page. Only a text search box and a sort-order dropdown (Newest/Oldest/Title A-Z/Title Z-A) are present. Users cannot filter resumes by creation date at all.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Invalid date range not rejected
    - Actual: Set experience Start Date=2018-01 and End Date=2017-01 (end before start); no validation error or feedback appeared; preview rendered the invalid range as "Jan 2018 - Jan 2017" without warning.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload
    - Actual: After creating 'QA Test Resume', duplicating it, renaming the copy to 'Marketing Application Resume', and editing both resumes' content, reloading the app (navigate to same URL) wiped all changes: the dashboard reverted to only the original 3 seed resumes (Product Designer Resume, Marketing Manager Resume, Software Engineer Resume) with their default data. Inspecting localStorage/sessionStorage via evaluate showed both were completely empty ({}), confirming the app holds resume data only in in-memory React state with no persistence layer (no localStorage, sessionStorage, or backend save observed).

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: No validation feedback on empty/whitespace rename
    - Actual: Cleared the resume title to empty string and pressed Enter, then also tried whitespace-only "   " and pressed Enter. In both cases the title reverted to its previous value (title remains unchanged, which is correct), but no error message, toast, or validation feedback of any kind appeared in the notifications region or elsewhere to inform the user the rename was rejected.


## Interaction
- [X] IX-12: Changes to resume fields and the selected template appear in the visible preview immediately, without requiring a manual save or refresh.

- [X] IX-13: Changing a section's order or visibility updates the preview immediately and preserves the new layout while navigating within the current session.

- [X] IX-14: Resume edits are retained automatically when the user moves between editor areas, returns to the dashboard, and reopens the resume during the current session; the dashboard reflects that the resume was updated.


## Content
- [ ] CT-15: For the selected style, the preview renders every visible section that has content and keeps the complete resume readable through scrolling when it exceeds the available preview area.
  - Bug Report:
    - Issue: Preview area has no bounded/independently scrollable viewport
    - Actual: All visible sections with content do render correctly. However, after adding enough experience entries to exceed the visible viewport, the "Live Preview" panel container (and its wrapping flex row containing both editor and preview) grew unboundedly (from ~757px to ~1953px height) instead of staying fixed-size with its own internal scrollbar; scrollHeight equaled clientHeight on the ScrollArea container at every size, confirming no internal scroll region caps the preview. The whole page had to be scrolled (body scrollHeight 2018 vs window height 720) to see the rest of the resume, pushing the editor form far off-screen rather than keeping preview independently scrollable within its available area.

- [X] CT-16: Contact details, summary, work experience, education, and skills entered for a resume remain present when switching editor areas, returning to the dashboard, and reopening that resume during the current session.

- [X] CT-17: After a resume is duplicated, editing any content in the copy does not alter the corresponding content in the original, and editing the original does not alter the copy.

- [X] CT-18: After a resume is renamed, the same non-empty title is shown in its editor and on its dashboard card.

- [X] CT-20: Every section heading that the editor allows the user to customize is shown with that customized text in each resume style where the section is visible.