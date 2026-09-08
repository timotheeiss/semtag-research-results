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
    - Issue: No job editing capability exists
    - Actual: The employer dashboard offers only "Post New Job", an applications toggle and a delete button per job — no edit control; the job detail page has only "Back to all jobs" and "Submit Application". Probing /employer/edit/job-1 returns the app's 404 page ("Oops! Page not found"). Published jobs cannot be edited.


## Constraint
- [X] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: Skill requirement and min<=max salary rule are not enforced
    - Actual: Required text/select fields and negative salaries are blocked (native messages, e.g. "Value must be greater than or equal to 0"), but (1) a job with ZERO required skills was published successfully ("QA Automation Engineer" now in dashboard), and (2) a job with min salary 200000 > max salary 100000 was published with toast "Job posted successfully!" and no error.

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application with the same email for the same job is accepted
    - Actual: After applying to job-1 as bob.candidate@example.com, a second submission with the same email ("Bob Duplicate") showed the same success toast and was stored: job-1 now lists "Applications (4)" including both "Bob Duplicate / bob.candidate@example.com" and "Bob Candidate / bob.candidate@example.com". No duplicate error was shown. (Same behavior observed earlier on job-3.)


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all changes are lost on page reload
    - Actual: Before reload: 10 public jobs (3 newly posted), job-2 deleted, dashboard 7 jobs / 4 applications. After reloading http://localhost:7003/: back to the original 8 seed jobs (new jobs gone, deleted job-2 restored) and dashboard shows 5 jobs / 3 applications (submitted applications gone). localStorage is empty — state is in-memory only.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created jobs and applications show an off-by-one relative date ("Yesterday")
    - Actual: With the browser date Thu Aug 27 2026, jobs published just now display "Yesterday" (e.g. "Backend Platform Engineer ... Yesterday", dashboard "Posted Yesterday") and applications submitted just now also display "Yesterday" instead of today's date.