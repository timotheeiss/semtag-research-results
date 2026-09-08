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
    - Issue: All application state is lost on page reload; nothing persists
    - Actual: While logged in as qatester1 (with saved bio/interests, an authored thread, and multiple votes cast by test accounts), performing a full page reload (navigating to http://localhost:6019/) logged the user out entirely (nav reverted to Login/Register) and reset all data to its original seeded state: thread-2's vote count reverted from 93 back to 89, thread-8's replies reverted from 42 back to 38, thread-1's replies reverted from 24 back to 23, and both qatester1's and qatester4's authored threads disappeared from the listings entirely. No registered accounts, profile edits, authored content, or vote state survive a reload; the app stores everything only in in-memory JS state with no persistence layer (localStorage/backend).


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted, creating a logged-in session
    - Actual: After registering qatester1@example.com and logging out, submitting the login form with email qatester1@example.com and an incorrect password ("wrongpassword") succeeded with a "Welcome back!" toast and signed the user in, instead of being rejected.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread creation renders a blank page instead of a usable sign-in screen
    - Actual: Navigating to /new-thread while logged out does not redirect to the login page or show any sign-in prompt; the page renders essentially blank (only the notification region is present, no heading, form, or message). The browser console shows a warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating the intended redirect-to-login logic is implemented incorrectly and fails to execute, leaving the user stuck on an empty, unusable page with no way to sign in and return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match number of replies listed in detail view
    - Actual: On thread-1, the header stat shows "24 replies" (after adding one reply, up from 23) but the "Replies (3)" section lists only 3 actual reply items. The displayed total is a seeded/static number disconnected from the actual reply list length.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending/Most Active; ranking uses all-time totals, not a recent window
    - Actual: Both "Trending Discussions" and "Most Active Discussions" views only display the generic subtitle "Join the conversation with our community" with no stated time window (e.g., "This Week," "Last 30 Days"). Both views rank threads by all-time cumulative vote/reply totals rather than interactions within any recent window — threads timestamped "over 2 years ago" rank at the top (e.g., thread-6 with 89 replies/156 votes from 2+ years ago is #1 in both views), while threads created minutes ago rank at the bottom. There is no recency-scoped calculation or label anywhere in the UI.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community stat counts are static and do not update when new discussions/members are created
    - Actual: Technology category count remained "243" both before and after creating two new Technology-category threads (thread-1787612961776 and thread-1787613350861) during this session. Community Stats sidebar "Threads" remained "1,045" despite at least 2 new threads being created, and "Members" remained "892" despite 4 new accounts (qatester1-4) being registered. These counts appear to be static/seeded values disconnected from actual data.