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
    - Issue: All state (accounts, session, profile edits, discussions, replies, votes) is lost on page reload
    - Actual: While logged in as qatester1 with an active upvote on thread-7 (count=93), reloaded the page via navigation to the same URL. After reload: user is signed out (nav shows Login/Register), thread-7 upvote count reverted to 92 and the upvote button is disabled (not attributable to a logged-out user's un-toggled state, it simply lost the vote), and thread.replies is completely empty ("No replies yet") despite earlier replies added in this session. Registered accounts, profile changes, authored discussions, and replies did not survive the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: Logging in with correct email qatester1@example.com but wrong password (tried 'wrongpassword' and 'definitelyWrong999') both succeeded, redirecting to home page with nav showing profile 'qatester1' and a logout action, i.e. a valid authenticated session was created despite the wrong password.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No usable sign-in redirect when unauthenticated user attempts to start a discussion
    - Actual: As an unauthenticated visitor, there is no "New Thread" link/CTA anywhere in the nav or homepage to attempt starting a discussion. Navigating directly to /new-thread renders a completely blank page (only an empty notifications region, no header, no sign-in form, no redirect to /login) — not a usable sign-in screen as required. The user has no way to reach discussion creation or get routed to authentication.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual number of replies listed on detail view
    - Actual: On thread-1, the header badge shows "24 replies" (was 23 before adding one reply, so it does increment correctly), but the detail view's "Replies (3)" heading and reply list only show 3 actual replies (reply-1, reply-2, and the newly added one). The displayed total (24) does not match the number of replies actually listed (3), a mismatch of 21.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending/Most Active, and ranking includes very old content
    - Actual: Headings are plain "Trending Discussions" and "Most Active Discussions" with subtitle "Join the conversation with our community" — no mention of any time window (e.g. "this week", "last 24 hours"). The threads populating both lists include posts timestamped "over 2 years ago", confirming ranking is not restricted to any recent interaction window; it appears to rank all-time totals.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category stats do not update when new discussions, replies, or members are created
    - Actual: During this session: 1 new thread was created (QA Test Discussion in Technology category), 5 replies were added across threads, and 5 new member accounts were registered (qatester1, voter2, voter3, voter4, voter5). Despite these, the home page Community Stats still show stats.threads=1,045, stats.replies=4,823, stats.members=892 — identical to the initial values before any of these actions. The Technology category badge also still reads "Technology (243)" even though a new tech thread was added and confirmed present in /category/tech. Counts are static and do not reflect actual created records.