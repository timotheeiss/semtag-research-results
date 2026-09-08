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
    - Actual: Inspected the employer dashboard job listing controls: each job row only exposes a 'View applications' (users icon) button and a 'Delete' (trash icon) button — confirmed via DOM inspection (button classes/icons: lucide-users and lucide-trash2 only). Clicking the job title/card does not open an edit form. No edit/pencil control or edit route is exposed anywhere in the employer dashboard or job details page, so employers cannot modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job posted despite invalid salary range and missing required skill
    - Actual: Submitted the Post a Job form with Minimum Salary=150000 > Maximum Salary=90000 and with zero required skills added (title, industry, type, location, description were filled). The app accepted it, showed 'Job posted successfully!' and created a listing 'QA Test Engineer' displaying '$150k - $90k' with no skills tags, instead of rejecting the invalid min>max salary and missing-skill submission with clear feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email not blocked
    - Actual: Submitted a second application for job-1 using the same email (test.user@example.com) already used for that job. The app accepted it, showed 'Application submitted!' success toast, and both 'Test User' and 'Test User Duplicate' now appear as two separate application records under job-1 in the employer dashboard (application count went from 3 to 4, not blocked or deduplicated).


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data does not persist across page reload
    - Actual: Before reload: 9 jobs existed (8 seed + 'Backend Engineer'), job-1 had 4 applications (2 seed + 2 test submissions), and 'QA Test Engineer' had been deleted. After navigating to http://localhost:6003/ again (reload), the listing reverted to exactly the original 8 seed jobs — 'Backend Engineer' job is gone, meaning published jobs, submitted applications, and deletions are not retained; state is held only in-memory/client-side and resets on reload.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created job/application dates show incorrect relative date
    - Actual: Browser system clock confirmed as Aug 25 2026 (current date). A job posted at that moment shows 'Posted Yesterday' instead of 'Posted Today', and applications submitted moments earlier show 'Yesterday' instead of 'Today', indicating the relative-date computation is off by one day relative to the true current date.