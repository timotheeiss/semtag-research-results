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
    - Issue: All account, profile, content, and vote data is lost on page reload
    - Actual: After establishing a full session (registered 'sessiontest', edited profile bio/interests, created a thread, replied to thread-1, upvoted thread-1 to 48), reloading the page (navigating to /thread/thread-1) logged the user out entirely (nav shows Login/Register, not signed in), thread-1's upvote count reverted to 47 and reply count reverted to 23 (sessiontest's reply gone), and a later re-check of the register form confirmed the sessiontest account itself no longer exists. None of the registered account, profile changes, authored discussions, replies, or vote state survived the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: Logged out qatester1, then submitted login form with correct email (qatester1@example.com) but wrong password 'wrongpassword'. The app navigated to home page and displayed the user as signed in (nav showed 'qatester1' profile link and logout action), instead of rejecting the login.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation renders a blank page instead of redirecting to a usable sign-in screen
    - Actual: While signed out, navigating to /new-thread (the only route to start a discussion) rendered a completely blank page (no header, no form, no sign-in prompt) — URL stayed at /new-thread. Console showed a React warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating an intended redirect to login is broken. No usable sign-in screen was reached, so users cannot resume discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual number of replies listed on detail view
    - Actual: On thread-4, header stat showed '67 replies' while the 'Replies' section only listed 1 reply (heading 'Replies (1)'). After adding a new reply, header updated to '68 replies' but the list only showed 2 replies (heading 'Replies (2)'), a persistent 66-reply discrepancy between the displayed total and the actually listed replies.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied for Trending/Most Active rankings
    - Actual: Trending and Most Active views show no time-window label (e.g. 'this week', 'last 30 days') anywhere in the UI (headings just say 'Trending Discussions' / sort button 'Most Active' with no period qualifier). All ranked threads are timestamped 'over 2 years ago' and are included in the Trending ranking purely by all-time upvote/reply totals, with no evidence of filtering to a recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are static and do not reflect actual forum records
    - Actual: Site-wide stats.threads stayed at '1,045' and stats.members stayed at '892' even after registering a new account (counttest) and creating a new thread. Category nav count 'News & Current Events (78)' did not increment after adding 'Count Test Thread' to that category. Additionally, category counts (e.g. 'Technology (243)') vastly exceed the actual number of threads visible in that category (only 2-3 real threads), showing counts are hardcoded/decorative rather than derived from actual records.