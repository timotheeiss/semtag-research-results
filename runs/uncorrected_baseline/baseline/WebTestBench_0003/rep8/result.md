# Test Result

## Functionality
- [X] FT-1: Employers can create and publish job listings, fully entering core information such as name and description.

- [ ] FT-2: Employers can edit and delete published jobs to manage their positions.
  - Bug Report:
    - Issue: No edit functionality for published jobs
    - Actual: Delete works correctly (confirmation dialog, then job removed and Active Jobs count decremented from 6 to 5). However, each job card on the employer dashboard only exposes two controls: a "view applications" (users icon) button and a "delete" (trash icon) button — there is no edit/pencil icon, no edit link, and clicking the job title does not navigate to an edit form. Inspecting all dashboard buttons via DOM confirmed only "lucide-users" and "lucide-trash2" icons exist; no edit capability is present anywhere in the app.

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
    - Issue: Incomplete job posting was published without required fields
    - Actual: Submitted a job with only Title, Description, Min/Max Salary filled — Industry, Employment Type, and Location dropdowns were left at their default unselected placeholder state. Instead of being blocked, the job was published successfully ("Job posted successfully! Your job listing is now live and visible to job seekers.") and now appears in the dashboard as "Test Job" with missing location/industry/type info (listing shows "· $50k - $70k · Posted Yesterday" with no location, no industry/type tags).

- [ ] CS-11: Job seekers are not allowed to submit multiple applications for the same position using the same contact information.
  - Bug Report:
    - Issue: Duplicate application not blocked
    - Actual: Submitted a second application to the same job (job-1) using the identical name "Jane Test" and email "jane.test@example.com" as a prior successful submission. The system accepted it and displayed "Application submitted! Your application for Senior Frontend Developer at TechCorp Solutions has been sent." with no warning or rejection of the duplicate.

- [ ] CS-12: Job seekers cannot post job openings.
  - Bug Report:
    - Issue: No access control preventing job seekers from posting jobs
    - Actual: The application has no authentication/login system distinguishing employers from job seekers. The "Post a Job" link is visible and freely clickable in the main navigation bar on every page (homepage, job listing pages, etc.) for any anonymous visitor, and navigating to /employer/post-job succeeds without any credential check. Any job seeker can therefore create job postings exactly like an employer, as demonstrated earlier when FT-1 was tested without any login step.


## Interaction
- [X] IX-13: After job seekers submit their application forms, the system receives them normally without any submission failures.

- [X] IX-14: After switching the filtering and search criteria, the job list updates the matching results in real time.


## Content
- [X] CT-15: The job listings clearly display key information such as job title, location, and salary range.

- [X] CT-16: The job details page fully displays all information, including the job description and required skills.

- [X] CT-17: Employers can view a complete list of applications, clearly distinguishing between different job seekers.