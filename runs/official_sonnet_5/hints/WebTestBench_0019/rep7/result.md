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
    - Issue: All application state is held only in memory and is lost on page reload
    - Actual: After a page reload: the signed-in user (qauser05) was logged out (nav shows Login/Register instead of profile). The account's authored thread 'CT-21 Counter Test Thread' (thread-1787705579375) disappeared entirely from the threads list. Reply-count and upvote changes made during the session were reverted (thread-1 back to 23 replies from 32; thread-2 back to 89 upvotes from 93). Registered accounts, profile edits, authored discussions, replies, and vote state do not survive a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted for sign-in
    - Actual: Logged in as user 'qatester2026' (email different2026@example.com) using password 'wrongpassword' instead of the registered password 'password123'. The app redirected to home page with the user fully signed in (nav.profile shows username, logout action present) instead of rejecting the login.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation results in a blank page instead of a usable sign-in screen
    - Actual: Navigating to /new-thread while logged out renders a completely blank page (only an empty notifications region, no header, no nav, no form, no sign-in prompt). A console warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered' indicates a broken redirect attempt to the login screen that fails to actually render, leaving the user stuck on a blank page with no way to sign in or return to thread creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [X] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending/Most Active; ranking includes very old activity
    - Actual: The 'Trending Discussions' and 'Most Active Discussions' headings/subtext ('Join the conversation with our community') contain no mention of any time window (e.g. 'This Week', 'Last 30 days'). Threads shown in both lists include ones timestamped 'over 2 years ago' (e.g. thread-6, thread-4, thread-7), proving the ranking is not scoped to any recent period at all, it just ranks by all-time upvote/reply counts.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category stat counters do not update when new records are created
    - Actual: stats.threads remained '1,045' after creating 2 new threads (one still live: thread-1787705579375 in Creative Corner). Creative Corner category count remained '(89)' despite the new thread being added to it. stats.members remained '892' after registering 6 new accounts (4 still live: qauser02-05). stats.replies remained '4,823' after adding 9 replies to thread-1 (which alone should add +9). None of the community stat counters reflected the new activity.