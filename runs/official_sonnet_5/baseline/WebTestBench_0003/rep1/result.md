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
    - Issue: No edit functionality available for published jobs
    - Actual: Employer dashboard job listing rows only expose two controls per job: an applications-count button and a delete (trash) icon button. No edit/pencil control exists, clicking the job title does not navigate to an edit form, and no other UI path to modify a published job's details (title, salary, description, skills, etc.) was found.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Invalid salary range and missing required skill accepted without validation
    - Actual: Posted job "QA Automation Engineer" with Minimum Salary 150000 > Maximum Salary 100000 and zero required skills added. System showed "Job posted successfully!" and created the listing displaying "$150k - $100k" with no skills, instead of rejecting the invalid min>max salary and missing skill with clear feedback.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email not blocked
    - Actual: Submitted a second application for job-1 using the same email (test.applicant@example.com) already used for that job; the system accepted it and showed "Application submitted!" success toast again. Employer dashboard confirms two separate application records with the same email for job-1 (Applications count went from prior value to 4, including both duplicate entries).


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: State not persisted across page reload
    - Actual: After reloading http://localhost:6003/, all changes made during the session were lost: the newly published "Cloud Support Specialist" job disappeared (job count reverted from 9 to 8 on homepage), the employer dashboard reverted from 6 Active Jobs/5 Total Applications to the original 5 Active Jobs/3 Total Applications, and the 2 test applications submitted for "Senior Frontend Developer" were gone (count reverted from 4 to the original 2). The prior job deletion also could not be re-verified as persisted since the deleted job's state simply reset along with everything else (data is held in memory only, not persisted server-side).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: New job/application dates show incorrect relative day
    - Actual: Current date is 2026-08-24 (Today). Immediately after posting "Cloud Support Specialist" and "QA Automation Engineer" jobs, both listings displayed "Posted Yesterday" instead of "Posted Today" (seen on homepage card, job listing, and employer dashboard). Likewise, two applications submitted for job-1 moments after creation showed submission date "Yesterday" instead of "Today" in the employer dashboard applications panel. This indicates a timezone/date-calculation bug producing an inaccurate relative date for newly created records.