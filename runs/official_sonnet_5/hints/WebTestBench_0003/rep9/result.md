# Test Result

## Functionality
- [X] FT-1: Employers can publish a job listing with a title, industry, employment type, location, valid salary range, description, and required skills; the new listing appears in the employer dashboard and the job-seeker listing.

- [X] FT-2: Employers can delete one of their posted jobs only after confirming the action; the job and its applications are then removed from the employer dashboard and the job-seeker listing.

- [ ] FT-3: The employer dashboard lists all active jobs posted by the current employer and shows accurate active-job and total-application counts.
  - Bug Report:
    - Issue: Dashboard shows jobs not posted by the current employer
    - Actual: Employer dashboard for "Sarah Chen / TechCorp Solutions" lists jobs from unrelated companies: Marketing Manager (BrandForward Agency), Part-Time Content Writer (ContentHub), DevOps Engineer (CloudScale Inc), while excluding other TechCorp-unrelated jobs like Registered Nurse (HealthPlus Medical), Data Analyst (FinanceFirst), Elementary School Teacher (Bright Futures Academy) inconsistently. Active-job/application counts are internally consistent with what's displayed, but the set of jobs shown does not correspond to "posted by the current employer."

- [X] FT-4: The employer dashboard lets the employer open each posted job's application list and view the applications associated with that job.

- [X] FT-5: Job seekers can browse the complete list of available jobs see an accurate result count and open any listing to view its details.

- [X] FT-6: Job seekers can filter listings by industry, location, and employment type individually or in combination; clearing the criteria restores the full list.

- [X] FT-7: Keyword search matches job titles, company names, and required skills without case sensitivity and immediately updates the visible results and count.

- [X] FT-8: On a job details page, a job seeker can submit an application with a name, syntactically valid email address, optional phone number, and message and receives a clear success confirmation.

- [ ] FT-18: Employers can edit the details of a published job and the updated information is reflected in both employer and job-seeker views.
  - Bug Report:
    - Issue: No edit functionality for published jobs
    - Actual: Employer dashboard job rows only expose an "applications" toggle and a delete (trash) button; no edit button/link exists anywhere on the dashboard or job cards. Clicking the job title does not navigate to an edit form. DOM search for any button containing "edit" returned no matches.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job posted successfully despite invalid salary range (min > max)
    - Actual: Posted a job "QA Test Engineer" with Minimum Salary=100000 and Maximum Salary=50000 (min > max). No validation error was shown; the job was created and appeared in both dashboard and job-seeker listing displaying "$100k - $50k", an obviously invalid range.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job was accepted
    - Actual: Submitted a second application to job-1 using the same email "test.user@example.com" already used for that job; the app showed "Application submitted!" success again and created a second distinct application record ("Test User Duplicate") visible in the employer dashboard alongside the original, instead of being blocked.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: All state (published jobs, submitted applications, deletions) is lost on page reload
    - Actual: Before reload: 9 jobs listed (after posting 1 job and deleting 1), job-1 had 4 applications, employer dashboard showed 7 active jobs/5 applications. After navigating to http://localhost:6003/ (reload), listing reverted to original 8 seed jobs (newly posted "QA Automation Specialist" gone), and employer dashboard showed only 5 active jobs / 3 applications (both newly submitted test applications and the new job vanished). State is purely in-memory, not persisted.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Submission date shown is off by one day (shows "Yesterday" for items created today)
    - Actual: Browser system date confirmed as Wed Aug 26 2026 (matching current date). Applications submitted moments earlier via the application form displayed "Yesterday" as their submission date in the employer dashboard instead of "Today". Likewise, a job posted moments earlier showed "Posted Yesterday" instead of "Posted Today" on both the job card and dashboard.