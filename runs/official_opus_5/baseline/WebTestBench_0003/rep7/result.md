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
    - Issue: Job editing is not implemented — no edit affordance or route exists anywhere in the app
    - Actual: Employer dashboard rows expose only two actions per job: a "view applications" button (users icon) and a delete button (trash2 icon). A DOM scan of all anchors and buttons found zero elements matching edit/update/modify, and the only hrefs present are /, /employer/dashboard and /employer/post-job. The job details page (/jobs/job-1) also offers no edit control - only the applicant apply form. Probing /employer/edit-job/job-1 returns the app's "404 Oops! Page not found" screen. An employer therefore cannot change a published job's details.


## Constraint
- [ ] CS-9: A job application cannot be submitted when the required name email or message is missing or when the email syntax is invalid; the invalid field is identified to the user.
  - Bug Report:
    - Issue: Invalid application submissions are blocked silently — no error message or field-level identification
    - Actual: Three invalid attempts on /jobs/job-1787877622714: (a) fully empty form, (b) name+message present with email "not-an-email", (c) name+valid email with empty message. All three were blocked (no success confirmation), but in every case there was no toast, no inline error text, and zero elements with aria-invalid="true" — the user is given no indication of which field is wrong or that anything happened at all.

- [ ] CS-10: A job cannot be published until its title, industry, employment type, location, description, salary range, and at least one required skill are provided; the salary values must be non-negative with the minimum no greater than the maximum, and invalid data produces clear feedback.
  - Bug Report:
    - Issue: No job-posting validation: invalid salary range and missing skills accepted; empty form gives no feedback
    - Actual: 1) Clicking "Post Job" on a completely empty form produced no error message, toast, or aria-invalid marking anywhere on the page (silently did nothing). 2) Submitting title="QA Test Engineer", industry=Technology, type=Full-time, location=Austin TX, min salary=200000, max salary=100000 (min > max) and ZERO required skills was ACCEPTED: toast "Job posted successfully!" and the job appears in dashboard as "QA Test Engineer ... Austin, TX · $200k - $100k".

- [ ] CS-11: A job seeker cannot submit another application for the same job using an email address already used for that job; the existing application remains unchanged.
  - Bug Report:
    - Issue: Duplicate application for the same job with an already-used email is accepted; no de-duplication
    - Actual: After applying to "Cloud Security Architect" as amara.okonkwo@example.com, a second application was submitted for the SAME job with the SAME email (different name "Amara O. Duplicate", phone 555-9999). It was accepted with the same success toast "Application submitted!". Employer dashboard application count for that job went 1 -> 2 and Total Applications 4 -> 5.


## Interaction
- [X] IX-13: A successfully submitted application produces visible confirmation and appears exactly once under the matching job in the employer dashboard with the submitted information.

- [X] IX-14: Changing search or filter criteria immediately refreshes the matching jobs and result count including the no-results state; clearing all criteria restores the full list.

- [ ] IX-19: Published jobs, submitted applications, and job deletions remain in effect after the page is reloaded.
  - Bug Report:
    - Issue: No persistence — all state is in-memory only and resets to seed data on page reload
    - Actual: All three mutations were lost on reload. (1) Published job: posted "Persistence Test Analyst" (Finance/Full-time/Chicago IL/$80k-$110k/Excel); dashboard showed 6 Active Jobs; after reloading /employer/dashboard the job is gone and Active Jobs is back to 5. (2) Submitted application: applied to Marketing Manager as reload.tester@example.com with success toast; after reload Marketing Manager shows 0 applications and Total Applications is back to 3. (3) Deletion: deleted seeded job "DevOps Engineer (Contract)" via confirm dialog and it disappeared; after reload it is present again in the dashboard. localStorage and sessionStorage are both empty (0 keys), confirming nothing is persisted.


## Content
- [X] CT-15: Every job card displays the job title, company, industry, employment type, location, salary range, posting date, and required skills.

- [X] CT-16: A job details page displays information matching the selected listing: title, company, industry, employment type, location, salary range, posting date, full description, and required skills.

- [X] CT-17: For each application, the employer can view the applicant's name, email address, optional phone number, message, and submission date as a distinct record.

- [ ] CT-20: Newly published jobs and newly submitted applications display an accurate submission date relative to the user's current date.
  - Bug Report:
    - Issue: Newly created jobs and applications display a date one day in the past ("Yesterday") instead of today
    - Actual: Browser clock verified as Fri Aug 28 2026 00:40 UTC. A job posted seconds earlier renders "Posted Yesterday" on the dashboard, job details page and job card (card shows just "Yesterday"). An application submitted seconds earlier also renders "Yesterday" in the employer dashboard application record. Expected "Today" / the current date.