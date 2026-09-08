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
    - Actual: The employer dashboard job cards expose only two controls: an applications-count toggle (users icon) and a delete button (trash2 icon) — there is no edit/pencil control. The job details page (/jobs/job-1) contains no edit link or button either (scan of all anchors/buttons for edit/update/manage/pencil returned none). Candidate routes /employer/edit-job/job-1 and /employer/jobs/job-1/edit both render the "404 — Oops! Page not found" page. Therefore an employer cannot edit a published job and no updates can propagate to the employer or job-seeker views.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Missing business validation: required-skill rule and salary min<=max rule are not enforced
    - Actual: Two violations: (1) "QA Automation Engineer" was published with the Required Skills list empty (field is not marked required and no error shown) — toast said "Job posted successfully!". (2) "Salary Validation Test Job" was published with Minimum Salary=150000 and Maximum Salary=50000; it was accepted with no error and now renders on the dashboard as "$150k - $50k". Only the browser's native HTML5 "Please fill out this field." validation blocks empty title/salary/description/selects.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application by same email for the same job is not prevented
    - Actual: After successfully applying to job-1 as alice.johnson@example.com, a second application to the same job with the identical email (name "Alice J Duplicate") was accepted and showed the same success toast. The dashboard badge for "Senior Frontend Developer" went 2 → 3 → 4 and the applications list now contains two separate records with alice.johnson@example.com. No duplicate warning was shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — state is in-memory only and resets on page reload
    - Actual: Two jobs published earlier ("QA Automation Engineer", "Salary Validation Test Job") raised the listing to "Showing 10 jobs". After reloading http://localhost:6003/ the listing returned to "Showing 8 jobs" containing only the seed jobs job-1..job-8; both published jobs were gone. localStorage is empty (no keys), confirming nothing is persisted.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative submission date is off by one day — items created today are labelled "Yesterday"
    - Actual: Current browser date is 2026-08-27T19:53Z. The job "Backend Platform Engineer" created at 2026-08-27T19:51Z (same calendar day, verified via its id timestamp) displays "Posted Yesterday" on both the dashboard and the job card. The application just submitted by Alice Johnson likewise displays "Yesterday" instead of "Today". Every newly created job showed the same wrong label ("QA Automation Engineer", "Salary Validation Test Job").