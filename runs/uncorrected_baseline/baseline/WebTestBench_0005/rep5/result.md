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
    - Issue: No validation of experience/education time frame validity or conflicts
    - Actual: Added a second Experience entry with Start Date 2021-06 and End Date 2020-01 (end before start, an impossible time frame) — the app accepted it silently, rendering "Jun 2021 - Jan 2020" in the preview with no error/warning. Also tested overlapping periods (2020-01–2022-06 vs 2021-01–2021-12) between two experience entries; both were accepted with no conflict warning.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resume data is not persisted across page reload
    - Actual: Created "QA Test Resume" and its duplicate/rename "QA Test Resume - Job Application 2", both visible in the resume list before reload. After navigating to http://localhost:7005/ again (full reload), both custom resumes disappeared entirely — only the 3 original seeded resumes (Product Designer, Marketing Manager, Software Engineer) remained. Data exists only in-memory, not persistently stored.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No user-facing feedback/confirmation shown after saving
    - Actual: There is no explicit "Save" action for resume content (it auto-saves to memory); throughout editing, renaming (via dialog "Save" button), navigating back to the list, etc., the "Notifications" region remained an empty list at every check — no toast, banner, or confirmation message ever appeared to indicate a successful save.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.