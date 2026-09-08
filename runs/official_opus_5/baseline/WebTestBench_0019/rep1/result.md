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
    - Issue: No persistence: all state is in-memory and lost on page reload
    - Actual: After registering qa_alpha@example.com/pass1234 and reloading the page (navigating to /login), signing in with the exact registered credentials returned "Invalid credentials. Try: tech@example.com". localStorage and sessionStorage are both completely empty (0 keys), so registered accounts, session, profile changes, threads, replies and votes do not survive a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in; any password is accepted
    - Actual: Signed in at /login as qa_alpha@example.com with the wrong password "wrongpass999" (registered password was "pass1234"). App showed a "Welcome back!" toast, redirected to /, and the header showed the logged-in user qa_tester_alpha with New Thread/profile/logout controls.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread creation renders a blank page instead of a sign-in screen
    - Actual: Logged out, the header offers no "New Thread" affordance at all; opening /new-thread directly renders an entirely empty page (document.body.innerText === "") with no redirect to /login, no sign-in form and no message, so the user cannot authenticate and return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply totals do not match the replies actually listed on the detail view
    - Actual: Thread "What programming language should I learn in 2024?" (/thread/thread-1) shows "23 replies" in its meta line while the detail view lists only 2 replies under "Replies (2)". After posting one reply the meta line went to "24 replies" but the list heading went to "Replies (3)" — the two totals remain inconsistent (24 vs 3). Same mismatch pattern on all seed threads (e.g. 89 replies claimed on the remote-work thread).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied; Trending/Most Active rank by all-time totals
    - Actual: The "Trending Discussions" and "Most Active Discussions" views show only the generic subtitle "Join the conversation with our community" — no time window such as "past 24 hours" or "this week" is stated anywhere. Both lists are topped by seed threads timestamped "over 2 years ago" (e.g. remote-work thread, 156 votes / 89 replies), proving ranking uses lifetime totals rather than interactions within a recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded fake numbers that do not reflect real data and never update
    - Actual: Sidebar shows Technology (243) but /category/tech lists only 3 threads; General (156)/Creative (89)/Lifestyle (167)/Gaming (312)/News (78) similarly vs 1-2 real threads each. Community Stats claims 1,045 Threads, 4,823 Replies, 892 Members while the forum contains 9 threads and ~7 users. After creating a new Technology thread and registering 2 new members, every count stayed identical (Technology still 243, Threads still 1,045, Members still 892).