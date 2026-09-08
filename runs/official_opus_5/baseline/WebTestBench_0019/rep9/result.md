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
    - Issue: No persistence: all state is lost on page reload
    - Actual: After reloading http://localhost:6019/ the header returned to Login/Register (session lost), the two threads created by qatester1 ("Zephyr testing strategies...", "Quokka morning routines...") disappeared, added replies/vote changes reverted to seed values, and localStorage/sessionStorage/cookies are all empty. Signing in again with the registered credentials qa1@example.com / pass123 was rejected with "Invalid credentials. Try: tech@example.com", proving the registered account did not survive the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not validated at login
    - Actual: Signing in as qa1@example.com with the wrong password "wrongpass999" (registered password was "pass123") succeeded: toast "Welcome back!" appeared, app navigated to /, and the header showed the logged-in user qatester1 with New Thread/profile/logout controls.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in gate for thread creation; unauthenticated /new-thread renders a blank page
    - Actual: Logged out, the UI offers no way to start a discussion (no New Thread link/button anywhere on the home page). Navigating to /new-thread renders an empty page (#root contains only the notifications region, no header, no form, no redirect to /login), so the user is never taken to a sign-in screen and cannot return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed on detail view
    - Actual: thread-4 header showed "67 replies" while the detail view listed only 1 reply ("Replies (1)"). After posting a reply the header showed "68 replies" but only 2 replies are listed ("Replies (2)"). Same mismatch on other seeded threads (e.g. thread-6 claims 89 replies).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or applied for Trending / Most Active
    - Actual: "Trending Discussions" and "Most Active Discussions" headings only show subtitle "Join the conversation with our community" — no period (e.g. "last 7 days") is stated anywhere, and no time-range control exists. Ranking uses all-time totals: threads created "over 2 years ago" occupy the top of both lists (e.g. Trending #1 = thread-6, 156 votes, posted over 2 years ago), while a thread created today with recent activity sits last.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded, do not reflect actual data and never update
    - Actual: Sidebar shows Technology (243) but /category/tech lists only 3 threads; Gaming (312) with 1 thread. Community Stats show 1,045 Threads / 4,823 Replies / 892 Members while the forum contains 9 threads, ~8 seeded replies and 6 users. After registering a new member and publishing a new Technology thread, all counts remained identical (Technology still 243, Threads still 1,045, Members still 892).