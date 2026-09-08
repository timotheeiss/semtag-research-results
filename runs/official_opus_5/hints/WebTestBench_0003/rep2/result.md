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
    - Issue: No edit capability for published jobs
    - Actual: Employer dashboard job rows expose only an applications toggle and a delete button (no edit link); job detail page contains no "Edit" control (body text has no "edit"); /employer/edit/job-1 renders the SPA fallback with no edit form. Editing a published job is impossible.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job published with missing industry/employment type/location/skills and with minimum salary greater than maximum
    - Actual: Submitted "QA Automation Engineer" with industry/type/location left unselected, zero skills, salary min 120000 > max 90000; no error was shown, the app navigated to /employer/dashboard and created job-1787834302888. (Only empty required text fields and negative salary are blocked, via native HTML validation.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email for same job is accepted
    - Actual: After applying to job-1 as test.applicant@example.com, a second submission with the same email ("Different Name", "Second attempt message.") showed the same success message and created a second record; employer dashboard for job-1 now lists both app-1787834210137 and app-1787834223098 with email test.applicant@example.com (count went 3→4).


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — state resets to seed data on page reload
    - Actual: After reloading http://localhost:7003/, the job list shows "Showing 8 jobs" and the two jobs published this session (job-1787834360187, job-1787834302888) are gone; employer dashboard reverted to 5 jobs / 3 applications, losing the applications submitted to job-1 (was 4, now 2).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created job and newly submitted application show a posting/submission date of "Yesterday" instead of today
    - Actual: Browser clock is Thu Aug 27 2026 12:39; job created seconds earlier renders "Posted Yesterday" on the dashboard and "Yesterday" on the job card, and the application just submitted to job-1 renders "Yesterday" in the employer dashboard.