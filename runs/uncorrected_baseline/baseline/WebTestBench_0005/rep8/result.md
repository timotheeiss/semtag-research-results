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
    - Issue: No validation of chronological/logical time-frame conflicts for experience/education dates
    - Actual: Set Work Experience Start Date=2020-01 and End Date=2019-01 (end before start, an invalid/conflicting range). The app accepted the input with no warning/error and displayed "Jan 2020 - Jan 2019" in the live preview unmodified.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resume data is not actually persisted — lost on page reload
    - Actual: Created "QA Test Resume - Backend Developer" and its duplicate "QA Test Resume - Frontend Application" (both fully filled with contact/experience/education/skills). After navigating within the app they persisted, but after a full page reload (http://localhost:7005/) both custom resumes disappeared entirely — dashboard reverted to only the original 3 seed resumes (Product Designer, Marketing Manager, Software Engineer).


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No explicit save confirmation/feedback for resume content edits, and saves are not durable
    - Actual: Editing resume fields (e.g., typing in Full Name) produced no toast/snackbar or any visible "Saved" indicator (Notifications region stayed empty). The only save feedback observed was the Rename dialog's own "Save" button closing the dialog. Combined with CS-11 (data lost on reload), there is no reliable feedback that content was successfully and durably saved.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.