# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published salary range is misrepresented (annual input rendered as hourly, unformatted)
    - Actual: Posted "Quality Assurance Lead" (Engineering, Contract, Seattle WA, min 90000 / max 120000 from fields labelled "Minimum/Maximum Salary (annual)", description, skill Playwright). The job appears in both the employer dashboard and the seeker listing, but its salary shows as "$90000 - $120000/hr" instead of an annual range like "$90k - $120k".

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
    - Actual: Employer dashboard job rows expose only "applications" toggle and "delete" buttons (enumerated all buttons/links: no edit control); the job detail page and post-job page offer no edit entry point, so a published job's details cannot be changed.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range and required-skill validation missing
    - Actual: Posted a job with Minimum Salary 200000 > Maximum Salary 100000 and zero required skills; it was accepted with "Job posted successfully!" and appears on the dashboard as "QA Automation Engineer — Austin, TX · $200k - $100k". Only empty-field required validation works.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with the same email for the same job is accepted
    - Actual: Second submission on job-1 with the already-used email alice.tester@example.com showed "Application submitted!" and created a second record; dashboard job-1 now lists both "Alice Tester" and "Alice Duplicate" with alice.tester@example.com (Applications (4)).


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on reload
    - Actual: After reloading http://localhost:7003/: seeker list back to "Showing 8 jobs", the two jobs I posted (Quality Assurance Lead, QA Automation Engineer) are gone, the deleted "Product Designer" (job-2) reappeared, and the dashboard shows Active Jobs 5 / Applications 3 with job-1 back to 2 applications (my 2 submitted applications lost). No localStorage entries are written.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Posting/submission date is off by one day
    - Actual: Current date is 2026-08-28 (page Date: Fri Aug 28 2026 00:39 GMT+0000). Job created just now displays "Posted Yesterday" and applications submitted minutes ago display "Yesterday" instead of "Today".