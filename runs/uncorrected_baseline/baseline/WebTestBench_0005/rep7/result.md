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
    - Issue: No validation for invalid/conflicting time ranges
    - Actual: Entered Start Date 2022-01 and End Date 2021-01 (end before start) for a work experience entry. The app accepted this without any error or warning, and the live preview displayed 'Jan 2022 - Jan 2021' as a valid date range.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resumes are not persisted across page reloads
    - Actual: Created and saved two new resumes ('UX Designer Resume - TechCo' and its duplicate/rename 'UX Designer Resume - StartupInc') which appeared correctly on the dashboard. After reloading the application at http://localhost:7005/, both newly created resumes disappeared entirely from the dashboard; only the original 3 preset resumes (Product Designer, Marketing Manager, Software Engineer) remained. This indicates data is held only in in-memory/session state and is lost on re-access/reload.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No user feedback/confirmation shown after saving
    - Actual: Editing resume content and navigating back (auto-save), as well as renaming a resume via the dialog's 'Save' button, both persisted the changes but produced no visible toast, banner, or confirmation message. The page's 'Notifications' region remained an empty list after each save action, giving the user no explicit feedback that the save succeeded.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.