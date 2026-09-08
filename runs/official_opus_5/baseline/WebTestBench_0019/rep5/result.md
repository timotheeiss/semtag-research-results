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
    - Issue: No persistence across page reload — all state is in-memory only
    - Actual: After reloading http://localhost:6019/: header showed Login/Register (session lost), both created threads ('QA Test Thread Alpha/Beta') disappeared, thread-1 upvote count reverted to 47 and the posted reply was gone. localStorage and sessionStorage are both empty. Logging in again with the registered qauser1@example.com / testpass123 was rejected with 'Invalid credentials. Try: tech@example.com', so the account itself did not survive the reload.


## Constraint
- [X] CS-14: A discussion cannot be published without a category, non-blank title, and non-blank body; the missing information is identified and no discussion is created.

- [ ] CS-17: An account can be signed in only with its registered password, and an incorrect password is rejected without creating a logged-in session.
  - Bug Report:
    - Issue: Password not validated at sign-in
    - Actual: Signed in at /login with qauser1@example.com and the wrong password 'wrongpass999' (registered password was 'testpass123'); app redirected to / and header showed logged-in user 'qauser1' with New Thread link.

- [ ] CS-22: An unauthenticated user who attempts to start a discussion is taken to a usable sign-in screen and can return to discussion creation after authenticating.
  - Bug Report:
    - Issue: No sign-in redirect for unauthenticated thread creation; /new-thread renders a blank page
    - Actual: Logged out, the UI offers no way to start a discussion (header shows only Login/Register; no 'New Thread' link or CTA anywhere on the home page). Opening /new-thread directly renders an empty document (body innerText is '', only the notifications region in the accessibility tree) — no sign-in screen, no redirect, and no path back to discussion creation.


## Content
- [X] CT-16: The Trending discussion list is ordered by upvote count from highest to lowest and updates its order when voting changes the ranking.

- [ ] CT-19: Every discussion's displayed reply total matches the number of replies listed on its detail view, and adding a reply keeps both totals consistent.
  - Bug Report:
    - Issue: Displayed reply total does not match replies listed on detail view
    - Actual: Thread 'What programming language should I learn in 2024?' showed '23 replies' but the detail view listed only 2 replies ('Replies (2)'). After posting one reply the header showed '24 replies' while the list showed 'Replies (3)' — the two totals stay 21 apart.

- [ ] CT-20: Trending and most-active views that describe recent popularity use a stated recent time window and rank discussions only by interactions within that window.
  - Bug Report:
    - Issue: No stated recent time window; ranking uses all-time totals
    - Actual: 'Trending Discussions' and 'Most Active Discussions' show only the subtitle 'Join the conversation with our community' — no time window (e.g. 'past 7 days') is stated anywhere. Rankings are all-time: top entries are threads dated 'over 2 years ago' ranked purely by lifetime upvote/reply totals, with no restriction to interactions in a recent period.

- [ ] CT-21: Category and community counts accurately reflect the discussions, replies, and members available in the forum and update when those records are created.
  - Bug Report:
    - Issue: Category and community counts are hardcoded, not derived from data
    - Actual: Sidebar shows Technology (243) while /category/tech lists only 3 threads; Community Stats show 1,045 Threads / 4,823 Replies / 892 Members although the forum contains 9 threads and 6 users. After registering a new member and creating a new Technology thread, all counts remained unchanged (243, 1,045, 892).