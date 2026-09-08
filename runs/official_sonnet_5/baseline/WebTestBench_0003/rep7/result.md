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
    - Actual: Employer dashboard job rows only expose two controls per job: an "applications count" toggle button and a delete (trash) button that opens a delete-confirmation dialog. There is no edit/pencil control, no clickable link to an edit form, and clicking the job title does not navigate anywhere. No route or UI path was found to modify an already-published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job posting does not validate salary min≤max or require at least one skill
    - Actual: Submitted "Post Job" with Title/Industry/Type/Location/Description filled, Minimum Salary=100000, Maximum Salary=50000 (min > max), and zero skills added. The app accepted it: toast "Job posted successfully!" appeared and the job "QA Test Engineer" was created and now appears in the employer dashboard listing "Austin, TX · $100k - $50k · Posted Yesterday" with no required skills — violating both the min≤max salary constraint and the at-least-one-skill requirement with no error feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job is not blocked
    - Actual: Submitted a second application to job-1 using the same email (test.applicant@example.com) already used for that job. App showed "Application submitted!" success toast again and created a second, separate application record ("Test Applicant Duplicate") visible in the employer dashboard alongside the original ("Test Applicant"), instead of rejecting the duplicate and leaving the original unchanged.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data does not persist across page reload
    - Actual: After submitting 2 new applications to job-1 (dashboard showed 4 applications, 5 total across all jobs), reloading /employer/dashboard reverted job-1 to 2 applications and total to 3 — the newly submitted applications were lost. No backend API calls were observed (only static assets) and localStorage was empty, indicating state is held only in memory and reset on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative date display is off by one day for newly created jobs/applications
    - Actual: Created job "Cloud Solutions Architect" (id job-1787624243378, timestamp resolves to 2026-08-25T02:17:23Z) while the browser's current time was 2026-08-25T02:17:52Z (29 seconds later, same day). The UI displays "Posted Yesterday" on both the job card and job details page instead of "Today"/"Just now". The same off-by-one-day behavior was observed for newly submitted applications in the employer dashboard, which showed "Yesterday" instead of "Today" immediately after submission.