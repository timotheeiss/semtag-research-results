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
    - Issue: Creation-date filtering feature not implemented
    - Actual: The dashboard offers no way to narrow resumes by creation date. Full snapshot (including hidden elements) exposes only dashboard.create, dashboard.search (text) and dashboard.sort (newest/oldest/title-asc/title-desc). A raw DOM scan found exactly one input on the page — the text search (type="text") — no input[type=date], no date-range picker, no calendar, and no button/combobox matching /date|filter|from|to|range|created|between/ (only "New Resume" and "Newest First" buttons exist). Using the search box as a fallback with the exact displayed date "Feb 25, 2024" returned 0 results and the empty-state message. Therefore neither an exact creation date nor a date range can be applied, and no date criterion exists to clear.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: Missing date-period validation: reversed and future periods accepted with no feedback
    - Actual: On experience entry "Analytical Engines Ltd": set start=2020-01, end=2019-05 (end before start) — accepted, no error text in the entry (innerText shows only field labels), aria-invalid=null, validationMessage="". Then set start=2030-01, end=2031-05 (both future; today 2026-08-27) — also accepted; inputs have no max attribute and the preview rendered "Jan 2030 - May 2031". No rejection or feedback in either case.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes are lost on reload; app resets to seeded demo data
    - Actual: Before reload the dashboard held 5 resumes: "Ada ML Engineer Resume", "Ada Data Scientist Resume" (created, edited, duplicated and renamed during the session) plus 3 seeded ones, with "Ada Product Manager Resume" deleted. After reloading http://localhost:7005/ the dashboard showed only the 3 original seeded resumes (resume-1/2/3) — every created, edited, duplicated and renamed version was gone. Deletion also does not persist: deleted seeded "Product Designer Resume" (resume-3), reloaded, and it reappeared unchanged ("Updated Feb 25, 2024", Minimal). localStorage is empty (Object.keys(localStorage) = []), confirming state is in-memory only.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts a whitespace-only title, blanking the resume name; editor rename rejects it but gives no feedback
    - Actual: Dashboard path (primary defect): card menu > Rename > entered "   " > Save. The dialog closed and the rename was COMMITTED — the resume card now renders an empty title (dashboard.resumes item sg8053e15n has name:"" where it was "Ada Data Scientist Resume (Copy)"). No validation blocked it and no error was shown. Editor path: entering "   " or "" in editor.title.input reverted to the prior title (title preserved) but displayed no validation feedback at all (no error text, no [role=alert]/toast/aria-live).


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
    - Issue: Customized Summary section heading is not rendered in the Minimal style
    - Actual: Section headings were customized to "About Me" (summary), "Professional Experience", "Academic Background", "Core Competencies". Modern and Classic render all four. Minimal renders "PROFESSIONAL EXPERIENCE", "ACADEMIC BACKGROUND" and "CORE COMPETENCIES" but shows NO heading for the summary section — the summary text appears directly after the contact block ("...ADA.DEV | EXPERIENCED MATHEMATICIAN AND ENGINEER..."), so the custom "About Me" heading is missing even though the summary section is visible and has content.