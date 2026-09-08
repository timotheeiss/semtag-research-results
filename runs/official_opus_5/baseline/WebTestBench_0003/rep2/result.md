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
    - Issue: Job editing is not implemented
    - Actual: No edit affordance exists anywhere: employer dashboard job cards only expose an applicants toggle and a trash/delete button; the job details page has no edit control. App routes are only "/", "/jobs/:id", "/employer/dashboard", "/employer/post-job" and "*" — there is no edit page, so a published job's details cannot be updated.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range validation missing (min > max accepted)
    - Actual: Posted "QA Test Engineer" with Minimum Salary=200000 and Maximum Salary=100000. No error shown; toast "Job posted successfully!" appeared and dashboard now lists it as "Austin, TX · $200k - $100k". Required text fields are enforced via HTML5 required, but no cross-field salary check and skills are not required (field labeled "Required Skills" without *).

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with an already-used email is accepted
    - Actual: After applying to "QA Test Engineer" as alice@example.com, a second submission with the same email (name "Alice Duplicate") was accepted with the same "Application submitted!" success toast. Dashboard application count for that job went from 2 to 3, i.e. a duplicate record was created instead of being rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state resets to seed data on reload
    - Actual: Published job "Persistence Check Engineer" (Finance/Contract/Chicago) and application "Persist Applicant / persist@example.com" for Marketing Manager both disappeared after reloading; dashboard reverted to 5 active jobs / 3 total applications. A deleted job ("Part-Time Content Writer", active jobs 5→4) reappeared after reload (back to 5). localStorage and sessionStorage are both empty — state is only in memory.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly published job shows an inaccurate posting date
    - Actual: Job created just now (current date 2026-08-27) is displayed as "Yesterday" on the job card, and "Posted Yesterday" on the details page and employer dashboard, instead of "Today"/2026-08-27.