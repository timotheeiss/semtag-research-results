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
    - Issue: No date filter feature exists
    - Actual: The dashboard only offers a title search box and a sort combobox (Newest/Oldest/Title A-Z/Z-A). There is no exact-date or date-range filter control (only one input on the page: "Search resumes..."). Typing a date ("Feb 25, 2024") into the search box returns "No resumes found", so date narrowing is impossible.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation on experience entries
    - Actual: Entered start 2023-05 with end 2023-01 (end before start): accepted with no error message, and preview rendered "May 2023 - Jan 2023". Entered a future start date 2030-05: also accepted, no validation feedback, and month inputs have no max attribute.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload — all session changes lost
    - Actual: Before reload the dashboard had 5 resumes incl. created "QA Engineer Resume", duplicated+renamed "Senior QA Application", and deleted "Startup PM Application". After reloading http://localhost:6005/ the dashboard shows only the 3 original seed resumes (Product Designer, Marketing Manager, Software Engineer); created/edited/duplicated/renamed items are gone and the deleted one's removal is moot. localStorage and sessionStorage are both empty — state is in-memory only.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only rename accepted (dashboard) / silently reverted with no feedback (editor)
    - Actual: Dashboard card menu → Rename → entered "   " → Save: dialog closed and the resume card now shows an EMPTY title (h3 text is ""), no validation message. In the editor header, entering "  " + Enter silently reverts to the previous title with no toast/inline error/aria-live feedback.


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
    - Actual: Renamed section headings to About Me / Career History / Core Skills / Academics. Modern and Classic show all four. In Minimal the summary section is visible (its text renders) but its heading "About Me" is absent — preview goes straight from contact block to the summary paragraph, then "CAREER HISTORY"; h2 list contains CAREER HISTORY, CORE SKILLS, ACADEMICS only.