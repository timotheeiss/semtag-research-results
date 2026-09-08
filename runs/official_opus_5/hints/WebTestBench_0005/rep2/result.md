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
    - Issue: No creation-date filter exists on the dashboard
    - Actual: Dashboard offers only a title search box and a sort select (Newest/Oldest/Title A-Z/Z-A). There are no date inputs anywhere on the page (querySelectorAll for input[type=date|month|datetime-local] returns none; the only input is "Search resumes..."), so an exact creation date or date range cannot be applied or cleared. Typing a date ("Feb 25, 2024") into the search box returns 0 results.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation for work or education entries
    - Actual: Experience: start=2030-05 (future) with end=2019-01 (before start) accepted silently, preview showed "May 2030 - Jan 2019". Education: start=2018-09 with end=2017-01 accepted silently, preview showed "Sep 2018 - Jan 2017". No error message, field highlight, or toast in either case.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: Resume data is not persisted across a page reload
    - Actual: Before reload the dashboard had 5 resumes (3 seeded + created "Data Scientist Resume" with full edits + duplicated/renamed "ML Engineer Application"; "Temp Delete Me" deleted). After reloading http://localhost:7005/ only the 3 seeded resumes (Product Designer, Marketing Manager, Software Engineer) remain — the created, edited, duplicated and renamed versions were all lost.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts a whitespace-only title, wiping the resume title; editor rename reverts silently with no feedback
    - Actual: Dashboard card menu > Rename: entering "   " and clicking Save closed the dialog and set the card title to an empty string (dashboard.resumes.item.n6c2auer8jq name = ""), with no validation message. Editor title rename with "   " or "" reverted to the previous title but showed no toast/inline error either.


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
    - Issue: Customized Summary section heading not rendered in the Minimal template
    - Actual: Section titles were customized to About Me / Career History / Academics / Core Skills. Modern and Classic previews show all four (preview.summary.heading = "About Me"/"ABOUT ME"). In Minimal, the summary section is visible and its text renders, but no preview.summary.heading element exists and the preview text contains no "About Me" — only CAREER HISTORY, ACADEMICS, CORE SKILLS headings are present.