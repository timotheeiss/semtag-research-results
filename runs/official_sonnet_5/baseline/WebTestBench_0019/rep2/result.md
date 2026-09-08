# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username and duplicate email are not rejected at registration
    - Actual: Registering username 'qaTester2026' a second time (with a different email) succeeded and created a second account with the same username. Registering a different username 'qaTesterUniqueName' with the already-used email 'qatester2026@example.com' also succeeded and logged in as the new user. Both should have been rejected as duplicates. (Positive parts of the item worked: new registration auto-signs in the user, sign-out/sign-in with same credentials during the session works.)

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
    - Issue: App state does not persist across page reload
    - Actual: Registered and logged in as a new account "reloadTester" (session confirmed active, "New Thread" link and username visible in navbar). Performed a full page reload (navigate to http://localhost:6019/). After reload, the navbar reverted to showing "Login"/"Register" links instead of the authenticated user — the session was completely lost. This is consistent with the app storing all data (accounts, sessions, threads, replies, votes) only in client-side memory with no persistence layer (e.g., localStorage/cookies/backend), so any full page reload wipes registered accounts, login state, profile edits, authored content, and vote state.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted at login
    - Actual: Logged in as qaTester2026 (qatester2026@example.com) using password 'wrongpass' which does not match the registered password 'test123'. Toast showed 'Welcome back!' and the session became authenticated (New Thread link, profile link, logout button appeared).

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread creation does not redirect to a usable sign-in screen
    - Actual: While logged out, navigating to /new-thread (the only way to reach thread creation, since the "New Thread" nav link is absent when unauthenticated) results in a completely blank page (only an empty notifications region renders) instead of redirecting to the login screen. Console shows a React warning: "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating a broken/improperly implemented redirect. The URL remains stuck at /new-thread with no sign-in form displayed, so an unauthenticated user cannot proceed to sign in and return to discussion creation from this entry point.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the number of replies actually listed on the thread detail view
    - Actual: On thread-1 ('What programming language should I learn in 2024?'), the header displayed '23 replies' but only 2 replies were listed under 'Replies (2)'. After posting a new reply, the header showed '24 replies' while only 3 replies were listed under 'Replies (3)' — the header count is 21 higher than the actual listed replies, and this gap persists after adding a reply.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: Missing/incorrect time-window labeling for Trending and Most Active views
    - Actual: Both "Trending Discussions" and "Most Active Discussions" headings show only the generic subtitle "Join the conversation with our community" with no stated recent time window (e.g., "this week", "past 24 hours"). No tooltips, filters, or labels indicating a time window were found on either view. Additionally, all thread timestamps display "over 2 years ago", meaning the ranked content is not actually recent, which contradicts the expectation that trending/most-active rankings reflect a stated recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category stats do not update with new activity
    - Actual: Sidebar "Community Stats" (Threads: 1,045; Replies: 4,823; Members: 892) remained completely unchanged throughout the session despite creating 5+ new user accounts (qaMainTester, voteTester1-4), new threads, and 4 new replies added to thread-3 (which raised its own displayed reply count from 31 to 35). The aggregate "Replies" counter did not increase to reflect the added replies, and "Members" did not increase despite new registrations. Category thread counts (e.g., "General Discussion (156)") also remained static despite new content.