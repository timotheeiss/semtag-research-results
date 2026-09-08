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
    - Actual: Inspected the employer dashboard's job listing rows (DOM query for all dashboard.jobs.item.* elements): each job row exposes only two controls — an "applications" toggle button and a "delete" button. There is no edit/pencil button, no clickable link to an edit form, and no aria-label/title indicating an edit action anywhere on the dashboard. The Post a Job form only supports creating new listings (submits via action "post-job"); there is no equivalent edit form or route reachable from the UI. Employers therefore have no way to modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Salary range min>max not validated
    - Actual: Submitted a job posting with all required fields filled (title, industry, type, location, description, 1 skill) but Minimum Salary=100000 and Maximum Salary=50000 (min > max). The form accepted it without any error message, showed "Job posted successfully!" toast, and the job now appears in the dashboard/listings displaying an illogical salary range "$100k - $50k". Missing-field validation (native required-field checks) does work correctly, but the min<=max salary constraint is not enforced at all.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application email not blocked
    - Actual: Submitted two separate applications to job-1 (Senior Frontend Developer) using the identical email "dupcheck@test.com" with different names/messages. Both submissions succeeded with a success toast each time, and the employer dashboard shows job-1's application count increased from 2 to 4 (both duplicate-email applications were saved as separate records). No error or rejection was shown for the second, duplicate-email submission.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence of app state across reload
    - Actual: After publishing job "QA Automation Engineer" and submitting an application to it, reloading the page (navigating to http://localhost:6003/) caused the job listing count to revert from 9 to 8 and the new job disappeared entirely from the job-seeker listing; the submitted application was lost as well. Data is stored only in-memory client-side and is not persisted (no backend/localStorage), so published jobs and applications do not survive a reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Incorrect relative date display for newly created records
    - Actual: Job posted at current session time (current date 2026-08-25) shows "Posted Yesterday" on both job card and job detail page instead of "Today"/"just now". The application submitted moments later also shows submission date as "Yesterday" in the employer dashboard instead of today's date. This indicates a date/timezone off-by-one bug.