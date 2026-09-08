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
    - Actual: Searched employer dashboard job cards (only 'toggle applications' and 'delete' actions available, no edit control) and the job detail page (no edit affordance for the owning employer); there is no way to modify a job's title, description, salary, location, or other details after publishing


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary min>max not validated; job with invalid salary range publishes successfully
    - Actual: Submitted job form with Minimum Salary=150000 and Maximum Salary=100000 (min > max) while all other required fields were valid; app showed 'Job posted successfully!' and created job 'QA Test Engineer' displaying '$150k - $100k' in both dashboard and (expected) job listing, instead of rejecting the invalid salary range with feedback

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: No duplicate-email prevention on job applications
    - Actual: Submitted a second application to job-6 using the same email (jamie.lee@example.com) already used for that job; app accepted it with a success toast and dashboard now shows 2 separate applications (Jamie Lee and Jamie Duplicate) for job-6 instead of blocking the duplicate


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence across page reload - all in-session changes are lost
    - Actual: After reloading the page, the newly published 'Backend Engineer' job (visiting its detail URL showed 'Job not found') and all newly submitted applications disappeared; employer dashboard reverted to the original seed state (5 active jobs, 3 applications) as if no create/apply/delete actions had ever happened


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Incorrect relative submission date shown for new applications
    - Actual: Two applications submitted moments ago (current date 2026-08-26, confirmed via page JS Date) both display submission date as 'Yesterday' in the employer dashboard instead of 'Today', for job-4 (Test User) and job-6 (Jamie Lee)