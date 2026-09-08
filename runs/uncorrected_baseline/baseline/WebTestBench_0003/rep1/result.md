# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: No edit functionality for job listings
    - Actual: Delete works correctly (confirmed via dialog, job removed and Active Jobs count decremented from 6 to 5). However, no edit control exists anywhere on the job listing card, dashboard, or job title — only an applications-count button and a delete (trash) button are present. Job titles are not links and no edit icon/menu was found in the DOM.

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
    - Issue: Duplicate application not prevented
    - Actual: Submitted a second application to the same job (job-1) using identical name (Test User) and email (test.user@example.com). The system accepted it and displayed the same "Application submitted!" success toast with no warning or rejection of the duplicate.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No access control restricting job posting to employers only
    - Actual: The "Post a Job" link and /employer/post-job page are accessible to anyone browsing the site without any login, authentication, or role check. There is no job-seeker vs employer account distinction anywhere in the app, so any visitor (including job seekers) can freely create job postings.


## Interaction
- [X] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.