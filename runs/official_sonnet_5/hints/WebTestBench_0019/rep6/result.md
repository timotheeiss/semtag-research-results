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
    - Issue: No data persists across a page reload; all state is purely in-memory and resets to seed data
    - Actual: While logged in as ft13tester (with a saved bio/interests, an authored thread, a posted reply, and an upvote on thread-1 raising it to 48), reloading the page (navigating to '/') logged the user out entirely, removed the authored thread from the trending list, reverted thread-1's upvote count back to 47, and reset community stats.threads back to the original 1,045. No registered accounts, profile edits, discussions, replies, or votes survive a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: After logging out of qatester01, submitted login with correct email but wrong password ('wrongpassword'). The app redirected to home page and showed the user as fully signed in (nav.profile = 'qatester01', logout button present), instead of rejecting the credentials.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No in-app entry point to start a discussion while logged out, and direct navigation to the discussion-creation route renders a blank page instead of a sign-in screen
    - Actual: While logged out, the home page navigation only shows Login/Register (no 'New Thread'/'Start Discussion' link), so there is no in-app way to attempt starting a discussion. Directly visiting /new-thread (reproduced twice) renders a completely blank page (only an empty notifications region) with no sign-in form and no redirect to /login.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [X] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recent time window is stated or applied for Trending/Most Active views
    - Actual: Trending Discussions and Most Active Discussions headings only show the generic subtitle 'Join the conversation with our community' with no mention of a time window (e.g. 'last 7 days', 'this week'). The ranked threads themselves are all timestamped 'over 2 years ago', showing the lists rank by all-time upvote/reply totals rather than interactions within any stated recent period.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community stats and category counts are static and do not update when new discussions, replies, or members are created
    - Actual: Registered a new account (ct21tester) - stats.members remained 892 (no increment). Created a new thread in General Discussion - stats.threads remained '1,045' (expected 1,046) and category count remained 'General Discussion (156)' (expected 157). Added a reply to that thread - stats.replies remained '4,823' (expected 4,824). None of the community/category counters reflect newly created records.