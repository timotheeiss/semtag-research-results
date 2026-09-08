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
    - Issue: Date filtering feature absent
    - Actual: Dashboard offers only a title search input (dashboard.search) and a sort select (dashboard.sort: newest/oldest/title-asc/title-desc). No exact-date or date-range filter control exists, even including hidden elements; full accessibility snapshot confirms no date inputs.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation
    - Actual: With start 2020-01, setting end date to 2019-01 (before start) produced no error message or feedback anywhere in the DOM; setting end date to 2030-05 (future; today is Aug 2026) was also accepted and rendered in the preview as "Jan 2020 - May 2030". Month inputs have no min/max constraints.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload
    - Actual: Before reload the dashboard held 5 resumes: "Startup Variant Resume" (created), "Sales Variant Resume" (duplicated + renamed), "QA Master Resume" (created + edited + renamed), Product Designer, Marketing Manager — with "Software Engineer Resume" deleted. After navigating to http://localhost:7005/ again, the list reverted to the 3 seed resumes (Product Designer, Marketing Manager, Software Engineer): all created/edited/duplicated/renamed resumes were lost and the deleted resume reappeared. localStorage is empty, so nothing is persisted.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only title accepted via dashboard rename dialog; no validation feedback anywhere
    - Actual: Dashboard rename dialog: entering "   " and clicking Save closed the dialog and set the resume title to blank — dashboard card for tlqorl8koa now shows an empty title (semantic name: ""), with no error message or toast. In the editor header, an empty/whitespace title silently reverted to the previous title but likewise displayed no validation feedback.


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
    - Issue: Customized summary heading not rendered in Minimal style
    - Actual: Section headings were customized to "About Me", "Employment History", "Academics", "Core Skills". Modern and Classic previews show all four (Classic uppercased). In the Minimal style the summary section is visible with its text but no preview.summary.heading element exists at all — the custom heading "About Me" is missing (only EMPLOYMENT HISTORY, CORE SKILLS, ACADEMICS shown).