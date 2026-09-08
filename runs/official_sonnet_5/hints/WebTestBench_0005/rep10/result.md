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
    - Issue: No date filtering feature exists
    - Actual: Inspected the dashboard toolbar and full page DOM for date-filter controls. The only inputs present are the title search textbox and the sort combobox (Newest/Oldest/Title A-Z/Title Z-A). There is no exact-date filter, date-range filter, or any date-picker input anywhere on the dashboard, so resumes cannot be narrowed by creation date at all.


## Constraint
- [ ] CS-9: For each past work or education entry, the end month must not precede the start month and neither date may be in the future; invalid periods are rejected with clear feedback.
  - Bug Report:
    - Issue: No validation for invalid date periods
    - Actual: Setting an education entry's end-date (2015-01) earlier than its start-date (2015-09) was accepted silently with no error message. Setting start-date to a future month (2027-01, current date is 2026-08) was also accepted with no warning/rejection. No validation feedback text appears anywhere on the page after either invalid entry.

- [X] CS-10: Experience and education dates use a consistent month/year format, and marking a work position as current clears and disables its end date.

- [ ] CS-11: Created, edited, duplicated, renamed, and deleted resume versions remain unchanged after the application is reloaded.
  - Bug Report:
    - Issue: No persistence across reload - all session changes are lost
    - Actual: Before reload, dashboard had 4 resumes: 'Software Engineer - StartupXYZ Application' (duplicated, renamed, edited), 'Software Engineer - TechCorp Application' (created and edited), 'Product Designer Resume', 'Marketing Manager Resume' - with 'Software Engineer Resume' having been deleted. After navigating to http://localhost:6005/ (reload), the dashboard reverted to the original 3 seed resumes ('Product Designer Resume', 'Marketing Manager Resume', 'Software Engineer Resume'); the created/duplicated/renamed/edited resumes were gone and the deleted resume reappeared. None of the created, edited, duplicated, renamed, or deleted changes persisted across a reload.

- [ ] CS-21: A resume cannot be renamed to an empty or whitespace-only title; the current title remains and the user receives clear validation feedback.
  - Bug Report:
    - Issue: Whitespace-only title accepted, no validation feedback
    - Actual: Opened Rename dialog for 'Software Engineer - TechCorp Application (Copy)', entered a whitespace-only value '   ', and clicked Save. The app accepted it with no error/validation message; the dialog closed and the resume card/title is now blank (empty string) instead of retaining its previous non-empty title.


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
    - Issue: Summary section heading not rendered in Minimal style
    - Actual: Customized section headings ('About Me' for Summary, 'Academic Background' for Education, 'Career History' for Experience, 'Technical Skills' for Skills) were entered in the Sections tab. In the Modern and Classic styles all four customized headings render correctly. In the Minimal style, Education/Experience/Skills headings render correctly, but the Summary section (visible, with content 'Experienced software engineer...') is rendered with no heading at all - the custom text 'About Me' does not appear anywhere for that section in Minimal.