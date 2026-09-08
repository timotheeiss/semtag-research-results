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
    - Issue: Job editing is not implemented — no edit affordance or route exists
    - Actual: The employer dashboard exposes only two controls per job: a "view applications" toggle (users/chevron-down icon) and a delete button (trash2 icon). There is no Edit button, link, or menu anywhere on the dashboard, and the job details page (/jobs/job-1) offers only "Back to all jobs" plus the application form. Probing /employer/edit-job/job-1 returns the app's "404 — Oops! Page not found" page. An employer therefore cannot modify a published job's details at all.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Missing job-posting validation: invalid salary range and empty skills accepted
    - Actual: Submitted "QA Automation Engineer" with min salary 200000 > max salary 100000 and ZERO required skills. The job was published successfully (toast "Job posted successfully!") and appears in the dashboard as "$200k - $100k". No validation feedback was shown. Additionally, submitting a completely empty form produced no error message or field-level feedback at all (form silently did nothing).

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: No duplicate-email guard on job applications
    - Actual: After successfully applying to job-1 as alex.rivera@example.com, a second application was submitted for the SAME job with the same email ("Alex R Duplicate"). It was accepted with the same success toast "Application submitted!". The employer dashboard now shows "Applications (4)" for Senior Frontend Developer containing two separate records both with alex.rivera@example.com. No error or warning was shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state is in-memory and resets on page reload
    - Actual: Before reload: 8 seeker jobs incl. newly published "QA Automation Engineer", "Product Designer" deleted, Senior Frontend Developer with 4 applications (2 newly submitted), dashboard 5 active / 4 applications. After reloading http://localhost:6003/: the published job "QA Automation Engineer" is GONE, the deleted "Product Designer" is BACK, and the dashboard resets to 5 active jobs / 3 applications with Senior Frontend Developer back to 2 applications — losing both new applications. localStorage/sessionStorage are empty, confirming no persistence layer.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Off-by-one relative date: items created today are labelled "Yesterday"
    - Actual: Browser clock is Fri Aug 28 2026 08:03 UTC. The job posted moments earlier displays "Posted Yesterday" in the employer dashboard and "Yesterday" on its job-seeker card. Both applications submitted moments earlier display "Yesterday" in the employer dashboard. Expected "Today".