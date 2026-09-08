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
    - Issue: No job editing capability exists anywhere in the application
    - Actual: Employer dashboard job cards expose only two controls: an applications-count toggle (users icon) and a delete button (trash2 icon) — no edit/pencil control. The job details page offers only the application form. A DOM scan for any element matching /edit|pencil|update/ returned zero results, and the only hrefs in the app are /, /employer/dashboard and /employer/post-job. Navigating to /employer/edit-job/job-1 renders the "404 Oops! Page not found" page. Employers therefore cannot edit a published job.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job publishing validation missing: industry/type/location/skills not required, and salary min>max accepted
    - Actual: (1) Submitted with industry, employment type, location all unselected and no skills — job "QA Test Engineer" published successfully (toast "Job posted successfully!"), shown in dashboard as "· $90k - $120k" with blank industry/type badges. (2) Submitted "Automation QA Specialist" with Minimum Salary 150 and Maximum Salary 90 — accepted with no error; dashboard shows "Boston, MA · $150 - $90/hr". No inline errors or toast warnings appeared in either case.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with an already-used email for the same job is accepted
    - Actual: Applied to "Clinical Research Coordinator" as dana.whitfield@example.com, then submitted again with the same email (name "Dana W. Duplicate"). The second submission produced the same success toast "Application submitted!" with no duplicate warning. Employer dashboard now shows 2 applications for that job and Total Applications rose from 3 to 5.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all data is in-memory only and resets on page reload
    - Actual: Created jobs "QA Test Engineer" and "Automation QA Specialist" and an application from marcus.reid@example.com; after reloading, the dashboard reverted to the 5 seed jobs with Total Applications back to 3 — created jobs and the submitted application were gone. Deletion also does not persist: deleted seed job "Marketing Manager" (Active Jobs 5→4), and after reload it reappeared with Active Jobs back to 5. localStorage and sessionStorage are both empty.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly published job shows an incorrect posting date (off by one day)
    - Actual: Browser date confirmed as Thu Aug 27 2026 09:52 UTC. All three jobs created during this session immediately display "Yesterday" instead of "Today" — job card shows "Yesterday" and details page shows "Posted Yesterday" for Clinical Research Coordinator created seconds earlier. Dashboard likewise shows "Posted Yesterday".