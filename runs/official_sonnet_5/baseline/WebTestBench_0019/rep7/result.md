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
    - Issue: State does not persist across page reload
    - Actual: Before reload: logged in as qatester08 with custom bio/interests set, thread-2 at 93 upvotes (including qatester08's vote, shown active), thread-5 at 39 replies (including 5 replies posted by qatester08). After a full page reload (browser_navigate to /), the app returned to an unauthenticated state (nav shows "Login"/"Register" links instead of the user), all upvote buttons became disabled/unauthenticated, thread-2 reverted to 89 upvotes, and thread-5 reverted to 34 replies. All accounts created and all activity (votes, replies, profile edits) performed during the session were completely lost, indicating the application only persists state in client-side memory with no backend/localStorage persistence.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Incorrect password is accepted and creates a logged-in session
    - Actual: Immediately after registering qatester01 and logging out in the same SPA session, logging in with qatester01's email and a deliberately wrong password ("wrongpassword") succeeded: toast showed "Welcome back!" and the app navigated to authenticated home with the user's profile link in nav. Password was not validated for that in-memory account.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation does not redirect to a usable sign-in screen
    - Actual: While logged out, the navigation bar shows only Login/Register links (no "New Thread" link is exposed in the UI). Directly navigating to http://localhost:6019/new-thread as an unauthenticated user does not redirect to /login or show any sign-in prompt; instead the page renders completely blank (DOM root only contains the notifications region, no heading, form, or message). A console warning "You should call navigate() in a React.useEffect(), not when your component is first rendered" appears, suggesting the app attempts an improper redirect that fails, leaving the user stuck on a blank, non-functional page with no way to sign in or return to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [X] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated time window for Trending/Most Active views; ranking is not restricted to a recent window
    - Actual: The "Trending Discussions" view heading/subtitle reads only "Trending Discussions" / "Join the conversation with our community" - no mention of any time window (e.g. "This Week", "Past 24 Hours", "Past 30 Days"). Same for "Most Active Discussions" view: heading/subtitle also just "Most Active Discussions" / "Join the conversation with our community" with no time-window label. Furthermore, all threads shown in both views are timestamped "over 2 years ago", confirming the ranking algorithm considers all-time data rather than being scoped to any recent window, and no UI text anywhere states a time window is being applied.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category counts do not update when new data is created; counts are static/inaccurate
    - Actual: Before any changes: Community Stats sidebar showed Threads=1,045, Members=892; Technology category badge showed "(243)". However, filtering to /category/tech only ever displayed 2 actual thread articles (thread-1, thread-7), proving the "(243)" badge never reflected real data even at baseline. Registered a new account (qatester09): Members count remained "892" (did not increment to 893). Created a new real discussion in the Technology category ("QA test thread for CT-21 count verification"): afterward, /category/tech correctly showed 3 actual thread articles, but the sidebar "Technology" badge still read "(243)" (unchanged) and the global "Threads" stat still read "1,045" (unchanged). None of the counters (Threads, Members, category counts) update to reflect real data changes.