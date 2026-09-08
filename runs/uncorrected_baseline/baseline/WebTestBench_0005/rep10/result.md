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
    - Issue: No validation for invalid/conflicting date ranges
    - Actual: Setting an Experience entry's Start Date to 2021-01 and End Date to 2019-01 (end before start) was accepted with no error or warning; the Live Preview silently displayed "Jan 2021 - Jan 2019".

- [X] CS-10: When adding past work or education experience, you must use the correct time format; otherwise, you will not be able to add it.

- [ ] CS-11: The saved resume version is persistently stored and will not be lost upon re-access.
  - Bug Report:
    - Issue: No persistent storage; data lost on reload
    - Actual: After creating/renaming/duplicating resumes (new titles "UX Designer Resume - TechCo", "UX Designer Resume - StartupX", "Untitled Resume" all visible in the list), reloading the page at http://localhost:7005/ caused all of them to disappear, leaving only the original 3 seed resumes. Inspection via browser_evaluate showed both localStorage and sessionStorage are empty, confirming no persistence mechanism is used.


## Interaction
- [X] IX-12: The preview function synchronizes edited content in real time, with no delay or display errors.

- [X] IX-13: The changes to chapter order and titles take effect immediately, and the operation is smooth.

- [ ] IX-14: Users can save their resumes and provide feedback after successful saving.
  - Bug Report:
    - Issue: No save confirmation/feedback shown
    - Actual: There is no explicit Save button; edits are persisted to the in-memory list only when navigating back to the dashboard. After editing a Full Name field and clicking the back button, the resume appeared in the dashboard list, but the "Notifications" region remained an empty list with no toast, message, or other success feedback shown at any point during editing, renaming, or duplicating.


## Content
- [X] CT-15: The resume template is displayed in its entirety, with no layout issues.

- [X] CT-16: The entered personal information was completely retained, with no fields missing.

- [X] CT-17: Resume information for multiple versions is stored independently, and there is no content sharing between versions.

- [X] CT-18: The resume title should remain consistent across multiple pages.