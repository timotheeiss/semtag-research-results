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
    - Issue: No edit functionality exists for published jobs
    - Actual: Inspected the employer dashboard (each job row only exposes an 'applications count' toggle and one icon-only action button) and the job detail page (/jobs/job-1, only shows a Back link and the application form) — no edit/update action, link, or form is present anywhere to modify a published job's details. Programmatic scan of all buttons/links on the dashboard confirmed only 'Post New Job', an applications-toggle button, and one unlabeled icon button (delete) per job row; no edit control exists.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job posted successfully despite minimum salary exceeding maximum salary
    - Actual: Submitted post-job form with all required fields filled but Minimum Salary=100000 and Maximum Salary=50000 (min > max). The app accepted it, showed 'Job posted successfully!' and created job 'QA Test Engineer' with salary range displayed as '$100k - $50k' in the employer dashboard, instead of rejecting it with feedback. (Separately confirmed: empty required fields and negative salary values ARE correctly blocked by native validation.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email accepted instead of rejected
    - Actual: Submitted a second application for job-1 using the same email 'test.applicant@example.com' as a prior application; the app showed a success toast and created a second distinct application record ('Test Applicant Duplicate') alongside the original ('Test Applicant') in the employer dashboard, instead of blocking it and leaving the original unchanged.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Published jobs and submitted applications do not persist across page reload
    - Actual: Created job 'Backend Engineer' (visible in both employer dashboard and job-seeker list) and 2 applications on job-1; after reloading the page (full navigation to the app URL), the created job disappeared entirely (direct visit to its URL showed 'Job not found'), the job-seeker list reverted to the original 8 seed jobs, and the submitted applications reverted to only the original 2 seed applications for job-1. All state resets to the initial seed data on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly submitted application date displayed as 'Yesterday' instead of 'Today'
    - Actual: Browser system date/time confirmed as 2026-08-25T10:19 UTC (matches today's date). Two applications submitted at this exact time to job-1 are displayed with submission date 'Yesterday' in the employer dashboard, which is inaccurate relative to the current date.