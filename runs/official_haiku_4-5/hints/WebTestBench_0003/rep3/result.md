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
    - Issue: Edit functionality not implemented for jobs
    - Actual: No edit button visible in employer dashboard. Direct navigation to /employer/edit-job/{id} returns 404 Page not found. Alternative URL patterns also unavailable. Only delete action available for job management.


## Constraint
- [ ] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.
  - Bug Report:
    - Issue: No validation feedback displayed for missing or invalid fields
    - Actual: Submitted form with all fields empty - no error message displayed and form remained on the page. Submitted with invalid email "invalid-email" - no validation error shown. No visible error messages, field highlighting, or user identification of invalid fields.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: No validation feedback for missing required fields or invalid salary data
    - Actual: Attempted to submit job with empty form - no error messages displayed. Submitted with title only and invalid salaries (min=150000, max=100000, violating min≤max rule) - no validation error shown. Form submission is prevented but user receives no feedback about what's invalid.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate email constraint not enforced - second application with same email overwrites or replaces the first
    - Actual: Job-2 (Product Designer) originally had John Doe's application (john.doe@example.com). Submitted second application as "Another Person" with same email. Success message was shown. Applications list now shows "Another Person" with the duplicate email, but original John Doe application is missing. Job-2 still shows 2 applications total (count unchanged), suggesting original application was replaced rather than rejection of duplicate.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [X] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [X] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.