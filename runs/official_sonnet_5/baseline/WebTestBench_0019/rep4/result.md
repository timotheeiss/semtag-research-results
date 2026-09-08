# Test Result

## Functionality
- [ ] FT-1: A visitor can register a unique username and email with a password of at least six characters, is signed in as the new user, and can sign out and sign back in with the same credentials during the current session; duplicate usernames or emails are rejected.
  - Bug Report:
    - Issue: Duplicate username/email not rejected
    - Actual: Registration, sign-out, and sign-back-in with the same credentials all worked correctly. However, re-registering with the exact same username (qaTester01) and email (qatester01@example.com) as an already-registered account succeeded instead of being rejected - a new account (user-1787605860664) was created and the user was signed in as a duplicate, with toast 'Account created! Welcome to ThreadHive!' shown instead of an error.

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
    - Issue: All application state is held in-memory only and is completely lost on page reload
    - Actual: Registered account "reloadTester" (reloadtester@example.com), edited profile bio ("FT-18 reload persistence test bio.") and interests (Reload, Persistence), created a thread "FT-18 reload test thread", and upvoted it (0→1). Then performed an explicit page reload/revisit of the thread's URL. After reload: the navbar reverted to logged-out state (Login/Register links, session lost), and the thread itself was completely gone — the page displayed "Thread not found" with only a "Go Home" button. Navigating home would also show the original seeded data with the new account, profile edits, thread, and vote entirely erased (consistent with earlier observation that any full page navigation resets all app state, since it is not persisted to localStorage or a backend). None of the required data (registered account, signed-in status, profile changes, authored discussions, replies, vote state) survives a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Sign-in accepts incorrect password and creates a valid session
    - Actual: Logged out of voterThree account (registered with email voterthree@example.com / password "secret6"). On the Login page, entered email "voterthree@example.com" with an intentionally WRONG password "wrongpassword123" and clicked Sign In. Instead of being rejected, the app displayed a "Welcome back!" success toast, redirected to the home page, and fully authenticated the session — navbar showed "New Thread" link and "voterThree" profile link/avatar (logged-in state), identical to a successful login with the correct password. No error was shown and the incorrect credential was accepted, meaning password validation is not enforced during sign-in.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation results in a blank page instead of a usable sign-in screen
    - Actual: While logged out, there is no "New Thread" link visible in the navbar (only Login/Register), so there is no in-app way to attempt starting a discussion. Navigating directly to the /new-thread route (the underlying protected route for thread creation) while unauthenticated does not redirect to a usable sign-in screen. Instead the page renders completely blank (document.body.innerText is empty, only an empty notifications region remains in the accessibility tree). Browser console shows a React warning: "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating a buggy/improper redirect attempt that fails to render either the new-thread form or a login prompt. An unauthenticated user has no way to reach or return to discussion creation after this failure.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Reply total shown on discussion header does not match number of reply items rendered for seeded/pre-existing threads
    - Actual: For pre-existing thread-1 ('What programming language should I learn in 2024?'), the header/list shows '23 replies' but the Replies() section heading shows 'Replies (2)' and only 2 reply items are actually rendered in the list. This mismatch exists on other seeded threads too (e.g. thread-6 shows '89 replies' but likely has only its seeded 2 replies). Note: for threads created during this test session, the counts did stay consistent when replies were added live (see prior evidence), but the pre-seeded data itself is inconsistent, so the checklist requirement is not met overall.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending/Most Active; ranking includes very old content
    - Actual: Checked both "Trending Discussions" and "Most Active Discussions" views. Neither the section heading, subtitle ("Join the conversation with our community"), nor any visible label/tooltip states a recent time window (e.g., "This Week," "Last 24 Hours," "Past Month"). Both lists include and rank threads whose timestamps show "over 2 years ago" alongside their upvote/reply counts, with no filtering to recent activity — i.e., ranking is by all-time upvote/reply totals rather than interactions within any stated recent window. There is no UI element anywhere on the page communicating a time window for these rankings.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community Stats and category counts are static and do not reflect actual data or update live
    - Actual: Baseline Community Stats: Threads 1,045, Replies 4,823, Members 892; General Discussion category showed (156). Registered a new account "ct21Tester" — Members count remained 892 (did not increment to 893). Created a new thread "CT-21 stats accuracy test" in the General Discussion category — Threads count remained 1,045 and General Discussion category count remained (156), both unchanged despite a new thread existing (visible in the Trending list with 0 upvotes/1 reply). Posted a reply to that thread — Replies count remained 4,823, unchanged. None of the sidebar stats (Threads, Replies, Members) or the per-category discussion counts update to reflect newly created accounts, discussions, or replies; they appear to be static/hardcoded values disconnected from actual live data.