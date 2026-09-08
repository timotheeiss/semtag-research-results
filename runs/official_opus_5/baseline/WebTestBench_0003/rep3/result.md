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
    - Issue: No edit capability for published jobs
    - Actual: Employer dashboard job rows expose only an applications-count toggle (users/chevron icon) and a trash/delete button — no edit control anywhere on the dashboard or job details page. Navigating to /employer/edit-job/job-1 renders the 404 "Oops! Page not found" screen, so job details cannot be edited.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range min>max and missing required skills are not validated
    - Actual: Posted "QA Automation Engineer" with min salary 120000 and max 80000 — accepted, toast "Job posted successfully!", dashboard shows "$120k - $80k". Posted "Negative Salary Test Job" with zero skills — accepted as well. (Required text/select fields and negative salaries ARE blocked: negative min gives "Value must be greater than or equal to 0.")

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job is accepted
    - Actual: Submitted a second application to job-1 with the already-used email alice.tester@example.com (name "Alice Duplicate"); it was accepted with a success toast. Dashboard now lists both "Alice Duplicate / alice.tester@example.com" and "Alice Tester / alice.tester@example.com" under Senior Frontend Developer (count went 2→4). No duplicate-email error was shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all created data is lost on page reload
    - Actual: Before reload: 11 jobs listed (3 newly posted) and 5 total applications (2 newly submitted to job-1). After reloading the app, the job list shows only the 8 seed jobs and the dashboard shows Active Jobs 5 / Total Applications 3 with job-1 back to 2 applications. localStorage is empty — state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show a date one day in the past
    - Actual: Current browser date is Thu Aug 27 2026. A job posted just now displays "Posted Yesterday" in the dashboard, and applications submitted just now display "Yesterday" instead of today's date/"Today".