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
    - Actual: Searched the employer dashboard job cards (only 'Applications' toggle and 'Delete' buttons present, no edit control), the job title heading (not clickable/no edit link), and scanned all data-semtag-id attributes on the dashboard page for any 'edit' related element — none found. There is no UI affordance anywhere to edit a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job publish form accepts invalid salary range and missing required skill
    - Actual: Filled title, industry, employment type, location, description but set Minimum Salary=150000 > Maximum Salary=100000 and added zero required skills, then clicked Post Job. The job was published successfully ('Job posted successfully!' toast) and now appears in the dashboard as 'QA Test Engineer' showing 'Remote · $150k - $100k · Posted Yesterday' with 0 skills — no validation error was shown for min>max salary or for missing required skill.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email not blocked
    - Actual: Submitted a second application to job-1 using the same email (test.user@example.com) already used for that job. The app accepted it, showed 'Application submitted!' success toast, and the employer dashboard now lists both 'Test User' and 'Duplicate User' as separate applications with the same email under job-1 (Applications count rose from 3 to 4). No error indicating the email was already used for this job.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Application/job state does not persist across page reload
    - Actual: Before reload: published a new job 'Backend Software Engineer' (6 active jobs total) and previously submitted 2 test applications to job-1 (bringing job-1 to 4 applications, total applications=5). After reloading the page (full navigation to the dashboard URL), the app reverted to its default seed state: only the original 5 jobs remained ('Backend Software Engineer' was gone), and job-1's applications reverted to only the original 2 (Alex Johnson, Maria Garcia) — my submitted 'Test User' and 'Duplicate User' applications were lost. Total applications shown = 3 (default), not reflecting any of the session's changes.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative submission date is off by one day for newly created items
    - Actual: Submitted a new application ('CT20 Tester') to job-1 just now (current date 2026-08-25, confirmed via page's Date.now() matching). The employer dashboard displays its submission date as 'Yesterday' instead of 'Today', indicating the relative date calculation is inaccurate by one day. The same 'Yesterday' mislabeling was also observed earlier for a newly published job's 'Posted' date.