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
    - Issue: No edit functionality available
    - Actual: Employer Dashboard job rows only expose an application-count toggle button and a delete (trash) button; there is no edit/pencil button, link, or any other UI path to modify a published job's details. Job titles are plain (non-clickable) headings. Editing a published job is not possible through the UI.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Server/client does not enforce required-skill and salary min≤max constraints
    - Actual: Posted "Test No Skill Job" with zero required skills — it was accepted ("Job posted successfully!") and appears in the dashboard with no skills. Posted "Test Bad Salary Job" with Minimum Salary $90,000 and Maximum Salary $50,000 (min > max) — it was also accepted and now shows "$90k - $50k" in the dashboard. Only the non-negative salary rule and title/description/industry/type/location required-field rules are enforced (via native HTML input validation); the skill-required and min≤max rules are not enforced and produce no feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application not blocked
    - Actual: Submitted a second application to "QA Automation Engineer" using the same email (jane.seeker@example.com) already used for that job. The app displayed the same "Application submitted!" success toast and created a second application record — the job's application count increased from 1 to 2 in the employer dashboard, instead of being rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No data persistence across reload
    - Actual: After reloading http://localhost:6003/, the job list reverted to the original 8 seed jobs. The two jobs created during this session ("QA Automation Engineer", "Test No Skill Job") and their applications disappeared entirely, and the site is back to its pristine seeded state (no server/localStorage persistence of created jobs, applications, or deletions).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Relative date off by one day
    - Actual: Browser/system date confirmed as Mon Aug 24 2026 (current date). A job posted seconds ago displays "Posted Yesterday" instead of "Posted Today", and an application submitted seconds ago shows "Yesterday" instead of "Today" in the employer dashboard.