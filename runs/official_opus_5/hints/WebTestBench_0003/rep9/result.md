# Test Result

## Functionality
- [ ] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.
  - Bug Report:
    - Issue: Published job's salary range is displayed incorrectly
    - Actual: Posted "QA Automation Engineer" (Engineering, Contract, Seattle WA, min 95000 / max 130000 annual, description, skill Playwright). It appears in the dashboard and in the job-seeker list (Showing 11 jobs), but the salary range renders as "$95000 - $130000/hr" — annual values shown as an hourly rate and unformatted — instead of "$95k - $130k". Posting date also shown as "Yesterday" although posted today.

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
    - Actual: Employer dashboard job rows expose only an applications-count toggle and a delete button (accessibility snapshot shows no edit control); the job detail page offers only Back/apply form, and /employer/post-job is create-only. There is no way to edit a published job.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Required-field and salary-range validation not enforced
    - Actual: Submitting with only title, salaries and description (no industry, employment type, location, or skills) published "QA Validation Probe Job" successfully. Submitting min=150000 / max=100000 published "QA Salary Range Probe" showing "$150k - $100k" with no error. (Only empty title/salary/description and negative salaries are blocked by native HTML validation.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job is accepted
    - Actual: After applying to job-1 as quinn.tester@example.com, a second submission with the same email ("Quinn Tester Dup") showed the success message and was stored: job-1 now shows Applications (4) with two records for quinn.tester@example.com; employer application count rose 3→5. Same behavior seen on job-3.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all changes are lost on page reload
    - Actual: After reloading http://localhost:7003/, the job list reverted to the original 8 seeded jobs: the 3 newly published jobs disappeared, the deleted "Product Designer" reappeared, and the employer dashboard reverted to 5 active jobs / 3 total applications (the 3 applications submitted during testing were gone).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: New jobs and applications show a date one day in the past
    - Actual: Browser date is 2026-08-28. Jobs just published show "Posted Yesterday" on the dashboard and "Yesterday" on job cards; applications just submitted show "Yesterday" in the employer dashboard instead of today's date.