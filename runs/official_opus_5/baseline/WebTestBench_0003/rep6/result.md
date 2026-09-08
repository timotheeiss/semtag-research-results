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
    - Issue: No job editing capability exists anywhere in the app
    - Actual: Employer dashboard job rows expose only two buttons (applications count with users icon and a trash/delete icon) — no edit control and no edit links. The job details page has no edit affordance either. Probing /employer/edit-job/job-1 returns the 404 "Oops! Page not found" page. Published job details cannot be modified.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: No validation of salary range ordering or required skills; invalid job is published without feedback
    - Actual: Posted "QA Automation Engineer" with minimum salary 150 and maximum 90 (min > max) and zero required skills. The form submitted successfully, redirected to /employer/dashboard, and the job appears as "Boston, MA · $150 - $90/hr". No error message or toast was shown. (Empty title/description/salary are blocked by native required validation, but the min≤max and at-least-one-skill rules are not enforced.)

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with an already-used email is accepted for the same job
    - Actual: After submitting as alice.tester@example.com on job-1, a second submission with the same email (name "Alice Duplicate") also showed "Application submitted!" and the employer dashboard now lists 4 applications for job-1 including two records with alice.tester@example.com. No duplicate-email error was shown.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — created jobs and submitted applications are lost on page reload
    - Actual: After posting job "QA Automation Engineer" and submitting 2 applications (dashboard showed 6 Active Jobs / 5 Total Applications), reloading the page reset the dashboard to the seed state: 5 Active Jobs / 3 Total Applications, no "QA Automation Engineer" job and no alice.tester@example.com applications. localStorage is empty; state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created records show a date one day in the past instead of today
    - Actual: Browser date is Thu Aug 27 2026 (UTC). A job posted seconds earlier displays "Posted Yesterday" on the dashboard, and applications submitted seconds earlier display "Yesterday" in the application list instead of "Today"/Aug 27.