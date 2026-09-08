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
    - Issue: Date filtering feature is entirely absent from the dashboard
    - Actual: The dashboard offers only a title search box and a sort combobox (Newest/Oldest First, Title A-Z/Z-A). There are no date filter controls: zero input[type=date|month|datetime-local] elements, the only input on the page is the "Search resumes..." textbox, and no date-picker/range buttons exist. Searching a date string ("Feb 10, 2024") that matches a card's displayed date returns "No resumes found / Try a different search term", so there is no way to narrow by an exact creation date or a creation-date range, and therefore no date criterion to clear.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation for work/education entries
    - Actual: Set Position 1 start=2020-03 with end=2019-01 (end before start) and later start=2030-05 (future). No error message, aria-invalid, or any feedback appeared; values were accepted and the preview rendered "May 2030 - Jan 2019". Month inputs have no min/max and checkValidity() returns true.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes are lost on reload; app resets to seeded demo data
    - Actual: Before reload the dashboard held 5 resumes: created "QA Engineer Resume", duplicated+renamed "Senior QA Lead Resume", and the 3 seeded ones, with "Startup Application Resume" deleted. After reloading http://localhost:6005/ only the 3 original seeded resumes remain (Product Designer, Marketing Manager, Software Engineer). The created resume, the duplicate, the rename and all content edits are gone, and the deleted resume's removal was also not persisted (state simply reset). localStorage and sessionStorage are both empty (no keys), confirming state is in-memory only.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only rename is accepted on the dashboard (empty title saved); no validation feedback on either rename path
    - Actual: Dashboard path: card menu → Rename → set title to "   " → Save. The dialog closed and the resume was actually renamed to an empty title — the card heading now reads "" (JSON.stringify of h3 innerText = ""). No error, no toast (Notifications region empty), and the previous title "QA Engineer Resume (Copy)" was lost. Editor path: renaming to "" / "   " silently reverts to the prior title with no error message, toast, or aria-invalid feedback.


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
    - Issue: Customized Summary section heading is not rendered in the Minimal template
    - Actual: Sections tab lets the user rename headings. Renamed Summary→"About Me", Experience→"Career History", Education→"Academics", Skills→"Core Skills". Modern preview shows "About Me / Career History / Academics / Core Skills" and Classic shows "ABOUT ME / CAREER HISTORY / ACADEMICS / CORE SKILLS", but the Minimal preview renders only "CAREER HISTORY / ACADEMICS / CORE SKILLS" — the summary body text appears with no heading. DOM check of the Minimal preview root: innerHTML does not contain "About Me"/"ABOUT ME"; headings list is [H1:Alice Tester, H2:CAREER HISTORY, H3..., H2:ACADEMICS, H3..., H2:CORE SKILLS].