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
    - Issue: All state lost on page reload (in-memory only, no persistence)
    - Actual: Reloading /thread/thread-1 while logged in as qa_tester2 (who had upvoted the thread and posted a reply) resulted in: user logged out (nav shows Login/Register again), thread.upvote count reverted from 48 to 47, thread.reply-count reverted from 24 to 23, and the "Persistence test reply from qa_tester2." reply disappeared entirely from the list. The registered account, session, profile edits, authored thread, and vote state do not survive a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Login does not validate password
    - Actual: Logging in with email qa_tester1@example.com and an incorrect password ("wrongpassword") succeeded: the app redirected to home and created a logged-in session (nav shows "qa_tester1" profile and logout action), instead of rejecting the attempt.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated new-thread access; blank page rendered
    - Actual: While logged out, there is no "New Thread" link in navigation. Navigating directly to /new-thread renders a completely blank page (no header, no form, no sign-in prompt) instead of redirecting to a usable sign-in screen.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual listed replies
    - Actual: thread-1 shows "23 replies" but only 2 reply items are rendered in the detail view; thread-6 shows "89 replies" but only 1 reply item is rendered. A newly created thread with 1 self-added reply did match (1=1), but pre-seeded threads show large discrepancies between the stated total and the actual listed replies.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window stated or enforced for Trending/Most Active
    - Actual: The Trending and Most Active views show no time-window label (no "This Week", "Last 7 days", etc.) anywhere in the sidebar filters or heading. All ranked threads display timestamps of "over 2 years ago", showing that ranking uses all-time interaction counts rather than being restricted to any recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category stat counters do not update on new records
    - Actual: Baseline: stats.threads=1,045, stats.replies=4,823, stats.members=892, General Discussion category=(156) — and category counts sum exactly to stats.threads at baseline (156+243+89+167+312+78=1045), so accuracy holds initially. After registering a new member (qa_tester3), creating a new thread in General Discussion, and posting a reply, all counters remained unchanged: stats.threads still "1,045" (expected 1,046), stats.replies still "4,823" (expected 4,824), stats.members still "892" (expected 893), and General Discussion category count still "(156)" (expected 157). Counts do not update when new discussions, replies, or members are created.