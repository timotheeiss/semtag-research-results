# Test Result

## Functionality
- [X] FT-1: Users can create blank resumes.

- [X] FT-2: Users can select preset templates to create professional resumes.

- [X] FT-3: Users can enter and edit personal information (contact information, work experience, and other core items).

- [X] FT-4: Supports customizable resume layout (chapter order and title modification).

- [X] FT-5: It provides a resume preview function, allowing users to see the edited result.

- [X] FT-6: Supports copying and renaming existing resume versions.

- [X] FT-7: It can save multiple resume versions to suit different job search scenarios.

- [X] FT-8: Supports filtering and searching saved resumes by title/creation date.


## Constraint
- [ ] CS-9: When adding past work or education experience, the time period must be a valid time frame and there should be no conflicts.
  - Bug Report:
    - Issue: No validation for conflicting/invalid time ranges in experience entries
    - Actual: Entered Start Date 2023-01 and End Date 2021-01 for a work experience entry (end before start). The app accepted it without any error/warning and displayed 'Jan 2023 - Jan 2021' in the live preview.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resumes are lost after a full page reload
    - Actual: Created 'QA Test Resume' and 'QA Test Resume - Job B' and confirmed they persisted while navigating within the SPA (back to dashboard and reopening). However, after navigating to http://localhost:7005/ (full page reload), both newly created resumes disappeared from the dashboard list; only the original 3 seed resumes remained.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No success feedback shown after saving
    - Actual: Editing fields and navigating back (which auto-saves, confirmed by data persisting in the dashboard) triggered no visible toast/notification/confirmation message. The 'Notifications' region remained an empty list in snapshots taken immediately after edits and after leaving the editor.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.