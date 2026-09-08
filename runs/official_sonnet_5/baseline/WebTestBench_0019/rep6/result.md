# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username not rejected
    - Actual: Registered "qatester1"/qatester1@example.com successfully, signed in, signed out, and signed back in with same credentials (that part worked). However, registering again with username "qatester1" but a different email (different_email@example.com) succeeded ("Account created! Welcome to ThreadHive!") and created a new account (user-1787619520082) instead of being rejected as a duplicate username.

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
    - Issue: All application state is lost on page reload instead of persisting
    - Actual: Established state as qatester_persist (registered account, edited bio/interests, authored a thread with a reply, upvoted thread-7 raising it from 92 to 93) then performed a full page reload. After reload: nav bar reverted to logged-out state (Login/Register links); navigating to the account's profile URL directly returned "User not found" (account no longer exists); the home Trending list no longer showed the authored thread at all (back to only the original 8 seed threads); thread-7's upvote count reverted to 92 and its vote button returned to the disabled/unvoted state. This shows the app relies entirely on non-persisted in-memory state — no registered accounts, sign-in sessions, profile edits, authored content, or votes survive a page reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password accepted for login
    - Actual: Logging in with qatester1@example.com and an incorrect password ("wrongpassword") succeeded, showing "Welcome back!" toast and a fully logged-in session (New Thread link, profile link for qatester1) instead of being rejected.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated user is not redirected to a usable sign-in screen when attempting to start a discussion
    - Actual: While logged out, navigating to the New Thread page (/new-thread) renders a completely blank page (only the notification region present) instead of showing the discussion form or redirecting to a sign-in screen. Console shows a React warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating the intended redirect-to-login logic is implemented incorrectly and fails to execute, leaving the user stuck on a blank page with no way to sign in or proceed.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual replies listed on detail view
    - Actual: Thread-1 ("What programming language should I learn in 2024?") displays "23 replies" in its header/summary, but the detail view's Replies section heading reads "Replies (2)" and only 2 reply items (GameMaster42, NewsHound) are actually rendered. The displayed total (23) does not match the number of replies listed (2).

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated time window for Trending/Most Active rankings
    - Actual: Both "Trending Discussions" and "Most Active Discussions" views show only the generic subtitle "Join the conversation with our community" with no indication of a time window (e.g., "last 7 days", "this week"). Rankings appear to be computed from all-time upvote/reply totals rather than a recent window, and no UI text states any such window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community Stats "Members" count does not update when new accounts are created
    - Actual: Registered 5 new accounts during this session (qatester2, qatester_vote1, qatester_vote2, qatester_vote3, qatester_vote4) via the UI, but the "Members" count in Community Stats remained fixed at 892 throughout, never incrementing. By contrast, the "Replies" counter (4,823) did correctly reflect the 5 replies added during testing, showing counts are selectively stale.