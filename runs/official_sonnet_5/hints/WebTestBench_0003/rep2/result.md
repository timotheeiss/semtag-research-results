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
    - Issue: No edit functionality exists for published jobs
    - Actual: Searched the employer dashboard job cards and job detail page for an edit control. Each dashboard job card only exposes an "applications" toggle button and a delete button (no data-semtag edit action, no icon/button with "edit" label/title anywhere in the DOM). Clicking the job title does not navigate to an edit form. There is no UI path to edit a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Invalid salary range (min > max) accepted without validation
    - Actual: Posted a job "QA Test Engineer" with Minimum Salary 90000 and Maximum Salary 60000 (min > max). The form submitted successfully with a "Job posted successfully!" toast and the job now appears in the dashboard as "Remote · $90k - $60k · Posted Yesterday" with no error or rejection for the invalid salary range.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email application not blocked
    - Actual: Submitted two applications for job-1 using the same email testuser@example.com (first with name "Test User", second with name "Test User Duplicate"). Both were accepted with a success toast and both appear as separate application records in the employer dashboard, instead of the second submission being rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data does not persist across page reload
    - Actual: After deleting job-6 ("Part-Time Content Writer") it was removed from both dashboard and job-seeker list. After reloading the page, job-6 reappeared in the job-seeker listing as if never deleted. Similarly, a newly posted job ("QA Test Engineer") and newly submitted applications (Test User, Test User Duplicate) disappeared after a page reload, reverting to only the original seed data. All in-memory changes (published jobs, submitted applications, job deletions) are lost on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Incorrect relative submission date shown
    - Actual: Current date is Aug 25, 2026. An application submitted moments ago on job-1 shows date "Yesterday" instead of "Today". A job posted moments ago ("QA Test Engineer") shows "Posted Yesterday" instead of "Posted Today". Both dates are inaccurate relative to the current date.