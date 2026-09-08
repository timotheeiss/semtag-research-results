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
    - Actual: The dashboard's only filter controls are a single text input (placeholder "Search resumes...") and a sort combobox offering Newest First / Oldest First / Title A-Z / Title Z-A. There is no date input, no date-range picker, and no clear-date control anywhere on the dashboard (main contains exactly one input, of type=text), so narrowing resumes by an exact creation date or a date range is impossible.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No validation on experience or education date ranges
    - Actual: Experience: start Jan 2020 with end May 2019 was accepted silently and rendered "Jan 2020 - May 2019"; end Sep 2030 (future; today 2026-08-27) also accepted, rendered "Jan 2020 - Sep 2030". Education: start Sep 2013 with end Jan 2012 accepted, rendered "Sep 2013 - Jan 2012". No inline error, toast, or field highlight appeared on change or blur; month inputs have no max attribute.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes lost on reload
    - Actual: Before reload the dashboard listed Software Engineer Resume, Product Designer Resume, Backend Engineer Resume (created+renamed+edited), Startup Backend Resume (duplicated+renamed+edited) and Data Analyst Application (created), with Marketing Manager Resume deleted. After reloading http://localhost:6005/ the list reverted to only the 3 seeded resumes (Product Designer, Marketing Manager, Software Engineer): the 3 created/duplicated resumes disappeared, the renames and content edits were lost, and the deleted Marketing Manager Resume reappeared. Both localStorage and sessionStorage contain zero keys, so state is held only in memory.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only rename accepted on dashboard; no validation feedback anywhere
    - Actual: Dashboard card menu → Rename: replacing "Backend Engineer Resume (Copy)" with "   " and clicking Save closed the dialog and set the card title to the whitespace string (dashboard heading now renders "   " with no visible title). No error message or toast was shown. In the editor header rename, empty/whitespace input is silently reverted to the previous title but still with no validation message (empty notifications region, no inline error).


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
    - Issue: Customized summary heading not rendered in Minimal template
    - Actual: Section headings were renamed to "About Me", "Career History", "Core Competencies", "Academics". Modern preview shows all four; Classic shows ABOUT ME / CAREER HISTORY / CORE COMPETENCIES / ACADEMICS. Minimal renders CAREER HISTORY, CORE COMPETENCIES and ACADEMICS but prints the summary paragraph with no heading at all — "About Me" is absent (the same happened with the default "Professional Summary" label), even though the Summary section is visible and has content.