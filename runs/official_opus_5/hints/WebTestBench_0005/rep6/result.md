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
    - Issue: Creation-date filtering (exact date or date range) is not implemented
    - Actual: The dashboard exposes only 8 interactive controls in total: "New Resume", the text search input ("Search resumes..."), the sort dropdown (Newest/Oldest/Title A-Z/Title Z-A), and one overflow menu per resume card (Rename/Duplicate/Delete). A full DOM scan including hidden elements found no date input, no from/to range fields, no calendar/date-picker, and no clear-date control. There is no way to narrow resumes by an exact creation date or a creation-date range.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation; invalid work/education periods accepted without feedback
    - Actual: Acme Corp experience with start 2020-03: set end-date to 2019-01 (end precedes start) — accepted, no error text, no aria-invalid, no role=alert content; preview rendered "Mar 2020 - Jan 2019". Set end-date to 2029-12 (future; today 2026-08-27) — also accepted, input.validity.valid=true, no max attribute, preview rendered "Mar 2020 - Dec 2029". No rejection or feedback in either case.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload; all session changes are lost and deletions are reverted
    - Actual: Before reload the dashboard held 5 resumes: Data Analyst Resume (created), Automation Lead Resume (duplicated+renamed, edited), QA Engineer Resume (created+edited+renamed), Product Designer Resume, Marketing Manager Resume — with Software Engineer Resume deleted. After reloading http://localhost:7005/ the dashboard shows only the 3 original seed resumes (Product Designer, Marketing Manager, Software Engineer). All created, edited, duplicated and renamed versions vanished, and the deleted "Software Engineer Resume" reappeared.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts a whitespace-only title, leaving the resume with an empty title; no validation feedback anywhere
    - Actual: Dashboard card menu > Rename: entered "   " (3 spaces) and clicked Save. The dialog closed and the rename was APPLIED — the card's title is now empty (dashboard.resumes.item.yc5kslx0yrl name:"" where it was "QA Engineer Resume (Copy)"). No error, no toast, no inline message. Separately, in the editor the inline title edit rejects ""/"   " and reverts to the previous title, but likewise shows no validation feedback of any kind.


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
    - Actual: Section titles were customized to About Me / Career History / Academics / Core Competencies. Modern renders all four (summary=About Me); Classic renders all four (summary=ABOUT ME). Minimal renders only ACADEMICS, CAREER HISTORY, CORE COMPETENCIES — preview.summary.heading element does not exist and the preview text contains no "About Me", even though the Summary section is visible and its text is displayed.