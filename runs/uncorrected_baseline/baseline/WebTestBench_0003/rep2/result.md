# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: No edit functionality for job listings
    - Actual: Delete works correctly (confirmed via alertdialog "Delete this job?" then job count dropped 6→5 and listing removed). However, each job row on the Employer Dashboard only exposes two controls: an applications-count toggle and a delete (trash) button. There is no edit button, link, or menu option anywhere on the dashboard or job cards to modify an existing job's details.

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
    - Actual: Submitted a second application to the same job (job-1) using the identical email (test.applicant@example.com) and name as a prior successful submission. The system accepted it and displayed the same success toast "Application submitted!" again with no warning or rejection of the duplicate.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No access control preventing job seekers from posting jobs
    - Actual: The application has no login/authentication or role separation at all (no sign-in/login links found anywhere). The "Post a Job" nav link and /employer/post-job page are directly accessible to any visitor without any employer verification, so a job seeker can freely create job postings just like an employer.


## Interaction
- [X] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.