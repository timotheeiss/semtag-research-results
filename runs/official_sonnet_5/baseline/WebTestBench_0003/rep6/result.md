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
    - Actual: Inspected the employer dashboard job rows: each row only has an application-count toggle button and one icon button, which opens a "Delete this job?" confirmation dialog (confirmed via DOM inspection showing a trash2 icon and delete confirmation dialog). No edit button, pencil icon, or link to an edit form was found on the dashboard or on the job details page. There is no accessible way for an employer to edit a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job posted despite invalid salary range and missing required skills
    - Actual: Submitted the "Post a Job" form with title, industry, employment type, location, and description filled, minimum salary 100000, maximum salary 50000 (min > max), and no required skills added. The app accepted it, showed "Job posted successfully!", and the job "QA Test Sales Rep" appeared in both the employer dashboard and job-seeker listing displaying an invalid salary range "$100k - $50k" and no skills tags, instead of being rejected with validation feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email accepted for same job
    - Actual: Submitted a second application to job-1 using the same email (test.user@example.com) already used for a prior application to that job. The app accepted it, showed a second "Application submitted!" success toast, and the employer dashboard's application list for that job shows both "Test User" and "Duplicate Test" as two separate records with the identical email, increasing the application count from 2 to 4 (should have been rejected, leaving the existing application unchanged).


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: All app state is in-memory only; reload wipes published jobs, applications, and deletions
    - Actual: After posting two new jobs and submitting applications, navigating directly to a job URL (equivalent to a reload of that route) caused a full page reload that reset the entire app back to the original 8 seed jobs — the newly posted jobs, submitted applications, and the deleted "Marketing Manager" job's absence were all lost; the app showed "Job not found" for the newly created job and the homepage reverted to "Showing 8 jobs" with Marketing Manager restored. No localStorage or IndexedDB persistence was found (both empty), confirming state is kept only in memory.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly submitted/posted items show an incorrect relative date ("Yesterday" instead of "Today")
    - Actual: Submitted an application and posted new jobs at 2026-08-25 00:2x UTC (matching the browser's current date/time of Aug 25, 2026). The employer dashboard displayed the new application's submission date as "Yesterday" and the new job postings as "Posted Yesterday" instead of "Today", indicating an off-by-one error in the relative date calculation.