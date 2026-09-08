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
    - Issue: No edit functionality available for published jobs
    - Actual: Explored the employer dashboard job listing rows (only two controls per job: an applications-count toggle and a delete button that opens a 'Delete this job?' confirmation) and the public job details page (only shows job info + apply form). No edit/pencil button, no clickable link to an edit form, and no employer-only edit affordance was found anywhere in the UI, so a published job's details cannot be edited.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Invalid job data accepted (min salary > max salary, no required skill)
    - Actual: Posted a job titled 'QA Test Engineer' with Minimum Salary=150000 and Maximum Salary=100000 (min > max) and with zero required skills added. The form accepted it and showed 'Job posted successfully! Your job listing is now live and visible to job seekers.' The new listing appears in the dashboard as 'Austin, TX · $150k - $100k · Posted Yesterday' with no skill tags, instead of being rejected with validation feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email application accepted
    - Actual: Submitted a second application to job-1 using the same email (alice.johnson@example.com) already used for that job. App showed success toast 'Application submitted!' and the employer dashboard's Applications(4) list for Senior Frontend Developer now shows two separate entries ('Alice Johnson' and 'Alice Johnson Duplicate') both with email alice.johnson@example.com, instead of rejecting the duplicate or leaving the original unchanged.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data does not persist across page reload
    - Actual: Before reload: 9 jobs existed (8 original + 1 new 'Backend Software Engineer'; 'QA Test Engineer' had been deleted), and Senior Frontend Developer had 4 applications (2 submitted during this test session). After navigating to http://localhost:6003/ (reload), the homepage reverted to 'Showing 8 jobs' (original seed set only, new job gone), and the employer dashboard reverted to Active Jobs=5, Total Applications=3 (back to the original seed applications, both test-submitted applications and the new job/deletion were lost). Published jobs, submitted applications, and job deletions do not persist after reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created job/application dates show as 'Yesterday' instead of 'Today'
    - Actual: Browser clock confirmed as Tue Aug 25 2026 08:06 UTC (matches current date). Job 'Backend Software Engineer' was published at this moment but its details page and dashboard listing show 'Posted Yesterday' instead of 'Today'. Similarly, applications submitted moments earlier (Alice Johnson entries) show submission date as 'Yesterday' in the employer dashboard instead of 'Today'. This is an off-by-one-day inaccuracy in the relative date display.