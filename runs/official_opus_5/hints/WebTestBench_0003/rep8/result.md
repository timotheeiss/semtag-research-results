# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published salary range is displayed incorrectly (annual input rendered as hourly, unformatted)
    - Actual: Posting "QA Automation Engineer" (Engineering / Contract / Boston, MA / min 95000 max 125000 entered in fields labelled "Minimum/Maximum Salary (annual)" / description / skills Playwright, Cypress) succeeded ("Job posted successfully!") and the job appears in the employer dashboard (Active Jobs 7→8) and in the job-seeker list ("Showing 11 jobs"). However the salary range renders as "$95000 - $125000/hr" on both the job card and the job detail page — the annual amounts are shown as an hourly rate and are unformatted, unlike seeded annual jobs ("$140k - $180k").

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
    - Actual: Employer dashboard job rows expose only "applications" toggle and "delete" buttons (full button inventory: nav links, Post New Job, per-job applications/delete). The job detail page exposes only the application form and "Back to all jobs". There is no edit control or edit form anywhere, so a published job's details cannot be modified.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Required fields and salary min≤max not enforced
    - Actual: 1) Submitting with only title/salary/description (no industry, employment type, location, no skills) published the job "QA Missing Select Test" (dashboard row shows blank industry/type/location, active jobs 5→6) with no error. 2) Submitting min 90000 / max 50000 published "QA Salary Range Test" with salary "$90k - $50k" (active jobs 6→7), no min≤max validation. Only negative salaries were blocked (native min=0: "Value must be greater than or equal to 0.").

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email accepted for the same job
    - Actual: After applying to job-1 as test.applicant@example.com, a second application with the identical email was accepted, showing "Application submitted!" again. Employer dashboard now lists two records for test.applicant@example.com under "Senior Frontend Developer" (Test Applicant and Duplicate Person), count rose 2→4. No duplicate-email error was shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on reload
    - Actual: After reloading http://localhost:7003/: job-seeker list back to "Showing 8 jobs" (was 10), the published job "QA Automation Engineer" (job-1787886043798) is gone, and the deleted "Product Designer" (job-2) has reappeared. Employer dashboard reset to 5 active jobs and 3 total applications, so the 2 submitted applications were lost too.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Posting/submission date off by one day
    - Actual: Current date is 2026-08-28. Jobs created just now display "Posted Yesterday" and applications submitted just now display "Yesterday" in the employer dashboard instead of "Today"/current date.