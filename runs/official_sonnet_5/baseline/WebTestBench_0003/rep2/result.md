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
    - Issue: No edit functionality for published jobs
    - Actual: Employer dashboard job cards only expose two controls: an applications-count toggle and a delete (trash) button. There is no edit/pencil icon or link, and clicking the job title does not navigate to an edit form. No way found to modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary min>max not validated
    - Actual: Submitted job with Minimum Salary=150000 and Maximum Salary=100000 (min > max). Form accepted it with no error message; job was published and displayed as "$150k - $100k" in both dashboard and job listing.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email not blocked
    - Actual: Submitted a second application to the same job (QA Automation Engineer) using the same email (test.applicant@example.com) as an existing application. It was accepted with the same "Application submitted!" success message and now appears as a second, separate application entry ("Duplicate Applicant") in the employer dashboard instead of being rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: State does not persist across page reload
    - Actual: After reloading http://localhost:6003/, all changes were lost: the newly published "QA Automation Engineer" job disappeared, and the previously deleted "DevOps Engineer (Contract)" job reappeared. The listing reverted to the original 8 seed jobs, indicating jobs, applications, and deletions are stored only in memory and not persisted.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Incorrect relative posting date for newly created job
    - Actual: Job posted at 2026-08-24 16:12 UTC (current date 2026-08-24) displays posting date as "Yesterday" instead of "Today" in both employer dashboard and job listing.