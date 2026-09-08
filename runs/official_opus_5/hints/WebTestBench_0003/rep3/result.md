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
    - Issue: No job editing capability exists
    - Actual: Employer dashboard job rows expose only two controls — toggle-applications and delete-job (no edit button/link); the string "edit" does not appear anywhere in the dashboard HTML, and the job detail page offers only "Back to all jobs" plus the application form. There is no route or UI to modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range validation missing — minimum greater than maximum is accepted
    - Actual: Required-field validation works (empty submit blocked with "Please fill out this field." on title/salary/description), but a job submitted with Minimum Salary 150000 and Maximum Salary 90000 was published successfully ("Job posted successfully!"). It now appears as job-1787843551813 with the inverted salary range "$150k - $90k" in the employer dashboard, with no error feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with an already-used email is accepted for the same job
    - Actual: After successfully applying to job-1 as alice@example.com, a second submission for the same job with the same email ("Alice Duplicate") was accepted: the same "Application submitted!" confirmation appeared and the employer dashboard now lists two separate applications under Senior Frontend Developer both with email alice@example.com (app-1787843407298 "Alice Tester" and app-1787843423266 "Alice Duplicate"). No duplicate-email error was shown and the job's application count rose from 3 to 4.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all data is in-memory and resets on page reload
    - Actual: After reloading http://localhost:7003/: the seeker listing returned to "Showing 8 jobs" (was 9) — both newly published jobs (QA Automation Engineer, Backend Engineer) disappeared, and the deleted job "Senior Frontend Developer" (job-1) reappeared. Dashboard reverted to Active Jobs 5 / Total Applications 3 with job-1 showing its 2 seed applications; the applications submitted during the session were gone. localStorage is empty (no storage keys), confirming state is not persisted.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created jobs and applications show an incorrect (off-by-one) submission date
    - Actual: Browser current date is Thu Aug 27 2026. An application submitted seconds earlier displays "Yesterday" instead of today, and a job posted seconds earlier (job-1787843551813, QA Automation Engineer) displays "Posted Yesterday" instead of today's date.