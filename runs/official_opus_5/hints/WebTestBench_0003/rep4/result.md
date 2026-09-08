# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published job's salary range is rendered incorrectly (annual salary shown as hourly, unformatted)
    - Actual: Posted "Automation QA Specialist" (Engineering, Contract, Austin TX, $95,000–$130,000 annual, description, skills Playwright/Cypress). It appears in the employer dashboard and in the job-seeker list ("Showing 11 jobs"), but salary displays as "$95000 - $130000/hr" on both the card and the detail page, i.e. an annual range labelled per hour and not formatted like seed jobs ($140k - $180k). Posting date also shows "Yesterday" though posted today.

- [X] FT-2: Employers can delete one of their posted jobs only after confirming the action; the job and its applications are then removed from the employer dashboard and the job-seeker listing.

- [X] FT-3: The employer dashboard lists all active jobs posted by the current employer and shows accurate active-job and total-application counts.

- [X] FT-4: The employer dashboard lets the employer open each posted job's application list and view the applications associated with that job.

- [X] FT-5: Job seekers can browse the complete list of available jobs see an accurate result count and open any listing to view its details.

- [X] FT-6: Job seekers can filter listings by industry, location, and employment type individually or in combination; clearing the criteria restores the full list.

- [X] FT-7: Keyword search matches job titles, company names, and required skills without case sensitivity and immediately updates the visible results and count.

- [X] FT-8: On a job details page, a job seeker can submit an application with a name, syntactically valid email address, optional phone number, and message and receives a clear success confirmation.

- [ ] FT-18: Employers can edit the details of a published job and the updated information is reflected in both employer and job-seeker views.
  - Bug Report:
    - Issue: No job editing capability exists
    - Actual: Employer dashboard job rows expose only "toggle applications" and "delete" buttons; no edit link/button anywhere in the dashboard DOM (no element or text matching /edit/i), and the job detail page offers no employer edit control. Probing /employer/edit/job-1 returns the app's 404 page. Published jobs therefore cannot be modified.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Missing industry/type/location/skills accepted; salary min greater than max accepted
    - Actual: (1) Job "QA Test Engineer" published with no industry, employment type, location or skills (only title/salary/description are natively required). (2) Job "Salary Validation Probe" published with min 200000 > max 100000 with no error. Negative salaries were correctly blocked (native min=0).

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job is accepted
    - Actual: After applying to job-1 as test.applicant@example.com, a second submission with the same email (name "Test Applicant Duplicate") succeeded with the same "Application submitted!" toast. Dashboard job-1 now lists two applications from test.applicant@example.com (app-1787852412812 and app-1787852449358); count rose 4→5.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all changes are lost on page reload
    - Actual: Before reload: 10 listed jobs incl. posted "Automation QA Specialist", job-2 deleted, job-1 with 5 applications (dashboard 7 jobs / 5 apps). After reloading http://localhost:7003/: "Showing 8 jobs", posted job absent, deleted job-2 back, dashboard 5 active jobs / 3 applications with job-1 back to 2 applications. localStorage is empty — state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: New jobs and applications show a submission date one day in the past
    - Actual: Browser date is Thu Aug 27 2026. Jobs posted just now display "Posted Yesterday" (card + detail page of job-1787852621201) and applications submitted just now display "Yesterday" in the employer dashboard, instead of "Today"/Aug 27.