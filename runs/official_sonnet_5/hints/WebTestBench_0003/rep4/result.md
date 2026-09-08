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
    - Issue: No edit functionality for published jobs
    - Actual: Employer dashboard job cards only expose an applications-toggle button and a delete button; there is no edit icon/link, clicking the job title does not navigate anywhere, and no edit route/form exists anywhere in the app.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary min>max not validated; job published with invalid salary range
    - Actual: Posted a job with Minimum Salary=150000 and Maximum Salary=90000 (min greater than max); the form submitted without any error, showed "Job posted successfully!", and the job appeared live everywhere displaying salary "$150k - $90k", a nonsensical range that should have been rejected with clear feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job not blocked
    - Actual: Submitted a second application to job-1 using the same email (test.user@example.com) already used for that job; the app showed the same "Application submitted!" success toast and created a second, separate application record (app-1787677926229) alongside the first (app-1787677919429), confirmed in the employer dashboard's application list for that job.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence of state across page reload
    - Actual: After a full page reload, the deleted "Product Designer" job reappeared in both the job-seeker listing and employer dashboard, the newly-posted "QA Automation Engineer" job disappeared, and the 2 newly-submitted applications for job-1 were gone (Applications count for job-1 reverted from 4 to 2, dashboard total from 5 to 3). All changes exist only in-memory and are lost on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show an inaccurate relative date ("Yesterday" instead of "Today")
    - Actual: Browser clock read 2026-08-25T17:12 UTC (matching current date). Immediately after submitting an application, its date was displayed as "Yesterday" rather than "Today". Immediately after posting a new job, its posted date was displayed as "Posted Yesterday" instead of "Posted Today". This is an off-by-one relative-date calculation bug.