# Test Result

## Functionality
- [X] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.

- [X] FT-2: A registered user can edit their bio and comma-separated interests, save the changes, and see the saved bio and interests on their own public profile.

- [X] FT-3: A logged-in user can publish a discussion with a category, title, and body; the published discussion displays those values and appears in its selected category.

- [X] FT-4: A logged-in user can add a non-empty reply to a discussion, and the reply appears without a manual reload while the discussion's displayed reply total increases by one.

- [X] FT-5: A logged-in user can add or remove their single upvote on another user's discussion or reply, and the displayed count and selected state update immediately.

- [X] FT-6: Submitting a keyword search returns only discussions whose title or body contains the keyword, case-insensitively, and an unmatched keyword produces an empty result with clear feedback.

- [X] FT-7: Selecting a discussion category shows only discussions assigned to that category, and selecting a different category changes the result set to that category.

- [X] FT-8: The Most Active discussion list is ordered by reply count from highest to lowest and updates its order when reply activity changes the ranking.

- [X] FT-10: From a discussion or reply, users can open the author's public profile and view the username, bio, interests, join date, reputation, and authored discussions.

- [X] FT-13: Within the same loaded session, a user's saved profile details, authored discussions, replies, and vote state remain intact after logging out and logging back in.

- [ ] FT-18: Registered accounts, signed-in status, profile changes, authored discussions, replies, and vote state survive a page reload and remain available when the user returns.
  - Bug Report:
    - Issue: No persistence across page reload; all state is in-memory only
    - Actual: After navigating/reloading to /thread/thread-1 the header showed Login/Register (session lost) and reply box said "Login to Reply". Home page no longer lists the created thread "QA Zebra Test Thread about Quokkas"; profile edits and reply are gone; localStorage is empty ({}).


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not verified at login
    - Actual: Signing in with qatester1@example.com and the wrong password 'wrongpass999' succeeded: app redirected to / and header shows logged-in user 'qatester1' with New Thread link.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation; route renders blank page
    - Actual: Logged out, no "New Thread" affordance exists anywhere in the UI, and visiting /new-thread renders a completely empty page (document.body.innerText === "") with no sign-in screen, no message and no way to return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed on detail view
    - Actual: Thread "What programming language should I learn in 2024?" (thread-1) shows "23 replies" in header/list but its detail view lists only "Replies (2)". Other seeded threads show similarly inflated totals (e.g. 89, 67, 45 replies) with only 0-2 actual replies.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; trending/most-active rank all-time interactions
    - Actual: "Trending Discussions" and "Most Active Discussions" headers only say "Join the conversation with our community" — no time window text anywhere on the page (no "past 7 days"/"24 hours"/"this week"). Both lists rank by lifetime totals and include threads posted "over 2 years ago" at the top.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded and do not reflect or update with actual records
    - Actual: Sidebar shows General Discussion (156), Technology (243), Gaming (312) etc. while the forum actually contains 10 threads total (e.g. only 3 in General, 1 in Gaming). Community Stats stay at 1,045 Threads / 4,823 Replies / 892 Members even after registering 2 new users and creating 3 threads and 4 replies during the session.