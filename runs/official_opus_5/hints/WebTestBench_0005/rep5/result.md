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
    - Actual: The dashboard exposes only three controls: "New Resume", a text search box ("Search resumes..."), and a sort dropdown (Newest/Oldest/Title A-Z/Title Z-A). An exhaustive scan of all input/select/button elements found zero date or date-range inputs (0 elements of type=date or type=month) and no exact-date or from/to filter UI, therefore no date criterion to clear either. The text search does not act as a date filter: entering "Feb 25, 2024" returned 0 results ("No resumes found - Try a different search term") even though a resume carries that date.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No validation of invalid date periods
    - Actual: In an experience entry, setting End Date 2019-05 with Start Date 2020-01 (end before start) was accepted with no error message or toast. Setting Start Date to a future month 2030-03 (today 2026-08-27) was also accepted silently. The month inputs have no min/max attributes and no inline feedback appears in the entry panel.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all created, edited, duplicated, renamed and deleted resume versions are lost on reload
    - Actual: Before reload the dashboard held 5 resumes: "Senior SDET Application" (duplicated + renamed), "QA Automation Resume" (created + heavily edited + renamed), and the 3 seeded ones, with "Zebra Consulting Variation" successfully deleted. localStorage and sessionStorage were both completely empty (0 keys). After reloading http://localhost:7005/ the dashboard reset to only the 3 original seeded resumes (Product Designer, Marketing Manager, Software Engineer). The created/edited/duplicated/renamed resumes vanished and the deleted one was not persisted as deleted — state is in-memory only.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts a whitespace-only title, leaving the resume with an empty title; no validation feedback anywhere
    - Actual: Dashboard card menu > Rename on "QA Automation Resume (Copy)": entered "   " (three spaces) and clicked Save. The dialog closed and the rename was ACCEPTED — the card title is now empty (dashboard.resumes.item.5b8lzf8w56l has name "" and renders no heading text). No validation message or toast appeared. Separately, the in-editor title rename does reject empty/whitespace input and keeps the previous title, but it also gives no validation feedback of any kind.


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
    - Issue: Customized Summary heading not rendered in the Minimal style
    - Actual: Section headings were customized to "About Me" (summary), "Career History", "Academics", "Core Strengths". Modern preview shows all four; Classic shows all four uppercased (ABOUT ME / ACADEMICS / CAREER HISTORY / CORE STRENGTHS). Minimal renders ACADEMICS, CAREER HISTORY and CORE STRENGTHS but the summary section (visible and containing text) is rendered with no heading at all — preview text goes straight from the contact block to the summary body, and preview headings are only H1 name, ACADEMICS, CAREER HISTORY, CORE STRENGTHS. "About Me"/"ABOUT ME" is absent from the Minimal preview.