# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate email/username not rejected
    - Actual: Registered 'qatester24' with a new email successfully even though the username 'qatester24' already existed on another account; a second account with the same username was created (redirected to profile/user-1787583292999) with a 'Account created!' toast instead of being rejected.

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
    - Issue: Application state does not persist across a page reload — no backend/persistence layer
    - Actual: While logged in as qatester99 with saved profile bio/interests and two authored threads (visible at /profile/user-1787583662204), performing a full page reload of that exact URL resulted in: nav bar reverting to logged-out state (Login/Register links instead of New Thread/username), and the profile page itself displaying "User not found" with a "Go Home" button — the account, session, profile edits, and authored threads were all wiped by the reload. All application state is held only in-memory client-side with no backend persistence.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted
    - Actual: Logging in with qatester24@example.com and password 'wrongpassword' (registered password was 'password123') succeeded, showing 'Welcome back!' and creating a logged-in session as qatester24.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated user attempting to start a discussion is not taken to a usable sign-in screen
    - Actual: While logged out, navigating to the new-thread route (/new-thread) results in a blank page — only a "Notifications" live region renders, with no visible sign-in form, no discussion form, and no error message. The URL remains /new-thread. Console shows a warning: "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating the app attempts an in-render redirect that fails to produce usable content. Waiting 2s did not change the blank state.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply total mismatch for pre-seeded discussions
    - Actual: On thread-3 ('Digital art vs traditional art'), the header/summary shows '31 replies', but the Replies section below shows 'Replies (0)' and 'No replies yet. Be the first to respond!' — the displayed reply total does not match the actual replies listed on the detail view for seeded discussions. (Newly created threads via the UI do keep the two counts consistent, but pre-existing/seed discussions do not.)

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No recency time window is stated or enforced for Trending/Most Active views
    - Actual: The Trending and Most Active views only display the headings "Trending Discussions" and "Most Active Discussions" with the generic subtitle "Join the conversation with our community" — no time window (e.g. "this week", "past 30 days") is disclosed anywhere in the UI. Both views rank and include threads timestamped "over 2 years ago" directly alongside threads created "less than a minute ago" / "1 minute ago" in the same ranked list, with no visible restriction to a recent window.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community stat counts do not update when new discussions are created
    - Actual: Before creating any new threads, "General Discussion" category showed (156) and Community Stats showed "1,045 Threads". After creating two new threads (Trending Test Alpha, Trending Test Beta) in the General Discussion category via the UI, the category sidebar still shows "General Discussion (156)" (unchanged) and Community Stats still shows "1,045 Threads" (unchanged), even though both new threads are visibly present in the discussion list. The displayed counts are static and do not reflect actual discussion records.