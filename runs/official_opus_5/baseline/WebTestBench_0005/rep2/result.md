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
    - Issue: No creation-date filter feature exists
    - Actual: The dashboard toolbar contains only a "Search resumes..." text box and a sort combobox (Newest/Oldest First, Title A-Z/Z-A) — there is no exact-date input, date-range picker, or any date-criterion clear control. Enumerating all main inputs/selects/buttons returned only the search input, sort button and the 6 per-card menu buttons. Searching "2024" in the search box returns "No resumes found", confirming the search matches titles only and offers no date narrowing.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date validation for experience/education periods
    - Actual: Set Position 1 end date to 2019-05 with start 2020-01: accepted silently, preview shows "Jan 2020 - May 2019". Then set start date to 2027-05 (future; today is 2026-08-27): also accepted, preview shows "May 2027 - May 2019". No error message, toast, or field-level feedback appeared anywhere on the page.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes are lost on reload
    - Actual: Before reload the dashboard held 5 resumes: created "Data Analyst Application" (renamed) and "Backend Engineer Application", duplicated+renamed "Marketing Analyst Application", plus 2 seeded ones after deleting "Software Engineer Resume". localStorage was empty (Object.keys(localStorage) = []). After reloading http://localhost:6005/ the dashboard reverted to the 3 original seeded resumes (Product Designer, Marketing Manager, Software Engineer) — all created/edited/duplicated/renamed resumes vanished and the deleted "Software Engineer Resume" reappeared.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only title accepted via dashboard Rename dialog; no validation feedback anywhere
    - Actual: Dashboard card menu → Rename → set title to "   " → Save: the dialog closed and the card heading became "   " (whitespace-only title saved, original title lost), with no error message. Separately, the editor header inline rename silently reverts empty/whitespace input to the previous title but shows no toast or inline validation message (verified via a MutationObserver on document.body).


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
    - Issue: Customized summary section heading not rendered in Minimal template
    - Actual: Renamed section headings to "About Me" (Summary), "Career History", "Core Skills", "Academics". Modern and Classic previews show all four. In the Minimal template the preview headings are only ["Career History","Core Skills","Academics"] — the summary section is visible and its text renders, but its custom heading "About Me" is omitted entirely.