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
    - Issue: No persistence — all state is lost on page reload
    - Actual: After a page reload the app resets to seed data: the session is dropped (header shows Login/Register instead of qa_tester_01), both threads I created (QA Test Thread About Widgets, QA Ranking Control Thread) disappear from the home list (only the 8 seeded threads remain), my replies are gone (thread-8 back to '38 replies' with 1 reply listed instead of 42/5), my upvote state is cleared, and the registered account itself no longer exists — logging in afterwards with qa_tester_01@example.com / secret123 returns 'Invalid credentials. Try: tech@example.com'. localStorage and sessionStorage are both empty (no keys).


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not validated at sign-in
    - Actual: Signed in at /login with email qa_tester_01@example.com and an incorrect password 'wrongpass999' (registered password was 'secret123'). The app redirected to home and the nav showed the logged-in state (nav.profile = 'qa_tester_01', logout button present), creating a full session.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation; blank page
    - Actual: While logged out there is no 'New Thread' entry point anywhere in the UI (logged-out nav only offers Login/Register). Opening the real thread-creation route /new-thread (same route the logged-in 'New Thread' link uses) while unauthenticated renders a completely empty page: document.body.innerHTML contains only an empty #root div, no sign-in screen, no message, and no way to return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed
    - Actual: thread-4 shows 'thread.reply-count = 67 replies' but its detail view lists only 1 reply (section header 'Replies (1)'). After posting a reply the header became 'Replies (2)' while the total became '68 replies' — the two numbers remain inconsistent (68 vs 2). The same mismatch exists across seeded threads (e.g. thread-6 '89 replies').

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or applied for Trending / Most Active
    - Actual: The Browse filters ('Trending', 'Recent', 'Most Active') and the list heading area ('Trending Discussions' / 'Most Active Discussions' with subtitle 'Join the conversation with our community') state no time window anywhere on the page. Ranking uses all-time totals: the Trending list is topped by thread-6 (156 upvotes) posted 'over 2 years ago' and every top entry is 2+ years old, and Most Active uses lifetime reply counts, so no recency window is applied.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and inaccurate; never update
    - Actual: Forum actually contains 10 threads (8 seeded + 2 I created), 6 users and a handful of replies, yet Community Stats permanently show Threads 1,045 / Replies 4,823 / Members 892 and category counts General(156) Technology(243) Creative(89) Lifestyle(167) Gaming(312) News(78) — e.g. Technology shows 243 but /category/tech lists 3 threads, Gaming shows 312 but lists 1. After registering a new member, creating 2 threads and posting 5 replies, every one of these counts was byte-identical to its initial value (1,045 / 4,823 / 892 and unchanged category numbers).