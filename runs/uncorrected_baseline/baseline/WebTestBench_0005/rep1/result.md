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
    - Issue: Overlapping/conflicting time periods across different experience entries are not detected or prevented
    - Actual: Added a second Work Experience entry with dates 2021-01 to 2021-06, which fully overlaps with the first entry's 2020-01 to 2022-06 range. The app accepted this without any warning, error, or conflict indication, and both entries were shown in the Live Preview simultaneously.

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: Saved resumes are not persisted across a page reload
    - Actual: After creating "QA Test Engineer Resume" and its duplicate "QA Test Engineer Resume - Startup Application" (both fully populated with data and visible in the dashboard list), reloading the page (navigating to http://localhost:7005/ again) caused both resumes to disappear entirely. Only the 3 original sample resumes (Product Designer, Marketing Manager, Software Engineer) remained, indicating data is held only in-memory/session state and not persisted to storage.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No user feedback (toast/notification/confirmation) is shown after saving actions
    - Actual: Throughout resume creation, field edits, template switching, section reordering, renaming, and duplicating, the "Notifications" region on the page remained empty (no list items) at every observed snapshot. No success message, toast, or confirmation of any kind appeared to indicate a save had occurred.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.