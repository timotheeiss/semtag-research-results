# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: Missing edit functionality for job listings
    - Actual: On the Employer Dashboard, each job listing card only has two controls: an applications-count toggle and a delete (trash) icon. Delete works correctly (confirmed via confirmation dialog and job removed from list, Active Jobs count decremented from 6 to 5). However, there is no edit/update button or link anywhere on the job card, and clicking the job title does not navigate to an edit page. Employers cannot edit a published job listing's details.

- [X] FT-3: The employer dashboard allows you to view all the job listings you have posted.

- [X] FT-4: The employer dashboard allows you to view a list of applications submitted by job seekers for all the positions you have posted.

- [X] FT-5: Job seekers can browse the platform's full list of job openings.

- [X] FT-6: The website allows users to filter job listings by criteria such as industry, location, and job type.

- [X] FT-7: The website supports keyword search, which allows users to retrieve job information.

- [X] FT-8: Job seekers can submit an application form that includes contact information and a message on the job details page.


## Constraint
- [X] CS-9: Job seekers must not submit job applications with missing or incorrect personal information.

- [X] CS-10: Job postings with incomplete information cannot be published; there is a clear notification.

- [ ] CS-11: Job seekers are not allowed to submit multiple applications for the same position using the same contact information.
  - Bug Report:
    - Issue: Duplicate application not blocked
    - Actual: Submitted a second application to the same job (job-5) using the identical email address (test.user@example.com) as a prior successful application. The system accepted it and displayed the same success toast "Application submitted!" with no warning or rejection of the duplicate.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No access control preventing job seekers from posting jobs
    - Actual: The application has no login/authentication system distinguishing employers from job seekers. The "Post a Job" link and /employer/post-job page are freely accessible in the main navigation to any visitor without any sign-in or role verification, and a job posting was successfully created earlier without any employer credentials. Any job seeker can post a job opening.


## Interaction
- [X] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.