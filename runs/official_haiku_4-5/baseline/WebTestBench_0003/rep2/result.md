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
    - Issue: No edit functionality found for published jobs
    - Actual: Employer dashboard shows each job with only two action buttons: applications count button and delete button (icon). No edit button or link is visible. Clicking on job title does not open an edit page. No edit functionality appears to be implemented or accessible from the UI.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job published with missing required fields and invalid salary range without validation
    - Actual: Posted "Test Invalid Job" with: no industry selected, no employment type selected, no location selected, no required skills added, invalid salary range ($100k minimum, $50k maximum where min > max). Job was accepted and posted successfully. System should have rejected it with clear error messages for: missing industry, missing employment type, missing location, missing required skills (at least one required), and invalid salary range (min > max).

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: System allows duplicate applications with same email for same job
    - Actual: Submitted first application with email john.smith@example.com (John Smith). Then submitted second application with same email but different name (Another User). Both applications were accepted and stored separately, resulting in 2 applications from the same email for Test QA Engineer job. System should have rejected the duplicate.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Published jobs and applications do not persist after page reload
    - Actual: Before reload: Employer dashboard showed 6 active jobs (including Test QA Engineer with 2 applications) and 5 total applications. After navigating to Find Jobs and reloading the page, then returning to Employer Dashboard: now shows only 5 active jobs and 3 total applications. Test QA Engineer job (with 2 applications) has disappeared. Job seeker listing went from 9 jobs (including Test QA Engineer) to 8 jobs without the Test QA Engineer.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly published jobs and applications show incorrect relative dates (off by one day)
    - Actual: Current system date is 2026-08-24. Test QA Engineer job was created on 2026-08-24 but displayed as "Posted Yesterday" instead of "Posted Today". Applications submitted on 2026-08-24 displayed as "Yesterday" submission date instead of "Today". This indicates date calculations are off by one day - jobs/applications created today should display as today's date, not yesterday's date.