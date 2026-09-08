# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username is not rejected during registration; a second account with the same username but a different email is silently created.
    - Actual: Registered qa_tester_th1/qa_tester_th1@example.com successfully, logged in, logged out, and logged back in correctly (that part works). However, registering again with the same username "qa_tester_th1" but a different email ("different_email@example.com") succeeded without error: the app navigated to home already signed in, and a new profile URL /profile/user-1787688353329 (distinct user id) was created with username "qa_tester_th1", proving a duplicate-username account was created instead of being rejected.

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
    - Issue: No state survives a page reload: signed-in session, newly registered accounts, newly created discussions, replies, and votes are all lost.
    - Actual: After a page reload (browser_navigate to a thread URL, equivalent to revisiting the app), the previously signed-in user was logged out (nav reverted to Login/Register links). Thread-2's upvote count, which had been raised from 89 to 93 via 4 accounts' upvotes, reverted to the original seed value of 89. The discussion created during this session (thread-1787688440893, "QA Test Thread: Automated Testing Best Practices") no longer exists at all — navigating to its URL shows no thread.detail region and no title, indicating the app holds all state in memory only with no backend/localStorage persistence across reloads.


## Constraint
- [ ] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.
  - Bug Report:
    - Issue: Submitting a discussion with no category, title, or body does not create a discussion (correct) but gives no feedback identifying the missing fields.
    - Actual: Clicking "Create Thread" with category, title, and content all empty leaves the user on the same /new-thread page with no error message, no highlighted fields, and no toast. DOM inspection confirms no HTML5 validation constraints are set (checkValidity() returns true for all fields) and no error text is present anywhere in the form region. The user has no way to know why nothing happened.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and a logged-in session is created instead of being rejected.
    - Actual: After logging out (confirmed nav showed Login/Register links, not a profile), navigating to /login and submitting email "vote_bot_4@example.com" with the deliberately wrong password "WrongPassword999" resulted in a successful sign-in: the app redirected to home already authenticated, with nav.profile showing "vote_bot_4" and a Logout action present. No error was shown and no password check appears to be enforced.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated users have no way to reach discussion creation and are not routed to a usable sign-in screen when attempting it.
    - Actual: While logged out, the "New Thread" link is absent from navigation entirely (nav only shows Login/Register), so there is no visible UI path to attempt starting a discussion. Navigating directly to /new-thread renders a completely blank page (no form, no sign-in prompt, no message) — semantic_snapshot returns no elements and browser_snapshot shows only an empty notifications region. Console shows a React warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating a broken redirect-to-login guard. The user is left on a blank page with no way to sign in or return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the number of replies actually listed on the discussion detail view for seeded threads.
    - Actual: Thread-8 ("How to overcome creative block") displays "thread.reply-count": "42 replies", but the thread.replies collection on its detail page contains only 4 items — the 4 replies added during this test session. The original seed data's claimed 38 replies are not present as actual reply entries, so the displayed total (42) does not match the listed replies (4).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: Trending and Most Active views do not state or use any recent time window; they rank by all-time totals.
    - Actual: Neither the "Trending" nor "Most Active" filter buttons, nor the "Trending Discussions" heading, nor any surrounding text mentions a time window (e.g., "last 7 days", "this week", "past 24 hours"). A full-text scan of the page found no such phrase. The threads ranked at the top of both Trending and Most Active are posts from "over 2 years ago", confirming ranking is based on all-time upvote/reply totals rather than interactions within any stated recent period.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community stats and category counts do not update when new discussions, replies, or members are created.
    - Actual: Stats bar still shows "1,045" Threads even after creating a new thread (thread-1787688440893) — should be 1,046. "892" Members still shown after registering 5 new accounts (qa_tester_th1 x2, vote_bot_2, vote_bot_3, vote_bot_4) during this session — should be at least 897. "4,823" Replies unchanged after adding 5 new replies (1 on the QA thread, 4 on thread-8) — should be at least 4,828. The Technology category count remained "243" despite a new thread being published in that category — should be 244.