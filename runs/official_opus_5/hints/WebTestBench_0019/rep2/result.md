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
    - Issue: No persistence across page reload; all user data and session are lost
    - Actual: After reloading the app (navigating to http://localhost:7019/thread/thread-3), the header reverted to 'Login | Register' (session lost), thread-3's reply count reset from 35 back to 31 with the reply list showing 'No replies yet.', the upvote/reply controls were disabled with 'You need to be logged in to reply', and the home thread list contained only the 8 seeded threads - both threads authored by QaTester01 were gone. localStorage and sessionStorage are both empty, so the registered account, profile edits, threads, replies and votes exist only in memory.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not validated at sign-in; any password is accepted
    - Actual: Registered QaTester01 with password 'secret123', logged out, then signed in with email qatester01@example.com and deliberately wrong password 'wrongpass999'. Login succeeded: redirected to home page and nav showed authenticated state (nav.profile = 'QaTester01', nav.logout and nav.new-thread present). No error was shown and no rejection occurred.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: Unauthenticated discussion creation renders a blank page instead of a usable sign-in screen
    - Actual: While logged out there is no 'New Thread' entry point at all (header only offers Login/Register and the home page has no start-discussion button). Navigating to /new-thread as an unauthenticated user renders a completely empty page: document.body.innerText is '' and the accessibility snapshot contains only the toast notification region. No sign-in screen, no redirect to /login, and no way to return to discussion creation after authenticating.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match the replies listed on the detail view
    - Actual: thread-3 ('Digital art vs traditional art') showed 'thread.reply-count = 31 replies' in the list and on its detail view, yet the detail view's reply section showed the empty state 'No replies yet. Be the first to respond!' (0 replies listed). After posting one reply the count became '32 replies' while exactly 1 reply is listed. The seeded counts are fabricated numbers unrelated to actual reply records; the same mismatch applies to every seeded thread (e.g. thread-1 '23 replies', thread-6 '89 replies').

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window for Trending / Most Active; ranking uses all-time totals
    - Actual: Headings are only 'Trending Discussions' and 'Most Active Discussions' with the subtitle 'Join the conversation with our community'. A full-page text scan for 'week', 'day', '24 hours', 'today', 'month' found no time-window statement anywhere in these views. Ranking is computed from each thread's lifetime upvote total and lifetime reply total (e.g. thread-6 with 156 upvotes / 89 replies stays first regardless of when those interactions occurred), so no windowed filtering exists.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are static fabricated numbers that neither match actual records nor update on creation
    - Actual: Community Stats stayed at Threads 1,045 / Replies 4,823 / Members 892 before and after registering a new member, creating 1 thread and posting 4 replies. Actual forum content is 9 threads, 4 replies and (at least) 9 members. Category counts are equally wrong and static: Technology shows (243) while /category/tech lists 3 threads (still 243 after adding a tech thread); Gaming shows (312) while /category/gaming lists 1 thread.