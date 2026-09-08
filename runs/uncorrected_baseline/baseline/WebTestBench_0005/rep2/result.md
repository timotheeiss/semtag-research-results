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
    - Issue: No validation for invalid/conflicting time ranges in experience entries.
    - Actual: Setting an experience End Date (2019-01) earlier than its Start Date (2020-01) was accepted with no error, showing "Jan 2020 - Jan 2019" in preview. Also adding a second overlapping experience entry (Jan 2021–Jan 2022, fully inside the first entry's Jan 2020–Jun 2023 range) was accepted with no conflict warning.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [X] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No user feedback/confirmation is given after a resume is saved.
    - Actual: Data is auto-saved silently (verified persisted after navigating back to the home list), but no toast, banner, or any visible confirmation message appeared in the "Notifications" region after edits or after navigating away, despite multiple edits and checks of the notification region.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.