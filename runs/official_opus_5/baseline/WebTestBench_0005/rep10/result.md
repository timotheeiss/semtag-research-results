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
    - Issue: No date filtering capability on the dashboard
    - Actual: Dashboard controls consist only of a "Search resumes..." text input and a sort dropdown (Newest/Oldest First, Title A-Z/Z-A). There is no exact-date or date-range filter control (no date inputs exist in main). Entering a date "Feb 10, 2024" into the search box yields "No resumes found", so dates cannot be used to narrow resumes at all.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation on experience entries
    - Actual: With start date Jan 2020, setting end date to Jan 2019 (before start) was accepted silently and preview showed "Jan 2020 - Jan 2019". Setting a future end date 2030-05 was also accepted ("Jan 2020 - May 2030"); month inputs have no max attribute and no error message or feedback appeared anywhere on the page.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload — all resume changes are lost
    - Actual: Before reload the dashboard held 5 resumes: created "QA Engineer Resume" and "Data Analyst Application", duplicate renamed to "Product QA Resume", and deleted "Software Engineer Resume". localStorage was empty. After reloading http://localhost:6005/, the dashboard shows only the 3 original seeded resumes (Product Designer, Marketing Manager, Software Engineer) — created/edited/duplicated/renamed resumes vanished and the deleted resume reappeared.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only title accepted by dashboard rename dialog; no validation feedback anywhere
    - Actual: Dashboard card menu → Rename → entered "   " → Save: the dialog closed and the resume card title became blank (empty heading), i.e. the whitespace title was saved with no validation error. In the editor header the same input silently reverts to the old title with no error message/toast (MutationObserver over 1.5s captured no feedback).


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
    - Issue: Custom summary section heading not rendered in Minimal template
    - Actual: Section headings were renamed to "About Me", "Career History", "Core Skills", "Academics". Modern and Classic previews show all four custom headings. In the Minimal template the preview renders only CAREER HISTORY, CORE SKILLS and ACADEMICS — the summary section (still visible/enabled) shows its text with no "About Me" heading at all.