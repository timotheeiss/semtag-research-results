# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: No edit functionality for published jobs
    - Actual: Delete works (trash icon on dashboard opens confirmation dialog "Delete this job?" and removes the job, decrementing Active Jobs count from 6 to 5). However, no edit/update capability exists anywhere: the job listing rows in the employer dashboard only expose an applications-count button and a delete button, clicking the job title does nothing, and the public job detail page (/jobs/job-2) has no edit option either.

- [X] FT-3: The employer dashboard allows you to view all the job listings you have posted.

- [X] FT-4: The employer dashboard allows you to view a list of applications submitted by job seekers for all the positions you have posted.

- [X] FT-5: Job seekers can browse the platform's full list of job openings.

- [X] FT-6: The website allows users to filter job listings by criteria such as industry, location, and job type.

- [X] FT-7: The website supports keyword search, which allows users to retrieve job information.

- [X] FT-8: Job seekers can submit an application form that includes contact information and a message on the job details page.


## Constraint
- [X] CS-9: Job seekers must not submit job applications with missing or incorrect personal information.

- [ ] CS-10: Job postings with incomplete information cannot be published; there is a clear notification.
  - Bug Report:
    - Issue: Incomplete job posting was published without validation error
    - Actual: Submitted the "Post a Job" form leaving Industry, Employment Type, and Location (all marked required with *) unselected, only filling Title, Salary range, and Description. The form was accepted with a success toast "Job posted successfully!" and the job appeared live in the dashboard/listings with no industry/type/location tag and no error notification, instead of being blocked with a clear notification.

- [ ] CS-11: Job seekers are not allowed to submit multiple applications for the same position using the same contact information.
  - Bug Report:
    - Issue: Duplicate application not blocked
    - Actual: Submitted a second application to the same job (job-1) using the identical email (test.applicant@example.com) and name. The system accepted it and displayed the same "Application submitted!" success toast with no duplicate warning or rejection.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No authentication/role restriction prevents job seekers from posting jobs
    - Actual: The app has no login or role-based access control. The "Post a Job" link and "Employer" dashboard link are visible in the global navigation on every page (including the public job-browsing homepage) with no sign-in gate. Throughout testing, a job was successfully created, viewed in the dashboard, and deleted without ever authenticating as an employer — meaning any job seeker/visitor can freely post job openings.


## Interaction
- [X] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.