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
    - Issue: No validation for invalid/conflicting time frames in work experience
    - Actual: Set Experience 1 End Date (2019-01) before Start Date (2020-01): app accepted it and rendered 'Jan 2020 - Jan 2019' in preview with no error. Also added Experience 2 (2021-01 to 2022-01) fully overlapping Experience 1 (2020-01 to 2022-06): no conflict warning or blocking occurred.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resume data is not persisted across page reloads
    - Actual: Created and filled a new resume 'QA Test Resume - Software Engineer' (with contact, summary, 2 experiences, education, skills), navigated back to the list where it appeared correctly, then reopened it to confirm all data was intact within the session. However, after performing a full page reload (navigating to http://localhost:7005/), the resume disappeared entirely from 'Your Resumes' list — only the original 3 preset resumes (Product Designer, Marketing Manager, Software Engineer) remained. Data is stored only in-memory/session state, not persisted to backend/localStorage.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No explicit save confirmation/feedback shown to the user
    - Actual: There is no explicit 'Save' button in the resume editor; changes appear to auto-persist to app state as you type. Edited the Phone field on 'Marketing Manager Resume' and checked the Notifications region (list) immediately after — it remained empty (no toast/snackbar/banner). The Rename dialog's 'Save' button also produced no visible confirmation message. The only indirect evidence of a save is the 'Updated' date changing on the resume card after returning to the list, which is not an active feedback mechanism (e.g., toast, success message, or button state change) to the user at the time of saving.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.