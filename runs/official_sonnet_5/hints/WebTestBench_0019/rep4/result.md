# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username/email is not rejected
    - Actual: Registering with an already-used username and email (qaduptest / qaduptest@example.com) a second time with a different password was accepted without any error: the app redirected to home and signed the user in as qaduptest, instead of rejecting the duplicate. Also observed the account's password appears to change/break as a side effect (original password stopped working after the duplicate 'registration'). Registration, sign-out, and sign-back-in with same credentials in the same session worked correctly (qatester01 flow).

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
    - Issue: All state (accounts, sessions, threads) is lost on page reload
    - Actual: After registering a user (mainqauser), creating a thread, and then performing a page reload/hard navigation (visiting a URL directly), the app came back completely reset: the user was logged out, and attempting to log back in with the exact same registered credentials failed with 'Invalid credentials. Try: tech@example.com', indicating the account itself no longer existed. The site stats and category thread counts also reverted to original seed values, and the newly created thread was gone. State does not survive a reload at all.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [X] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread creation results in a blank page, not a usable sign-in screen
    - Actual: As a signed-out user, there is no 'New Thread' link/CTA available in the UI at all (nav only shows Login/Register). Navigating directly to /new-thread while unauthenticated does not redirect to a login screen nor create a thread; instead the page renders completely blank (document.body.innerText is empty, no form or message), with a console warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered.' indicating a broken client-side redirect. The user is stuck on a blank page with no way to sign in or return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match number of replies actually listed
    - Actual: On thread-1 ('What programming language should I learn in 2024?'), the top badge shows '23 replies' but the detail view's Replies section heading reads 'Replies (2)' and only 2 replies are actually listed (reply-1 by GameMaster42, reply-2 by NewsHound). The seeded reply-count metadata does not match the actual reply list length.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; ranking uses all-time totals, not a recent window
    - Actual: Neither the 'Trending Discussions' nor 'Most Active Discussions' view states any time window (e.g. 'last 7 days', 'past 24 hours') anywhere in the heading, subtitle, or filter controls — subtitle is just the generic 'Join the conversation with our community'. Additionally, both views rank threads that are 'over 2 years ago' alongside brand-new ones purely by all-time upvote/reply totals, with no evidence of filtering or weighting toward recent activity only.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category counts do not update when new content/members are created
    - Actual: After creating a new thread in the Technology category, the home page 'stats.threads' still shows 1,045 (unchanged) and the Technology category sidebar count still shows '243' (unchanged), even though the new thread is visibly present in the thread list. Also, after registering multiple new accounts (mainqauser2, qaduptest, etc.) during this session, 'stats.members' remained 892, not incremented.