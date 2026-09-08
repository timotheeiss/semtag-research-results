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
    - Issue: No persistence across page reload — all user data lost
    - Actual: After reloading, the session was gone (header back to Login/Register), the registered account no longer existed (/profile/user-1787845482842 renders 'User not found'), both authored threads and all posted replies disappeared (home list back to the original 8 seeded threads) and vote state reset. localStorage, sessionStorage and cookies are all empty — state is only kept in memory.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not verified at login
    - Actual: Signed in at /login with registered email different@example.com but deliberately wrong password 'wrongpass999'; app redirected to / and header showed 'New Thread / qa_tester1', i.e. a full logged-in session was created with an incorrect password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Guest gets a blank dead-end page instead of a sign-in screen
    - Actual: Logged out, no 'New Thread' entry point exists anywhere in the UI (header only shows Login/Register). Opening /new-thread as a guest renders a completely blank page — #root contains only the toast container, no header, no login form, no redirect (URL stays /new-thread after 2s). The user is never taken to a sign-in screen and cannot return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match listed replies
    - Actual: /thread/thread-1 header says '23 replies' (and list card says 23) but the detail view renders 'Replies (2)' with only 2 reply items. Seeded threads' totals are fabricated and inconsistent with actual reply content.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; ranking is all-time
    - Actual: 'Trending Discussions' and 'Most Active Discussions' views show only the subtitle 'Join the conversation with our community' — no period is stated anywhere (no 'this week/24h' label). Both rank by lifetime totals: every listed item is a thread posted 'over 2 years ago', ordered by all-time votes/replies rather than interactions within a recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and never update
    - Actual: Sidebar shows Technology (243), General Discussion (156), Gaming (312) etc. while /category/tech actually lists 3 threads and /category/gaming 1. Community Stats reads 1,045 Threads / 4,823 Replies / 892 Members although the forum contains 10 threads and 6 users. After creating 2 threads, 5 replies and 1 new member, every one of these numbers stayed identical (e.g. Technology still 243, Threads still 1,045, Members still 892).