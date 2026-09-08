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
    - Issue: Date filtering feature is entirely absent
    - Actual: The dashboard exposes only a text search box (dashboard.search) and a sort dropdown (dashboard.sort with newest/oldest/title-asc/title-desc). Full accessibility snapshot of the dashboard confirms no date input, date-range inputs, or date-filter clear control anywhere on the page, so narrowing by exact creation date or a creation-date range is impossible.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation on experience/education entries
    - Actual: Set experience start=2020-01 and end=2019-05 (end precedes start): accepted silently, no error text, no aria-invalid/role=alert node anywhere in the experience region. Then set start=2030-03 and end=2031-09 (both in the future vs today 2026-08-28): also accepted. The month inputs carry no max attribute and checkValidity() returns true for both. No feedback text matching invalid/error/must/cannot/future exists anywhere in the document.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes are lost on reload and deletions are resurrected
    - Actual: Before reload the dashboard held 4 resumes: "ML Engineer Resume" (duplicated + renamed + edited), "Data Scientist Resume" (created new + fully authored + renamed + edited), "Marketing Manager Resume", "Software Engineer Resume"; "Product Designer Resume" had been deleted via the confirm dialog. After reloading http://localhost:7005/ the dashboard reset to the 3 original seeded resumes (Product Designer, Marketing Manager, Software Engineer). Both newly created/duplicated resumes and every edit and rename were lost, and the deleted "Product Designer Resume" reappeared. localStorage and sessionStorage are both completely empty ({}), confirming state is held only in memory.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts whitespace-only title, blanking the resume name; no validation feedback on either rename path
    - Actual: Two rename paths, both defective. (1) Dashboard card menu > Rename: entered "   " (whitespace-only) and clicked Save — the dialog closed and the rename was APPLIED. The card title is now empty (dashboard.resumes item name = "", card innerText = ""), so the resume is left with no visible title and no validation message or toast appeared. (2) Editor inline title: entering "   " or "" reverted to the previous title, which is correct, but produced no validation feedback (no toast, no [role=alert]/[role=status], nothing matching empty/required/cannot/invalid; polled ~720ms). Neither path gives the user clear validation feedback, and the dashboard path violates the requirement that the current title remains.


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
    - Issue: Customized summary heading not rendered in the Minimal style
    - Actual: Section headings were customized to: summary="About Me", experience="Career History", education="Academics", skills="Core Competencies". Modern preview shows all four ("About Me", "Academics", "Career History", "Core Competencies"); Classic shows all four ("ABOUT ME", "ACADEMICS", "CAREER HISTORY", "CORE COMPETENCIES"). Minimal renders "ACADEMICS", "CAREER HISTORY" and "CORE COMPETENCIES" but omits the summary heading entirely — the summary body text ("Pioneering analytical engine programmer...") appears with no heading above it even though the summary section is visible.