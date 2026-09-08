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
    - Issue: No persistence layer; all state lost on page reload
    - Actual: After a full page reload: nav reverted to Login/Register (signed-in session for qa_main lost), both discussions created during the session (thread-1787744110051, thread-1787744371001) disappeared from the thread list entirely, and thread-8's reply count reverted from 42 back to the original 38 (the 4 QA-added replies were lost). localStorage/sessionStorage are empty, confirming the app keeps all data purely in-memory with no persistence across reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted; login does not enforce the registered password
    - Actual: Logged in as qa_main (registered with password TestPass123) using email qa.main@example.com and an incorrect password 'WrongPassword999'. Login succeeded and a full authenticated session was created (nav showed profile 'qa_main' and logout action) instead of being rejected.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to thread-creation route does not redirect to a usable sign-in screen
    - Actual: While logged out, navigating to /new-thread renders a completely blank page (only an empty notifications region) instead of a sign-in form. URL remains /new-thread. Console shows a React warning 'You should call navigate() in a React.useEffect(), not when your component is first rendered', indicating the route guard's redirect logic is broken/misfiring, leaving the user stuck on a blank page with no way to sign in or return to thread creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [X] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or enforced for Trending/Most Active rankings
    - Actual: Headings are plain 'Trending Discussions' / 'Most Active Discussions' with subtitle 'Join the conversation with our community' - no mention of a time window (e.g. 'past 7 days', 'this week'). Threads ranked include ones timestamped 'over 2 years ago', ranked purely by all-time upvote/reply totals rather than restricted to any recent interaction window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category stats are static and do not update when new records are created
    - Actual: Baseline: stats.threads=1,045, stats.members=892, Technology category count=243. After registering new user 'qa_ct21', stats.members remained 892 (expected 893). After creating a new thread in the Technology category, stats.threads remained 1,045 (expected 1,046) and Technology category count remained 243 (expected 244), even though the new thread is visibly listed under Technology.