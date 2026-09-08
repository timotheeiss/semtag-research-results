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
    - Issue: No edit functionality available for jobs
    - Actual: Employer dashboard only shows toggle-applications and delete-job actions for each job. No edit button or link is visible or available through browser evaluation. Jobs cannot be edited after publication.


## Constraint
- [ ] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.
  - Bug Report:
    - Issue: No visible error feedback for invalid fields
    - Actual: Form uses HTML5 native validation which prevents submission and shows browser popups, but no visible on-page error messages or ARIA indicators. Invalid email field (invalid-email) and missing message field showed no visible user-facing error messages.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Invalid salary range allowed (min > max)
    - Actual: Submitted job with min salary $100,000 and max salary $80,000. Job was created successfully (Test Job). No validation error for salary constraint.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email applications are allowed
    - Actual: Submitted second application to job-1 using email sarah.j@example.com (already used). Application was accepted. Job-1 applications increased from 3 to 4.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data not persisted across page reload
    - Actual: Before reload: 6 active jobs (including Test Job), 6 applications. After reload: 5 active jobs, 3 applications. Test Job disappeared. Sarah Johnson's applications (submitted to job-1 and job-2) were lost. Only original applications remained.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [ ] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.
  - Bug Report:
    - Issue: Missing phone number and submission date in applications list
    - Actual: Applications show: name (Alex Johnson), email (alex.j@email.com), message (full text). Missing: phone number and submission date. These fields are not visible in the folded applications collection.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Applications missing submission dates
    - Actual: Job posting dates display correctly (Posted Jan 15, Yesterday). However, applications in employer dashboard do not display submission dates. No date/time information visible for when applications were submitted.