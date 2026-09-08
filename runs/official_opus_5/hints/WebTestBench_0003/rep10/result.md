# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published job displays an incorrect salary range
    - Actual: Posted "Backend Engineer QA" (Engineering, Contract, Chicago IL, annual salary 110000–150000, skills Golang/PostgreSQL/Docker, description). The job does appear in the employer dashboard and the job-seeker listing, but the salary entered in the "Minimum/Maximum Salary (annual)" fields is rendered as "$110000 - $150000/hr" on both the card and the details page — mislabelled as hourly and not formatted (other jobs show "$140k - $180k").

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
    - Actual: Employer dashboard job rows expose only an applications toggle and a delete button (no edit/update control); the job details page and navigation offer no edit affordance either. All buttons/links on the dashboard: JobBoard, Find Jobs, Employer, Post a Job, Post New Job, per-job applications count and delete icon.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Required-field and salary-range validation missing on job posting form
    - Actual: A job ("QA Automation Engineer") was published with no industry, no employment type, no location and no skills — form submitted and job appeared in dashboard/listing without any error. Also a job with minimum salary 150000 > maximum 100000 ("Salary Validation Probe") was accepted and published with "Job posted successfully!". Only title/salary/description are enforced (HTML5 required) and negative salaries are blocked by min=0.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application by same email for the same job is accepted
    - Actual: After applying to job-1 as alice.tester@example.com, a second submission with the same email (name "Alice Duplicate") was accepted with the same "Application submitted!" confirmation. Employer dashboard now lists 2 records for job-1 with email alice.tester@example.com (count went 2→4). No duplicate-prevention message shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on reload
    - Actual: Before reload: 10 seeker jobs (3 newly posted), job-2 deleted, 4 employer applications incl. 2 new ones for job-1. After reloading http://localhost:7003/: seeker list back to the original 8 jobs (new jobs gone, deleted "Product Designer" restored) and dashboard shows 5 active jobs / 3 applications (seed values) — job-1 no longer contains the submitted "Alice Tester" application. localStorage is empty; state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show a date one day in the past
    - Actual: Browser date is Fri Aug 28 2026. Jobs posted seconds earlier show "Yesterday" ("Posted Yesterday" on details page, "Yesterday" on cards), and the application just submitted shows "Yesterday" in the employer dashboard instead of today's date.