# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username/email is not rejected during registration
    - Actual: Registering with username 'qatester1' and email 'qatester1@example.com' a second time (already used by an existing account) succeeded and created a new distinct account (new profile URL /profile/user-1787633244065) instead of being rejected as a duplicate. Sign-out/sign-in with correct credentials during the session did work correctly.

- [X] FT-2: A registered user can edit their bio and comma-separated interests, save the changes, and see the saved bio and interests on their own public profile.

- [X] FT-3: A logged-in user can publish a discussion with a category, title, and body; the published discussion displays those values and appears in its selected category.

- [X] FT-4: A logged-in user can add a non-empty reply to a discussion, and the reply appears without a manual reload while the discussion's displayed reply total increases by one.

- [X] FT-5: A logged-in user can add or remove their single upvote on another user's discussion or reply, and the displayed count and selected state update immediately.

- [X] FT-6: Submitting a keyword search returns only discussions whose title or body contains the keyword, case-insensitively, and an unmatched keyword produces an empty result with clear feedback.

- [X] FT-7: Selecting a discussion category shows only discussions assigned to that category, and selecting a different category changes the result set to that category.

- [X] FT-8: The Most Active discussion list is ordered by reply count from highest to lowest and updates its order when reply activity changes the ranking.

- [X] FT-10: From a discussion or reply, users can open the author's public profile and view the username, bio, interests, join date, reputation, and authored discussions.

- [ ] FT-13: Within the same loaded session, a user's saved profile details, authored discussions, replies, and vote state remain intact after logging out and logging back in.
  - Bug Report:
    - Issue: User's own vote status does not persist across logout/login within the same session
    - Actual: While logged in as qatester5, the upvote button on thread-2 showed as active/pressed (having previously upvoted it, raising count to 93). After logging out and logging back in as the same account (qatester5@example.com) within the same browser session, the profile page and authored-threads list correctly still showed qatester5's own data (username, 0 threads, no bio), but on thread-2 (both in the list view and on /thread/thread-2 detail view) the upvote button no longer shows as active/selected, even though the vote count itself remained at 93. This indicates the app forgets which items the user personally upvoted after a logout/login cycle, while other profile state was preserved correctly.

- [ ] FT-18: Registered accounts, signed-in status, profile changes, authored discussions, replies, and vote state survive a page reload and remain available when the user returns.
  - Bug Report:
    - Issue: Application state (session, accounts, votes) does not survive a full page reload
    - Actual: While logged in as qatester5 on /thread/thread-2 (vote count 93 after this session's upvotes from qatester2-5), performed a full page reload via browser_navigate to the same URL. After reload: (1) the user was signed out - nav bar reverted to 'Login'/'Register' links instead of showing qatester5's profile; (2) thread-2's vote count reverted from 93 back to 89, and the vote/reply buttons became disabled pending login, showing all in-memory votes and the session were lost; (3) reply box changed to 'Login to Reply' disabled state. This confirms accounts, sign-in state, and vote data are stored only in-memory and are wiped by a page reload, rather than persisting as expected.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: Logged in as qatester1@example.com with password 'wrongpassword' (registered password was 'password1'). App showed 'Welcome back!' toast and established an authenticated session (nav showed 'New Thread' and 'qatester1' profile link), instead of rejecting the login.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation results in a blank page instead of a usable sign-in screen
    - Actual: Navigated to /new-thread while logged out. The page rendered blank (only the notification region, no sign-in form, no redirect to /login, no message). Console showed a warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating a broken redirect attempt. Waited 2s with no change. The user is not taken to any usable sign-in screen nor given a path back to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual number of replies listed in detail view for existing seeded threads
    - Actual: On /thread/thread-6 ('Discussion: The future of remote work'), the thread header shows '89 replies', but the Replies section below only lists 1 actual reply and the heading reads 'Replies (1)'. For a newly authored thread (thread-1787633293280), the count and detail list did stay consistent (0→1) after adding a reply, but pre-existing seeded threads show a mismatch between the summary reply count and the actual reply list length.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: Trending/Most Active views do not state any time window and are not limited to recent activity
    - Actual: The 'Trending Discussions' and 'Most Active Discussions' headings only show the subtitle 'Join the conversation with our community' - no time window (e.g., 'This Week', 'Last 30 Days') is stated anywhere in the sidebar tabs, headings, or list. Additionally, all threads shown in both Trending and Most Active lists are timestamped 'over 2 years ago', proving the ranking is not restricted to any recent window - it ranks the entire all-time thread set by vote count / reply count with no recency filter or disclosure.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community stats and category counts do not update when new content is created
    - Actual: Before creating a new thread, sidebar showed 'Threads: 1,045' and 'Creative Corner (89)'. Created a new thread titled 'CT-21 count verification thread' in the Creative Corner category as user TechExplorer; it was created successfully and appeared in the Trending list. After returning to the homepage, 'Threads' stat remained unchanged at '1,045' (should be 1,046) and 'Creative Corner' category count remained unchanged at '(89)' (should be 90) - the displayed counts do not reflect the actual current data.