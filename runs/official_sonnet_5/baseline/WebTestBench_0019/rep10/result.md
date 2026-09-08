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
    - Issue: App state does not survive a page reload
    - Actual: After registering and signing in as qaTester1, navigating the browser to the app URL again (full page reload) reset the app to a logged-out guest state, and the Community Stats reverted to the seed values (1,045 threads). Attempting to log back in with the same registered credentials (qaTester1@example.com / test123) after the reload returned "Invalid credentials. Try: tech@example.com" — the registered account itself was lost, not just the session.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: Logging in with email qaTester1@example.com and an incorrect password ("wrongpassword") succeeded: app showed "Welcome back!" toast and navigated to home page fully signed in as qaTester1. Password is not actually validated on this account.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation does not redirect to a usable sign-in screen
    - Actual: While logged out (confirmed nav shows only Login/Register links, no "New Thread" link), navigating to /new-thread renders a completely blank page (document.body.innerText is empty; #root only contains the notifications region, no form, no login prompt, no nav). URL stays at /new-thread rather than redirecting to /login. Console shows a React warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating a broken/improper redirect attempt that fails to actually navigate the user to the sign-in screen. As a result, an unauthenticated user attempting to start a discussion is stuck on a broken blank page with no way to sign in or return to discussion creation, rather than being taken to a usable sign-in screen as required.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply count stat does not match actual number of replies listed for pre-seeded threads
    - Actual: For seed thread-6 ("Discussion: The future of remote work"), the thread header/summary shows "89 replies", but the detail view's Replies section shows "Replies (1)" with only 1 reply actually listed. Similarly thread-8 ("How to overcome creative block") shows "38 replies" in its summary but the detail view shows "Replies (0)" / "No replies yet." This mismatch is only correct for threads created during this test session (e.g. my own thread-1787647313434, where the counts did match after adding a reply), but the seeded discussions' displayed totals are inconsistent with their actual reply lists.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No time window is stated for Trending/Most Active views
    - Actual: The "Trending Discussions" and "Most Active Discussions" headings only show the generic subtitle "Join the conversation with our community" with no indication of a time window (e.g., "This Week", "Last 24 Hours", "All Time"). Additionally, ranking is not scoped to any recent period - all-time seed threads from "over 2 years ago" are ranked directly alongside brand-new threads created seconds ago, with no visible time-window boundary or filter.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and Community Stats counts are static and do not reflect actual data
    - Actual: Community Stats sidebar still shows the original seed values (1,045 Threads, 4,823 Replies, 892 Members) even after this session created 1 new thread, posted 6+ new replies, and registered 4 new accounts (qaTester1 x2, qaVoter99, qaVoter100) - none of these actions incremented the stats. Additionally, the "Technology (243)" category count is wildly inconsistent with the actual number of threads visible in that category (only 3: thread-1, thread-7, and the newly created thread-1787647313434), confirming the displayed counts are hardcoded/fake rather than derived from actual records.