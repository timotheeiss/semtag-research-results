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
    - Issue: No validation for invalid/conflicting time frames in experience entries
    - Actual: Setting End Date (2019-01) earlier than Start Date (2020-01) for a work experience entry was accepted without any warning or error. The Live Preview rendered the illogical range 'Jan 2020 - Jan 2019' with no validation message.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Newly created/edited resumes are not persisted across a page reload
    - Actual: Created two resumes ('QA Test Resume - Backend Role' and its duplicate/rename 'QA Test Resume - Sales Role') with full content (contact, summary, experience, education, skills, LinkedIn/Website). After performing an actual page reload (navigating to http://localhost:7005/), both resumes disappeared entirely from 'Your Resumes' — only the original 3 seeded sample resumes (Product Designer, Marketing Manager, Software Engineer) remained. This shows data is kept only in-memory/session state and is lost on reload, violating the persistence requirement.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No user feedback/confirmation shown after auto-saving resume edits
    - Actual: The app auto-persists edits (data reappears correctly after navigating back to the resume), but no explicit 'Save' action or confirmation feedback (toast, banner, checkmark, 'Saved' text) is shown after edits. The Notifications region/toast list remained empty after multiple field edits and navigation events.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.