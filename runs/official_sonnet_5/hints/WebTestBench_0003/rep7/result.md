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
    - Issue: No edit-job feature exists
    - Actual: Inspected the employer dashboard's job listing rows: each job only exposes an applications-count toggle and a delete (trash) icon button — no edit/pencil control, no edit link, and no /employer/edit-job route. Checked all buttons and links on the dashboard page via DOM inspection; none reference editing a job. There is no way for an employer to modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job publish accepts invalid salary range and missing required skill
    - Actual: Submitted the Post Job form with Min Salary=100000, Max Salary=50000 (min > max), no skills added, but all other required fields filled. The form accepted it: toast 'Job posted successfully!' appeared and the job 'QA Test Job' was created and now appears in the employer dashboard showing 'Remote · $100k - $50k · Posted Yesterday' with 0 required skills. No validation error was shown for the invalid salary range or missing skill.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application email not blocked
    - Actual: After submitting an application to job-2 with email priya.nair@example.com, submitted a second application to the same job using the identical email address ('Priya Nair Duplicate'). The app accepted it with the same success toast ('Application submitted!') and no warning. The employer dashboard confirmed job-2's application count increased from 2 to 3 (total 4 to 5), meaning a second application with an already-used email was saved rather than rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data not persisted across reload
    - Actual: Submitted an application for job-4 (Jamie Carter) which appeared correctly in the employer dashboard (job-4 applications count = 1, total = 4) while navigating via in-app links. After reloading the page (navigating to the same dashboard URL, simulating a reload), job-4's application count reverted to 0 and total reverted to 3 — the submitted application was lost. Newly submitted applications do not survive a page reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative date off by one day for new submissions
    - Actual: Current date is 2026-08-26. A job posted just now ('QA Automation Engineer') displayed 'Posted Yesterday' instead of 'Posted Today' in both the employer dashboard and job-seeker listing. Likewise, an application submitted just now (Priya Nair, for Product Designer) displayed a submission date of 'Yesterday' instead of 'Today' in the employer dashboard's applications list. Newly created records show an inaccurate (one-day-off) relative date.