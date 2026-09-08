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
    - Issue: No job-editing capability exists
    - Actual: The employer dashboard rows expose only an applications-count toggle and a trash/delete button — no edit control; the job details page offers only the apply form. App routes are exactly "/", "/jobs/:id", "/employer/dashboard", "/employer/post-job", "*" (from src/App.tsx), so no edit route/UI exists.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary-range ordering and required-skill rules not enforced
    - Actual: (1) A job was published with Minimum Salary 120000 > Maximum Salary 90000 — accepted with "Job posted successfully!" and displayed as "$120000 - $90000/hr". (2) A job ("No Skills Test Role", Sales/Full-time/Boston, MA, 50000-70000) was published with zero required skills — the skills input is not required and no error is shown. Text/number fields do use native required validation, but these two constraints are unenforced with no feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application for same job/email is accepted
    - Actual: After applying to job-1 as dana.whitfield@example.com, a second application to the same job with "DANA.Whitfield@example.com" was accepted with the same "Application submitted!" toast. The dashboard now shows "Applications (4)" for Senior Frontend Developer, containing both "Dana Whitfield / dana.whitfield@example.com" and "Dana W. Duplicate / DANA.Whitfield@example.com"; total applications rose 4→5.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence across page reload
    - Actual: A job "QA Automation Engineer" was published and appeared in both the employer dashboard (7 rows, 9 listings) and the job-seeker list. After reloading the app, the listing count returned to "Showing 8 jobs" and the published job was gone. localStorage is empty — all state is in-memory only, so published jobs (and therefore applications/deletions) do not survive a reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Submission dates are off by one day
    - Actual: Today is 2026-08-28. Jobs posted just now ("QA Automation Engineer", "Cloud Security Specialist") display "Posted Yesterday" in the dashboard and "Yesterday" on their job cards. Applications submitted seconds ago also display "Yesterday" in the dashboard application records. Expected "Today" / the current date.