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
    - Issue: No edit functionality found for published jobs
    - Actual: Searched the employer dashboard (job rows only expose an applications-toggle button and a delete button) and each job's detail page (no owner/edit affordance) - no Edit control or route exists anywhere in the UI to modify a published job's details.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Job published despite minimum salary greater than maximum salary; no validation feedback shown
    - Actual: Filled Post a Job form with Minimum Salary=100000 and Maximum Salary=50000 (min > max) plus all other required fields, and clicked Post Job. The job was accepted and published: toast said 'Job posted successfully!' and the job appeared in the employer dashboard as 'QA Test Engineer ... $100k - $50k'. No error/validation message was shown for the invalid salary range.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with same email not blocked
    - Actual: Submitted two applications for job-1 using the same email (alice.johnson@example.com) with different names/messages. Both were accepted; app shows 'Application submitted!' both times, and the employer dashboard lists both as separate distinct application records (Applications count went from 2 to 4). No error or rejection was shown for the second submission.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Application state (published jobs, applications, deletions) not persisted across page reload
    - Actual: All data is held only in-memory client-side (no API calls observed, localStorage empty). After deleting 'Part-Time Content Writer' (job-6) and reloading the home page, the job reappeared and the listing count reverted from 7 to 8. Similarly, earlier a newly published job and newly submitted applications disappeared entirely after any full page navigation/reload, reverting the employer dashboard counts back to their seed values (5 active jobs / 3 applications).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created job/application dates show incorrect relative date
    - Actual: Current date is 2026-08-25 (verified via browser Date() = Tue Aug 25 2026). A job posted just now shows 'Posted Yesterday' instead of 'Posted Today', and an application submitted just now shows submission date 'Yesterday' instead of 'Today', indicating an off-by-one-day error in the relative date calculation.