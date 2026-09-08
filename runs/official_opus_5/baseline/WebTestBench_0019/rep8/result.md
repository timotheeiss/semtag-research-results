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
    - Issue: No persistence across page reload
    - Actual: After registering qatester1/qatester1@example.com (password secret123) and reloading the page, signing in with those exact credentials is rejected with "Invalid credentials. Try: tech@example.com". localStorage and sessionStorage are both empty, so accounts, session, profile edits, threads, replies and votes are all in-memory only and lost on reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [X] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation
    - Actual: While logged out, opening /new-thread renders a completely blank page (document.body.innerText is empty, only the toast region exists). No login form, no message, and no way to return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed on detail view
    - Actual: thread-4 "Best indie games of 2023" shows "67 replies" in its meta line while its detail view lists "Replies (1)" with a single reply. Same mismatch pattern for all seeded threads (e.g. thread-6 shows 89 replies). Only newly created threads are consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending / Most Active
    - Actual: Trending shows "Trending Discussions - Join the conversation with our community" and Most Active shows "Most Active Discussions - Join the conversation with our community". Neither view states any time period, offers a period selector, nor limits ranking to recent interactions: threads created "over 2 years ago" are ranked using all-time vote/reply totals.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and do not reflect or update with real data
    - Actual: Sidebar shows Technology (243), Gaming (312), General (156)... and Community Stats 1,045 Threads / 4,823 Replies / 892 Members, but the forum actually contains 9 threads total (Technology page lists 3, Gaming lists 1) and only a handful of members. After registering 2 new accounts, creating a Technology thread and posting a reply, every count stayed identical (Technology still 243, Threads still 1,045, Replies still 4,823, Members still 892).