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
    - Issue: No edit functionality found
    - Actual: Explored employer dashboard job listing rows (only 'view applications' count button and one icon button, which opens a Delete confirmation dialog), the job details page, and DOM for any edit link/route. No edit form, edit button, or route to modify an existing published job's details was found anywhere in the app.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job published without any required skill
    - Actual: Filled title, industry, employment type, location, min/max salary, and description but left 'Required Skills' empty, then clicked Post Job. The job ('QA Test Engineer') was published successfully ('Job posted successfully! Your job listing is now live...') and appears in both employer dashboard and job-seeker listing with no skill tags, violating the 'at least one required skill' constraint.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email not blocked
    - Actual: Submitted a second application to job-1 using the same email (test.user@example.com) as an already-submitted application; the app accepted it, showed 'Application submitted!' success toast, and created a second distinct application record ('Test User Duplicate') instead of rejecting it or leaving the original unchanged.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: State does not persist across page reload/navigation
    - Actual: After posting 2 new jobs (Backend Engineer, QA Test Engineer) and submitting 2 applications to job-1 (Active Jobs 7, Total Applications 5, job-1 applications 4), navigating directly to a job URL and back to the homepage/dashboard reset all state to the original seed data: Active Jobs reverted to 5, Total Applications reverted to 3, job-1 applications reverted to 2, and the new jobs disappeared entirely ('Job not found' when visiting their URL).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Incorrect relative submission date
    - Actual: Submitted two applications at 2026-08-24T18:23-18:24 UTC (browser Date.now() confirms current date is 2026-08-24). Both appear in the employer dashboard application list labeled 'Yesterday' instead of 'Today', an off-by-one day error.