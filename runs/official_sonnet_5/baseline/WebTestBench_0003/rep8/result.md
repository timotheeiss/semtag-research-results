# Test Result

## Functionality
- [X] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.

- [X] FT-2: Employers can delete one of their posted jobs only after confirming the action; the job and its applications are then removed from the employer dashboard and the job-seeker listing.

- [X] FT-3: The employer dashboard lists all active jobs posted by the current employer and shows accurate active-job and total-application counts.

- [X] FT-4: The employer dashboard lets the employer open each posted job's application list and view the applications associated with that job.

- [X] FT-5: Job seekers can browse the complete list of available jobs see an accurate result count and open any listing to view its details.

- [X] FT-6: Job seekers can filter listings by industry, location, and employment type individually or in combination; clearing the criteria restores the full list.

- [X] FT-7: Keyword search matches job titles, company names, and required skills without case sensitivity and immediately updates the visible results and count.

- [X] FT-8: On a job details page, a job seeker can submit an application with a name, syntactically valid email address, optional phone number, and message and receives a clear success confirmation.

- [ ] FT-18: Employers can edit the details of a published job and the updated information is reflected in both employer and job-seeker views.
  - Bug Report:
    - Issue: No edit functionality available for published jobs
    - Actual: Searched employer dashboard job listing rows (only "view applications" and "delete/trash" icon buttons present, no edit control) and the job details page (/jobs/job-...) for a job seeker view — no "Edit" link, button, or affordance exists anywhere in the UI to modify a published job's details. Employers cannot edit a job once posted.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job creation lacks required-skill and salary-range validation
    - Actual: Posted "QA Automation Engineer" with all fields filled except Required Skills left empty (no skill tags added) — job was accepted and published ("Job posted successfully!") with 0 skills, violating "at least one required skill" rule. Separately, posted "Salary Test Job" with Minimum Salary=150000 and Maximum Salary=100000 (min > max) — job was accepted and published showing "$150k - $100k" with no error, violating the "minimum no greater than maximum" rule. Only the primitive required-text-field/select HTML validation works; the skill-count and salary-order business rules are not enforced.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email application not blocked
    - Actual: Submitted a second application to job-1 using the same email (test.user@example.com) already used for that job. The app showed a success toast and created a second, separate application record ("Duplicate Tester") alongside the original ("Test User"), instead of rejecting the duplicate submission. Applications count for job-1 increased from prior value to 4.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: State not persisted across page reload
    - Actual: Before reload: employer dashboard showed 7 active jobs (5 original + 2 newly posted: "QA Automation Engineer", "Salary Test Job") and job-1 had 4 applications (2 original + 2 newly submitted). After navigating/reloading the app (job seeker list and employer dashboard), state reverted to the original seed data: only 8 jobs shown on job-seeker list (new jobs gone), employer dashboard back to 5 active jobs and Total Applications back to 3 (the 2 newly submitted applications for job-1 were also gone). Published jobs, submitted applications, and any changes are lost on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Posting/submission date incorrect relative to current date
    - Actual: Current date is 2026-08-25. Newly posted job "Backend Engineer" and newly submitted applications (e.g. "Test User") display posting/submission date as "Yesterday" instead of "Today", indicating an off-by-one-day error in the relative date calculation.