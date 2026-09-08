# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published listing displays the salary range incorrectly (annual values labelled "/hr", unformatted)
    - Actual: Posted "Cloud Security Specialist" (Engineering, Contract, Boston MA, min 110000 / max 150000 annual per the "Minimum/Maximum Salary (annual)" fields, description, skill "Zero Trust"). The job is created and appears in both the employer dashboard and the job-seeker list (count went 8 -> 10 incl. earlier test job), but its salary renders as "$110000 - $150000/hr" instead of an annual range like "$110k - $150k".

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
    - Actual: The employer dashboard exposes only "Post New Job", per-job applications toggle and delete buttons — no edit control exists for any job. The job detail page likewise offers no edit action, so a published job's details cannot be updated.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: No validation of salary range order or of required skills
    - Actual: Submitted a job with Minimum Salary 150000 > Maximum Salary 90000 and zero required skills. It was published with "Job posted successfully!" and appears in the dashboard as "QA Automation Engineer — Austin, TX · $150k - $90k". (Empty-form submit is correctly blocked by required-field validation, and salary inputs have min=0, but the min<=max rule and the at-least-one-skill rule are not enforced.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email accepted for the same job
    - Actual: After applying to job-1 as alice.tester@example.com, a second submission with the identical email (name "Alice Duplicate") was accepted with the same "Application submitted!" confirmation. Employer dashboard now shows job-1 with 4 applications including two records for alice.tester@example.com.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on page reload
    - Actual: Before reload: 10 jobs incl. 2 newly posted, job-2 deleted, job-1 with 4 applications. After reloading http://localhost:7003/: job-seeker list shows the original "Showing 8 jobs" (both new jobs gone, deleted job-2 restored) and the dashboard shows Active Jobs 5 / Total Applications 3 with job-1 back to 2 seed applications (submitted applications lost). No localStorage entries are written.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show an inaccurate (off-by-one-day) submission date
    - Actual: Browser clock is Thu Aug 27 2026 22:22 UTC (matches current date). A job posted just now displays "Posted Yesterday" and applications submitted just now display "Yesterday" in the employer dashboard instead of Today/Aug 27.