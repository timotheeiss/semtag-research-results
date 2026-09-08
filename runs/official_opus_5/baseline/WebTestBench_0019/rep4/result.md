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
    - Issue: No persistence — all data is in-memory and lost on page reload
    - Actual: After reloading http://localhost:6019/thread/thread-1 the header reverted to Login/Register (session lost), the reply I posted was gone ('Replies (3)' -> 'Replies (2)', reply text absent), the created thread/profile edits/votes were gone, and localStorage, sessionStorage and cookies were all empty. Signing back in with the registered account (different@example.com / secret123) then failed with 'Invalid credentials. Try: tech@example.com', proving the registered account itself did not survive the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in
    - Actual: Signed in at /login with email different@example.com (registered password 'secret123') using the wrong password 'wrongpass999'. App redirected to / and the header showed the logged-in user 'qatester1' with a New Thread link, i.e. a full logged-in session was created despite the incorrect password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation; /new-thread renders a blank page
    - Actual: While logged out there is no entry point to start a discussion (home page contains no 'New Thread'/'Start a discussion' affordance; only /login, /register, category, thread and profile links). Reaching the creation route directly at http://localhost:6019/new-thread while logged out renders a completely blank page (document body text empty, only the toast region rendered), the URL stays /new-thread, and the console warns 'You should call navigate() in a React.useEffect(), not when your component is first rendered' — so the intended redirect to the sign-in screen never happens and the user cannot authenticate and return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed
    - Actual: /thread/thread-1 header claimed '23 replies' while the detail view listed only 2 replies ('Replies (2)'). After posting one reply the header showed '24 replies' but only 3 replies were listed ('Replies (3)'). The seeded reply counts on all threads (e.g. thread-6 '89 replies', thread-4 '67 replies') are likewise unrelated to the listed replies.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or applied for Trending / Most Active
    - Actual: Trending view header reads only 'Trending Discussions — Join the conversation with our community' and Most Active reads 'Most Active Discussions'; neither states a period (no 'past week', 'last 7 days', 'today', etc.). Both rank by all-time totals: every listed thread is 'over 2 years ago' old and ranking is driven by lifetime upvote/reply counts (e.g. thread-6 with 156 all-time upvotes stays #1 in Trending, and my brand-new thread with recent activity ranks last), so no window-limited interaction filtering exists.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded, not derived from actual records
    - Actual: Sidebar shows Technology (243), Gaming (312), General (156), Creative (89), Lifestyle (167), News (78) and Community Stats 1,045 Threads / 4,823 Replies / 892 Members, while the forum actually contains only 8 seeded threads (3 in Technology) and 5 seeded users. After registering a new member and publishing a new Technology thread, Technology stayed at (243), Threads stayed at 1,045 and Members stayed at 892 — no count updated.