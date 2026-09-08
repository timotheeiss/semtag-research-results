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
    - Actual: Each job row in the employer dashboard only exposes two controls: "toggle applications" and "delete". There is no edit/update button or link on the dashboard, on the job card, or on the job detail page, and no "edit" text/element exists anywhere in the DOM. Employers cannot modify a published job's details after creation.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary min>max not validated
    - Actual: Filled Post Job form with title, industry=Technology, type=Full-time, location=San Francisco CA, description, skill=Testing, min salary=100000, max salary=50000 (min > max) and submitted. The job was accepted and published: toast "Job posted successfully!" appeared and the job "QA Test Engineer" now shows on the employer dashboard with salary range "$100k - $50k" (invalid, min>max), instead of being rejected with a validation message.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application not blocked
    - Actual: Submitted a second application for job-1 using the same email (test.user@example.com) already used for that job. The app showed a success toast "Application submitted!" and the employer dashboard's job-1 application count increased from 3 to 4, i.e. the duplicate was accepted instead of being rejected. Expected: submission should be blocked with the existing application unchanged.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence across reload
    - Actual: Before reload: job-seeker listing had 9 jobs (including newly posted "QA Automation Specialist"; "QA Test Engineer" was deleted), and employer dashboard showed 6 active jobs / 5 applications (job-1 had 4 applications including the newly submitted one). After navigating/reloading the site, all changes were lost: job-seeker listing reverted to the original 8 jobs (QA Automation Specialist gone), and employer dashboard reverted to 5 active jobs / 3 applications (job-1 back to 2 applications, missing the submitted "Test User" application). Published jobs, submitted applications, and job deletions do not persist after reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative dates off by one day
    - Actual: Browser system date is 2026-08-26 (today). A job application submitted just now on job-1 displays its submission date as "Yesterday" instead of "Today". A job posted just now ("QA Test Engineer") displays "Posted Yesterday" instead of "Posted Today". Both newly created records show an inaccurate (one day off) relative date.