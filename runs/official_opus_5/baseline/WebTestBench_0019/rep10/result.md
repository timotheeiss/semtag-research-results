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
    - Issue: No persistence across page reload — all state is in-memory only
    - Actual: After reloading http://localhost:6019/, the session was logged out (header shows Login/Register), localStorage is empty ({}), the two threads created during the session ("Zebra testing strategies...", "Antelope co-op games...") and the posted replies/votes were gone (only the 8 seeded threads remain), and logging in with the registered account qauser1@test.com/abc123456 failed with "Invalid credentials. Try: tech@example.com" — the registered account no longer exists.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in
    - Actual: Logging in as qauser1@test.com with the wrong password 'wrongpass999' succeeded: app navigated to '/' and header showed 'New Thread / qauser1', i.e. a logged-in session was created.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation; route renders a blank page
    - Actual: While logged out there is no visible way to start a discussion (header only has Login/Register, no "New Thread" control anywhere). Reaching /new-thread as an unauthenticated user (both via direct navigation and via in-app client-side routing) renders a completely blank page (document.body.innerText === "") instead of a sign-in screen, so the user can neither authenticate nor return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed
    - Actual: /thread/thread-1 showed "23 replies" in the meta but only 2 replies were listed ("Replies (2)"). After posting one reply the meta read "24 replies" while only 3 replies were listed ("Replies (3)"). Seeded reply counts are fabricated and inconsistent with the actual reply list.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; rankings use all-time totals
    - Actual: "Trending Discussions" and "Most Active Discussions" headers only say "Join the conversation with our community" — no time window (e.g. last 7 days) is stated anywhere. Rankings are pure all-time upvote/reply totals: threads posted "over 2 years ago" occupy every top slot, and a brand-new thread with a fresh upvote/reply still ranks last.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and do not reflect or update with actual records
    - Actual: Sidebar shows Technology (243), Gaming (312), etc., while /category/tech lists only 3 threads and /category/gaming only 2. Community Stats claim 1,045 Threads / 4,823 Replies / 892 Members although the forum contains 10 threads and 6 users. After creating 2 threads and 2 replies during the session all counts remained identical (243, 312, 1,045, 4,823, 892).