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
    - Issue: Job editing not implemented
    - Actual: No edit affordance exists anywhere. Employer dashboard job rows expose only two controls: the application-count toggle and a delete button (all icon buttons resolve to lucide-trash2 SVGs); there is no edit/pencil control, and job rows are not links. The job details page (/jobs/...) contains only "Back to all jobs" and the application form. Therefore a published job's details cannot be edited at all.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range and required-skills validation missing
    - Actual: Submitted a job with Minimum Salary 120000 > Maximum Salary 90000 and zero required skills. The form accepted it: redirected to dashboard with "Job posted successfully!" and the job appears as "QA Test Engineer ... Seattle, WA · $120000 - $90000/hr · Posted Yesterday". No error feedback was shown. (Empty-field validation for title/salary/description does work via HTML5 required.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: No duplicate-email guard per job
    - Actual: After applying to "Automation QA Analyst" as dana.whitfield@example.com, a second application with the identical email (name "Dana W Duplicate") was accepted with the same "Application submitted!" success toast. The dashboard application count for that job rose from 1 to 2 and Total Applications from 4 to 5.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on reload
    - Actual: State is held only in memory (localStorage/sessionStorage are empty). After reloading /employer/dashboard: (1) the published job "QA Test Engineer" disappeared and Active Jobs reverted 6→5; (2) the just-submitted application from Priya Raman for that job was lost, Total Applications reverted to the seed value 3; (3) a confirmed deletion of the seed job "Part-Time Content Writer" was undone — the job reappeared and Active Jobs went back to 5.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Off-by-one relative posting date for newly created records
    - Actual: Job "Automation QA Analyst" was published at 2026-08-28 03:09 (page's own new Date() = "Fri Aug 28 2026"), yet both the job card and the details page display "Posted Yesterday" instead of "Today". Same wrong label on the other just-created job "QA Test Engineer".