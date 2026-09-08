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
    - Issue: Job editing feature is not accessible or not implemented. No edit button found in employer dashboard, and navigating to /employer/edit-job/{id} returns 404.
    - Actual: Searched for edit functionality in employer dashboard - only delete button found. Attempted to navigate to /employer/edit-job/job-1 which returned 404 error. No edit form or edit interface is accessible.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: System did not validate salary range constraint. Allowed job posting with minimum salary ($100k) greater than maximum salary ($50k).
    - Actual: Submitted job with min=$100k, max=$50k and received "Job posted successfully!" message. Job appears in dashboard showing "$100k - $50k" range. No validation error or feedback was provided.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: System allowed duplicate application submission using same email for same job.
    - Actual: Second application with jane.smith@example.com for job-2 was submitted and received success confirmation, rather than being rejected.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: Data does not persist consistently after page reload. Newly created jobs disappear from employer dashboard.
    - Actual: Before reload: Employer dashboard showed 6 active jobs including "Senior QA Engineer" posted yesterday. After reload: Employer dashboard shows only 5 active jobs and "Senior QA Engineer" is missing. Application counts also changed (Senior Frontend Developer: 3→2, Product Designer: 3→1).


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [X] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.