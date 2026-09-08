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
    - Issue: Job editing is not implemented — no edit capability exists anywhere
    - Actual: The employer dashboard exposes only "toggle-applications" and "delete-job" controls per job (no edit/update button in the DOM); the job detail page has no edit affordance either. The plausible route /employer/edit-job/job-1 returns "404 — Oops! Page not found". An employer therefore cannot change a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range validation missing — minimum greater than maximum is accepted
    - Actual: Posted a job with Minimum Salary=200000 and Maximum Salary=100000 (all other fields valid). Instead of an error, the app showed "Job posted successfully!", redirected to the dashboard, incremented Active Jobs 5→6, and created the listing displaying an inverted range "$200k - $100k". (Missing required fields ARE blocked by native HTML5 validation, but the min<=max rule is not enforced.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application for the same job with an already-used email is accepted
    - Actual: After submitting as test.user@example.com for job-1, a second application with the same email (name "Duplicate Person") was submitted and showed "Application submitted!" with no duplicate warning. Employer dashboard now lists BOTH records under Senior Frontend Developer (Applications count went 2 -> 4: "Duplicate Person / test.user@example.com" and "Test User / test.user@example.com").


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all data resets to seed state on page reload
    - Actual: After reloading http://localhost:7003/: the 2 jobs published during testing ("Cloud Security Specialist", "QA Automation Engineer") are gone, the deleted job "Product Designer" is back, and the list returned to "Showing 8 jobs". Dashboard reverted to 5 active jobs / 3 applications with job-1 back to 2 applications (my 2 submitted applications lost). localStorage and sessionStorage are both empty — state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show an off-by-one relative date ("Yesterday" instead of today)
    - Actual: Browser current date is Thu Aug 27 2026. A job posted moments earlier displays "Posted Yesterday", and the applications submitted moments earlier ("Test User", "Duplicate Person") both display "Yesterday" in the employer dashboard, instead of "Today"/the current date.