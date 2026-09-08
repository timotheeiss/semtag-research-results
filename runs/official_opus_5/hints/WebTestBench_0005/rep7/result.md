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
    - Issue: Date filtering feature is entirely absent from the dashboard
    - Actual: The dashboard toolbar contains only a text search box (dashboard.search) and a sort dropdown (dashboard.sort with newest/oldest/title-asc/title-desc). A DOM scan found 0 input[type=date] elements and no date-range or exact-date controls anywhere on the page, so saved resumes cannot be narrowed by creation date.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No date-period validation on experience/education entries
    - Actual: Set Acme Corp experience start=2020-01 with end=2019-05 (end before start): no error, no styling change, item text unchanged ("Position 1 / Company / Position / Start Date / End Date..."), no message anywhere in body. Then set start=2030-01 (future, today is 2026-08-28): also accepted. The preview rendered the invalid period verbatim as "Jan 2030 - May 2019". Date inputs are type=month with no max attribute.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence — all resume changes are lost on reload and deletions are resurrected
    - Actual: Before reload the dashboard held 5 resumes: created "QA Lead Application" (edited, renamed from Untitled), duplicated+renamed "Automation Architect Application", created "Zeta Startup Pitch Resume", plus seeds resume-3 and resume-1 ("Marketing Manager Resume" had been deleted via the confirm dialog). After navigating to http://localhost:7005/ again, the dashboard reverted to the 3 original seed resumes: Product Designer Resume, Marketing Manager Resume (the deleted one is back), Software Engineer Resume. All 3 session resumes and every content edit were lost. localStorage and sessionStorage are both empty, so state is in-memory only.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Dashboard rename dialog accepts a whitespace-only title (resume left with a blank name); editor rename silently reverts with no feedback
    - Actual: Path A (dashboard menu → Rename dialog): entered "   " and clicked Save. The dialog closed, the rename was APPLIED and the card title became an empty string (semantic_observe of dashboard.resumes.item.f50os8qr4h returns name:""). No validation message shown. Path B (editor header title): entering "   " or "" and pressing Enter reverts to the previous title but shows no message at all — sonner toaster empty, no [role=alert]/[aria-live] text, no inline error.


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
    - Issue: Customized section headings not rendered in every style: contact heading never rendered; summary heading missing in Minimal
    - Actual: Renamed all 5 editable section titles to: Contact="How To Reach Me", Summary="About Me", Experience="Career History", Education="Academic Background", Skills="Core Competencies". Modern preview shows About Me / Academic Background / Career History / Core Competencies but no "How To Reach Me". Classic shows ABOUT ME / ACADEMIC BACKGROUND / CAREER HISTORY / CORE COMPETENCIES but no "How To Reach Me". Minimal shows ACADEMIC BACKGROUND / CAREER HISTORY / CORE COMPETENCIES only — the summary paragraph is rendered with NO heading at all, so the customized "About Me" is dropped, and "How To Reach Me" is again absent.