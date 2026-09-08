# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: No edit functionality for published job listings
    - Actual: Delete works: clicking the trash icon on a job listing opens a "Delete this job?" confirmation dialog, and confirming removes the job from the dashboard (verified by deleting the test-created "QA Automation Engineer" listing, which disappeared from "Your Job Listings"). However, no "Edit" button, link, or affordance exists anywhere on the Employer Dashboard or job listing cards to modify an already-published job's details (title, description, salary, etc.) - only a delete (trash) icon and an applications-count toggle are present.

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
    - Issue: No duplicate-application prevention, and applications are not persisted at all
    - Actual: Submitted two applications to job-1 with identical name/email ("Test User" / "testuser@example.com"); both were accepted with a success toast and no duplicate warning. Additionally, neither submission (nor a third with different info) was reflected in the employer dashboard's application list/count even after reload, so duplicate submissions are not blocked by contact information.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No authentication/authorization separating job seekers from employers
    - Actual: There is no login, signup, or role-based access control anywhere in the app. The "Post a Job" link and /employer/post-job page are freely accessible to any visitor without any employer authentication, and the form was successfully submitted (job "QA Automation Engineer" was published) with no identity check. Any job seeker visiting the site can post a job opening in the exact same way an employer would.


## Interaction
- [ ] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.
  - Bug Report:
    - Issue: Application submissions are not persisted to the backend
    - Actual: Submitting the application form always shows a success toast ("Application submitted!"), but after submitting 3 additional applications to job-1 (including a uniquely-named "QA Tester" applicant), the Employer Dashboard's application count for that job and the Total Applications stat remained unchanged (2 and 3 respectively) even after a full page reload. The new applications never appear in the dashboard's applications list, indicating the system does not actually receive/store the submitted applications despite the success confirmation.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.