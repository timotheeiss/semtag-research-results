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
    - Issue: No persistence across page reload; all state is purely in-memory
    - Actual: After reloading http://localhost:6019/, the signed-in session for qatester1 was lost (nav shows Login/Register again). The account qatester1 itself is gone (later confirmed login with qatester1@example.com/password1 would fail as unregistered). The authored thread "Persistence Test Discussion" no longer appears in the thread list. thread-6 upvotes reverted from 157 back to seed value 156 (our vote lost). thread-2 reverted from 93 to seed 89. thread-1 reverted from 33 replies back to seed 23 replies. All registrations, profile edits, authored content, replies, and votes are stored only in front-end memory and do not survive a reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Authentication does not verify password
    - Actual: Signing in with correct email but a deliberately wrong password ("totallywrongpass999") succeeded: user was redirected to home page and logged in as qatester1 (nav.profile showed "qatester1", logout action present). Any password is accepted for a valid email.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated access to discussion creation produces a blank page instead of a sign-in screen
    - Actual: As an anonymous user, there is no "New Thread" link in the nav at all. Navigating directly to /new-thread (the create-discussion route) renders a completely blank page (only the notifications region, no form, no message, no redirect to /login) — URL stays at /new-thread. Console shows a React warning "You should call navigate() in a React.useEffect(), not when your component is first rendered," indicating the intended redirect-to-login logic is broken and never completes, leaving the user stuck on a blank page rather than a usable sign-in screen.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match actual replies listed
    - Actual: thread-6 shows thread.reply-count "89 replies" but its detail view "Replies" heading and rendered list show only 1 reply. thread-1 shows thread.reply-count "33 replies" (after adding 10 replies via UI to the seeded 23) but its "Replies" heading/list shows only 12 (2 seeded + 10 added). The reply-count badge is a static seed number decoupled from the actual reply list, so it does not match the number of replies listed on the detail view for pre-seeded threads.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; ranking is not restricted to recent activity
    - Actual: The "Trending Discussions" and "Most Active Discussions" views only show the generic subtitle "Join the conversation with our community" — no mention of a time window (e.g. "last 7 days", "past 30 days"). Both lists include threads whose timestamp reads "over 2 years ago" (e.g. thread-6, thread-4, thread-7, thread-1) ranked at the top, showing ranking is computed over all-time upvotes/replies rather than any recent window. There is no UI element or copy indicating what "recent" period is used.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Community/category counts do not update when new records are created
    - Actual: stats.threads remained "1,045" after creating multiple new threads (including "Persistence Test Discussion" just created, visible in the list). stats.members remained "892" after registering 5+ new accounts (qatester1 plus 4 throwaway voter accounts) during this session. stats.replies remained "4,823" despite adding 12+ new replies across threads. Category sidebar count "General Discussion (156)" did not increment after publishing a new thread into that category. All headline/category counts are static and do not reflect actual created content.