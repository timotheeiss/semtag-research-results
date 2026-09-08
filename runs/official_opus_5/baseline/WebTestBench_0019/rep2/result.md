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
    - Issue: No persistence — all user data and session are lost on page reload
    - Actual: localStorage/sessionStorage are empty; state is in-memory only. After reloading /thread/thread-3: header reverted to Login/Register (session lost), the thread's 4 posted replies vanished ("Replies (0)", total back to 31), the upvote count reverted 57->56 with vote state cleared, and the authored thread "QA Zebra Protocol for testing search" no longer exists. Signing in again with the registered account other@example.com / secret123 fails with "Invalid credentials. Try: tech@example.com" — the registered account itself was discarded.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password is not verified at sign-in; any password grants a session
    - Actual: Account other@example.com was registered with password "secret123". Signing in with email other@example.com and password "wrongpass999" was accepted: app redirected to / and the header showed the logged-in user "qa_tester1". No error was shown and a full logged-in session was created.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated new-thread route renders a blank dead-end instead of a sign-in screen
    - Actual: Visiting /new-thread while signed out renders a completely blank page (document.body.innerText is empty, only a hidden notifications region in the DOM) and the URL stays at /new-thread even after waiting 2s. Console shows "You should call navigate() in a React.useEffect(), not when your component is first rendered" — the guard redirect fails, so no sign-in screen is presented and there is no way to continue to discussion creation. There is also no "New Thread" entry point anywhere in the signed-out UI (header, home or category pages), so the flow is unreachable for unauthenticated users.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies actually listed on the detail view
    - Actual: /thread/thread-3 header showed "31 replies" while the detail view listed "Replies (0) — No replies yet. Be the first to respond!". After posting 4 replies the header read "35 replies" but only "Replies (4)" were listed. Both numbers increment by 1 per new reply, but they never agree because seeded reply counts have no corresponding replies (same pattern on all seeded threads, e.g. 89/67/45 replies with none rendered).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: Trending/Most Active state no recent time window and rank by all-time totals
    - Actual: "Trending Discussions" and "Most Active Discussions" both show only the generic subtitle "Join the conversation with our community" — no time window ("last 7 days", "this week", "past 24h") appears anywhere in the page text. Ranking uses lifetime totals: the top trending/most-active entries are threads posted "over 2 years ago" (156 upvotes / 89 replies), so interactions are not limited to any recent window. The only date-aware control is a "Recent" tab, which sorts by creation date rather than scoping interactions.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hard-coded and do not reflect or update with actual records
    - Actual: Sidebar shows Technology (243), Gaming (312), General (156), Lifestyle (167), Creative (89), News (78) and Community Stats 1,045 Threads / 4,823 Replies / 892 Members, while the forum actually contains 9 threads total (Technology page lists 3, Gaming 1, etc.). After this session created 1 new thread, 4 replies and 3 new member accounts, every count was unchanged: Technology still (243), Threads still 1,045, Replies still 4,823, Members still 892.