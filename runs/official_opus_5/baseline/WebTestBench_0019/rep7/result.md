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
    - Issue: No persistence — all user data is lost on page reload (state kept only in memory, localStorage/sessionStorage empty)
    - Actual: After reloading http://localhost:6019/: header reverted to Login/Register (session lost); the created thread 'Zephyr Quest speedrun strategies' disappeared; thread-7 reply count reverted from 46 to 41 and its 5 replies were gone; thread-2 upvotes reverted from 93 to 89. Signing in again with the registered qatester01@example.com/pass123 was rejected with 'Invalid credentials. Try: tech@example.com', i.e. the registered account no longer exists. localStorage and sessionStorage are both empty.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not validated at sign-in; any password grants a session
    - Actual: Signed in at /login with qatester01@example.com and deliberately wrong password 'wrongpass999'. App redirected to / and header shows 'New Thread / QaTester01', i.e. a fully logged-in session was created despite the incorrect password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread creation renders a blank page instead of a sign-in screen
    - Actual: While logged out, /new-thread renders a completely empty document (no header, no form, no login prompt, body text empty) and stays on /new-thread with no redirect after waiting. There is also no 'New Thread' entry point in the logged-out header, so an unauthenticated user gets no usable sign-in path or way back to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed on the detail view
    - Actual: /thread/thread-7 header showed '41 replies' while the detail view showed 'Replies (0) - No replies yet. Be the first to respond!'. After adding one reply the header showed '42 replies' but the list showed 'Replies (1)'. The seeded reply counts on every thread are fabricated numbers unrelated to the listed replies.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied for Trending / Most Active
    - Actual: 'Trending Discussions' and 'Most Active Discussions' both show the generic subtitle 'Join the conversation with our community'; no window (e.g. 'this week', 'last 24 hours') appears anywhere. Both lists rank by all-time totals — threads posted 'over 2 years ago' with lifetime upvote/reply counts dominate, while activity recency is ignored.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded fake numbers that never update
    - Actual: Sidebar shows Gaming (312) but /category/gaming lists only 2 threads; totals show 1,045 Threads / 4,823 Replies / 892 Members while the forum actually contains 9 threads and 6 accounts. After registering a new member the Members count stayed 892, and after publishing a new Gaming thread the Gaming count stayed 312 and Threads stayed 1,045.